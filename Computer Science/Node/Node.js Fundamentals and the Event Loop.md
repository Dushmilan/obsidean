# Node.js Fundamentals & the Event Loop

Node.js is a **JavaScript runtime** built on Chrome's V8 engine. Its superpower is **non-blocking I/O**: one thread handles thousands of concurrent connections by *delegating* slow work (files, network, databases) and continuing. The **event loop** is the scheduler that makes this possible — understanding it is understanding Node.

**The Intuition:** A traditional server is a single cashier who serves one customer at a time — a slow customer (waiting for their coffee) blocks everyone. Node is a cashier who takes orders, hands the ticket to the kitchen, and serves the next customer *while the kitchen works*. The kitchen (OS threads, libuv thread pool) does the slow work; the cashier just checks when orders are ready.

## What Node actually is

```text
┌───────────────────────────────┐
│  Your JavaScript code         │
├───────────────────────────────┤
│  V8 — compiles & runs JS      │
├───────────────────────────────┤
│  Node.js APIs (fs, http, ...) │
├───────────────────────────────┤
│  libuv — event loop + thread  │
│  pool (for files, DNS, crypto)│
└───────────────────────────────┘
```

- **V8** — the JS engine (same one in Chrome)
- **libuv** — the C library providing the event loop, the thread pool, and non-blocking OS operations
- **Node APIs** — `fs`, `http`, `path`, `crypto`, `stream`, etc.

## The single-threaded model

```js
// This LOOKS slow, but the loop keeps running while the file loads:
const fs = require('fs');

fs.readFile('big.txt', (err, data) => {   // async — handed to libuv
  console.log('file ready');               // runs LATER, when data is in
});

console.log('first');                      // runs immediately
console.log('second');                     // runs immediately

// Output order:
//   first
//   second
//   file ready
```

The process has **one main thread** for running JS. Slow operations are offloaded (libuv thread pool or OS async APIs), and the callback fires when they finish. Your JS never blocks waiting — *blocking the loop* (heavy `for` loops, synchronous `fs.readFileSync`) is the cardinal sin.

## The event loop — the phases

```text
   ┌───────────────────────────┐
┌─►│         timers            │  setTimeout / setInterval callbacks
│  └────────────┬──────────────┘
│  ┌────────────┴──────────────┐
│  │     pending callbacks     │  I/O callbacks deferred
│  └────────────┬──────────────┘
│  ┌────────────┴──────────────┐
│  │ idle, prepare             │  internal
│  └────────────┬──────────────┘
│  ┌────────────┴──────────────┐
│  │          poll            │  ★ I/O events — where most work happens
│  └────────────┬──────────────┘
│  ┌────────────┴──────────────┐
│  │          check           │  setImmediate callbacks
│  └────────────┬──────────────┘
│  ┌────────────┴──────────────┐
│  │     close callbacks       │  socket/server close
│  └───────────────────────────┘
└───────────── every iteration
```

Each iteration processes callbacks by phase. Between phases, the **microtask queue** (promises, `queueMicrotask`) drains first. This ordering is why promise callbacks beat timers:

```js
setTimeout(() => console.log('timer'), 0);
Promise.resolve().then(() => console.log('promise'));

// promise  ← microtasks run first
// timer
```

## Microtasks vs macrotasks

```text
Macrotasks (one per loop phase):  setTimeout, setInterval, setImmediate, I/O callbacks
Microtasks (drain between each):  Promise.then/catch/finally, queueMicrotask, async/await continuations

Promise callbacks ALWAYS run before the next macrotask.
```

```js
fs.readFile('a.txt', () => console.log('A'));
Promise.resolve().then(() => console.log('P1'));
process.nextTick(() => console.log('NT'));

// NT      — nextTick runs before the loop even continues
// P1      — microtask queue drains
// A       — I/O callback in the poll phase
```

**`process.nextTick`** is a special queue that runs *before* the event loop continues — use it sparingly (mostly inside libraries). Heavy `nextTick` usage can starve I/O.

## The thread pool (the hidden parallel workers)

```js
// These use libuv's thread pool (default 4 threads):
fs.readFile / fs.writeFile / fs.stat
crypto.pbkdf2 / crypto.randomBytes (some)
zlib compression
DNS lookups (dns.lookup)

// These do NOT (true OS async — no pool):
fs.* with these operations? no — network sockets, http, net
```

You can raise the pool with `UV_THREADPOOL_SIZE=8`. The pool is why "single-threaded" Node still does file work concurrently — the *JS* is single-threaded, but *I/O* isn't.

## Blocking vs non-blocking — spot the difference

```js
// BLOCKING — freezes the loop, everything waits:
const data = fs.readFileSync('big.txt', 'utf8');   // loop is STUCK until read done
doWork();                                          // runs after

// NON-BLOCKING — loop keeps serving while I/O runs:
fs.readFile('big.txt', 'utf8', (err, data) => {    // callback later
  doWork();
});
```

**When sync is OK:** top-level startup (config load), one-off scripts, tiny reads where the cost is negligible. **When it's fatal:** inside request handlers, inside loops, anywhere the loop must stay free.

---

**Setup:** A server that stays responsive during a slow database call.

**Solution:**
```js
const http = require('http');
const { queryDb } = require('./db');       // returns a Promise

const server = http.createServer(async (req, res) => {
  if (req.url === '/slow') {
    const rows = await queryDb('SELECT ...');   // await = yield the loop
    res.end(JSON.stringify(rows));
  } else {
    res.end('fast path');
  }
});

server.listen(3000);
// While /slow waits on the DB, /still works — the loop was never blocked.
```

**Key insight:** `await` inside the handler *yields* the single thread back to the event loop. The DB call runs off-thread; meanwhile the loop accepts and serves other requests. This is the entire scalability story of Node in one pattern.

---

**Setup:** Order the output of mixed timers, promises, and I/O callbacks.

**Solution:**
```js
setTimeout(() => console.log(1), 0);
setImmediate(() => console.log(2));
Promise.resolve().then(() => console.log(3));
fs.readFile('x.txt', () => console.log(4));
process.nextTick(() => console.log(5));

// 5  (nextTick — before anything)
// 3  (microtask — drains before timers/poll)
// 1  (timer phase)
// 2  (check phase — after poll)
// 4  (poll I/O callback)
```

**Key insight:** `nextTick` > microtasks > timers > poll I/O > `setImmediate`. Note `setImmediate` isn't "immediate" — it's *after* I/O in the check phase. (Inside the poll phase, I/O callbacks queue; `setImmediate` runs right after that batch.)

---

**Setup:** Why does a CPU-heavy loop freeze all server requests?

**Solution:** The loop can only run JS on its single thread. A long synchronous computation (`for` loop, JSON parse of a huge string, `readFileSync`) *occupies* that thread — no phase advances, no I/O completes, no timer fires until it returns. Fix: move heavy work to a worker thread (`worker_threads`), a child process, or an external service.

**Key insight:** Node's scalability comes from *not* doing CPU work on the main thread. Offload anything hot. The event loop is only as fast as your slowest synchronous block.

---

## Practice (try before peeking)

1. What does "single-threaded, non-blocking" actually mean for concurrency?
2. Order: `Promise.resolve().then(fn)` vs `setTimeout(fn, 0)` — who runs first and why?
3. `fs.readFileSync` in a request handler — why is that bad?

<details><summary>Answers</summary>

1. Your JavaScript runs on one thread — there's no parallel JS. But I/O (files, network, DB) is delegated to libuv/the OS and callbacks come back to the loop. Thousands of *waiting* connections cost nothing while they wait; only *executing* JS occupies the thread.
2. The promise callback — microtasks drain completely between macrotasks. `setTimeout(0)` schedules a timer in the *timer phase* of the next loop iteration, which only runs after the microtask queue is empty.
3. It blocks the single thread for the whole read duration — every other request, timer, and I/O callback stalls. In a server, that's a single point of failure; use the async version and let the loop breathe.

</details>

---

**Common traps:**
- `readFileSync` / `readdirSync` in handlers — blocks the entire server
- CPU-heavy loops on the main thread (parsing, hashing big data) — freeze everything
- Assuming promises run "in parallel" — they don't; they *defer* on one thread
- `process.nextTick` in a recursive loop — starves the event loop
- Forgetting `UV_THREADPOOL_SIZE` — heavy `fs`/`crypto` share only 4 pool threads by default

---
