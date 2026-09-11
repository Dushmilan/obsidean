# Async JavaScript: Promises & async/await

JavaScript handles concurrency with **async/await** over **Promises**. A Promise is a *future value* — a placeholder for a result that arrives later. `async` functions always return Promises; `await` pauses them without blocking the thread. Getting the mental model right (promises vs callbacks, error propagation, parallelism vs sequencing) is the core skill of modern Node.

**The Intuition:** Ordering food at a restaurant: the waiter gives you a *ticket* (the Promise). You don't have the food yet, but you hold something that will *become* the food. You can do other things while waiting. When you `await`, you're saying "I'll sit here until my order's ready — but the kitchen keeps cooking everyone else's food."

## The three states

```text
Promise states:
┌─────────┐    resolve()   ┌──────────┐
│ pending │──────────────► │ fulfilled│
└─────────┘                └──────────┘
     │
     │ reject()
     ▼
┌──────────┐
│ rejected │
└──────────┘
```

A promise settles **once** — either fulfilled with a value or rejected with a reason. Everything after is immutable.

## From callbacks to promises

```js
// Callback style — nesting grows fast:
fs.readFile('a.txt', (err, a) => {
  if (err) throw err;
  fs.readFile('b.txt', (err2, b) => {
    if (err2) throw err2;
    console.log(a + b);
  });
});

// Promise style — flat chain:
const read = (file) => fs.promises.readFile(file, 'utf8');

read('a.txt')
  .then(a => read('b.txt').then(b => a + b))   // still nested a bit
  .then(result => console.log(result))
  .catch(err => console.error(err));

// async/await — reads like synchronous code:
async function main() {
  const a = await read('a.txt');
  const b = await read('b.txt');
  console.log(a + b);
}
main().catch(err => console.error(err));
```

**The progression:** callbacks → `.then` chains → `async/await`. The logic stays the same; the *shape* gets flatter and more readable.

## Creating promises

```js
// Wrap a callback-style API:
function readFileAsync(file) {
  return new Promise((resolve, reject) => {
    fs.readFile(file, 'utf8', (err, data) => {
      if (err) reject(err);        // → catch
      else resolve(data);          // → then
    });
  });
}

// The executor runs immediately; resolve/reject are one-shot.
```

Modern Node: prefer the built-in promise APIs — `fs.promises.*`, `node:timers/promises`, `util.promisify` — over hand-rolling wrappers.

## Error propagation

```js
async function fetchUser(id) {
  const res = await fetch(`/api/users/${id}`);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);   // reject the promise
  return res.json();
}

// Every throw inside an async fn rejects its returned promise.
// Every await unwraps the promise and throws on rejection.

async function loadProfile(id) {
  try {
    const user = await fetchUser(id);
    return user;
  } catch (err) {
    // runs for BOTH fetchUser's throws and any HTTP/network error
    console.error('profile load failed:', err.message);
    throw err;            // re-throw to let the caller decide
  }
}
```

**The rules:**
- `throw` in an async fn ⇔ reject
- `return` in an async fn ⇔ resolve
- `await` throws if the awaited promise rejects
- `try/catch` around `await` catches async errors exactly like sync ones

## Parallelism vs sequencing

```js
// SEQUENTIAL — waits for a, then starts b (slow, but ordered):
async function sequential() {
  const a = await fetch('/api/a');
  const b = await fetch('/api/b');     // starts AFTER a resolves
  return [a, b];
}

// PARALLEL — start both, wait for both (fast when independent):
async function parallel() {
  const [a, b] = await Promise.all([
    fetch('/api/a'),
    fetch('/api/b'),                   // starts immediately, not after a
  ]);
  return [a, b];
}
```

**`Promise.all`** — all-or-nothing: rejects as soon as *any* input rejects (fast-fail).
**`Promise.allSettled`** — waits for all, reports each as fulfilled/rejected (no fast-fail — for partial success).
**`Promise.race`** — settles with the *first* settled promise (timeouts).
**`Promise.any`** — settles with the first *fulfilled* (ignores rejections until all fail).

```js
// Timeout pattern — race against a timer:
async function fetchWithTimeout(url, ms = 5000) {
  const controller = new AbortController();
  const timer = setTimeout(() => controller.abort(), ms);
  try {
    return await fetch(url, { signal: controller.signal });
  } finally {
    clearTimeout(timer);
  }
}
```

## Common async patterns

```js
// Retry with backoff:
async function fetchWithRetry(url, { retries = 3, delayMs = 500 } = {}) {
  for (let attempt = 1; attempt <= retries; attempt++) {
    try {
      return await fetch(url);
    } catch (err) {
      if (attempt === retries) throw err;
      await new Promise(r => setTimeout(r, delayMs * attempt));   // 500, 1000, 1500
    }
  }
}

// Sequential map (slow but controlled — e.g., rate-limited API):
async function processAll(items) {
  const results = [];
  for (const item of items) {           // for/await — one at a time
    results.push(await process(item));
  }
  return results;
}
```

---

**Setup:** Fetch two user profiles and render both — fast and independent.

**Solution:**
```js
async function getProfiles(ids) {
  const [a, b] = await Promise.all(ids.map(id => fetch(`/api/users/${id}`).then(r => r.json())));
  return { a, b };
}
```

**Key insight:** mapping to promises and awaiting with `Promise.all` starts all fetches at once. If you `await` inside the `map` callback you'd get sequential — that's the classic "why is my code slow?" bug.

---

**Setup:** A script that processes files and must keep going even if one fails.

**Solution:**
```js
const results = await Promise.allSettled(files.map(read));

for (const r of results) {
  if (r.status === 'fulfilled') {
    console.log('ok:', r.value);
  } else {
    console.warn('failed:', r.reason.message);   // no throw — continue
  }
}
```

**Key insight:** `allSettled` never rejects — every outcome is reported. Perfect for batch jobs where one bad file shouldn't kill the whole run. With `Promise.all`, one rejection would skip every subsequent result.

---

**Setup:** Why doesn't `await` block the event loop, even in a busy server?

**Solution:** `await` *suspends* the current async function and returns control to the event loop — the thread is free to run other handlers, timers, and I/O. When the awaited promise settles, the continuation is queued as a microtask and resumed later. Nothing is held hostage; the "waiting" happens in the event loop, not on the thread.

**Key insight:** async/await is *cooperative multitasking on one thread*. You yield at each `await`; the loop fills the gap with other work. That's why thousands of concurrent `await`-ing requests don't starve the server.

---

## Practice (try before peeking)

1. `Promise.all` vs `Promise.allSettled` — pick for a batch job, and why?
2. What happens when you `await` a promise that rejects — and how do you catch it?
3. Three fetches that don't depend on each other — parallel or sequential, and how?

<details><summary>Answers</summary>

1. `allSettled` — a batch job should process what it can and report failures, not die on the first one. `all` fast-fails, which is right when the whole operation is meaningless if any part fails (e.g., a transaction-like load).
2. `await` throws the rejection reason in that function — caught by a surrounding `try/catch`, or it becomes an unhandled rejection if nothing catches it. Always handle: unhandled rejections crash Node processes (or at least log loudly).
3. Parallel — `Promise.all([fetch(a), fetch(b), fetch(c)])`. Sequential `await`s in a row waste the wait time; they only belong when each step needs the previous result.

</details>

---

**Common traps:**
- `await` in a `map` callback → sequential instead of parallel
- Missing `try/catch` around `await` — unhandled rejections
- Forgetting `await` entirely — using the promise object instead of its value
- `Promise.all` on a large array → many simultaneous requests; add concurrency limiting
- Swallowing errors with empty `catch {}` — at minimum log them

---
