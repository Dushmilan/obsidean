# Error Handling & Debugging

Errors in Node come in two flavors: **operational** (bad input, network down, file missing — expected, handle them) and **programmer** (bugs — crash loudly, fix them). Good error handling means: async errors never escape silently, every error carries context, and the process stays alive for what it can recover from.

**The Intuition:** An error handler is a safety net with levels. The first net is *right where the work happens* (try/catch in the handler — you know the context). The second is at the *app boundary* (Express error middleware — turns errors into HTTP responses). The last is the *process* net (unhandled rejections) — catch what slipped through, log it, and decide whether the process can keep going.

## The two error classes

```js
// OPERATIONAL — expected, recoverable:
try {
  const config = await readFile('config.json', 'utf8');
} catch (err) {
  if (err.code === 'ENOENT') {
    console.warn('no config — using defaults');   // handle gracefully
  } else {
    throw err;                                     // unexpected — escalate
  }
}

// PROGRAMMER — a bug: undefined(), null.field, wrong type
// These should crash in dev and be fixed — not papered over.
```

**The rule:** distinguish them. Operational errors get graceful handling (retry, fallback, 4xx/5xx response). Programmer errors should surface loudly in development — silent `catch {}` hides bugs.

## Error objects — carry context

```js
// Make errors self-describing:
class AppError extends Error {
  constructor(status, code, message, details) {
    super(message);
    this.status = status;
    this.code = code;
    this.details = details;
    this.name = 'AppError';
  }
}

// Usage:
function createUser(input) {
  if (!input.email) throw new AppError(400, 'VALIDATION', 'Email is required');
  throw new AppError(409, 'DUPLICATE', 'Email already exists');
}
```

**The intuition:** a bare `Error('boom')` tells you *something* failed. An `AppError` with `status`, `code`, and `details` tells a client (and a logger) exactly what failed and why — the difference between "500, good luck" and "409 DUPLICATE on field email."

## Async error handling

```js
// Express 4 — async handlers do NOT catch rejections automatically:
app.get('/api/things', async (req, res, next) => {
  try {
    const things = await db.findThings();
    res.json(things);
  } catch (err) {
    next(err);                    // → error middleware
  }
});

// Cleaner: a wrapper that forwards rejections:
const asyncHandler = (fn) => (req, res, next) =>
  Promise.resolve(fn(req, res, next)).catch(next);

app.get('/api/things', asyncHandler(async (req, res) => {
  const things = await db.findThings();
  res.json(things);
}));
```

**The danger:** an async handler that rejects without `next(err)` *drops* the error — the client hangs (no response) and the rejection may go unhandled. Express 5 catches this automatically; on Express 4, wrap.

## Unhandled rejections & the process

```js
// A promise that rejects and nobody catches:
fetch('/api/x').then(r => r.json());   // if it rejects → unhandled

// Node's default: CRASH (unhandled rejection → process exits nonzero)

// If you must survive, log and exit anyway — don't limp along:
process.on('unhandledRejection', (reason) => {
  console.error('UNHANDLED REJECTION:', reason);
  process.exit(1);     // fail fast — the state is unknown
});

process.on('uncaughtException', (err) => {
  console.error('UNCAUGHT EXCEPTION:', err);
  process.exit(1);     // restart via the process manager (PM2/Docker/systemd)
});
```

**The intuition:** an unhandled rejection means *something* you awaited was supposed to handle an error and didn't — the app's state is now unpredictable (a connection half-open, data half-written). Failing fast and letting the supervisor restart is safer than continuing with corrupted state.

## Logging — structured, not console soup

```js
// Instead of scattered console.log, log structured JSON (pino is fast):
import pino from 'pino';
const logger = pino();

app.get('/api/users/:id', async (req, res) => {
  logger.info({ userId: req.params.id, path: req.url }, 'fetch user');
  try {
    const user = await db.find(req.params.id);
    res.json(user);
  } catch (err) {
    logger.error({ err, userId: req.params.id }, 'failed to fetch user');
    res.status(500).json({ error: 'Internal error' });
  }
});
```

**The intuition:** `logger.error({ err, context }, 'message')` captures the stack, the request context, and the message in one queryable record. When you're debugging production, "give me all errors for userId=7" is only possible if context is in the logs.

## Debugging toolbox

```bash
# 1. Logging — structured + levels (fatal, error, warn, info, debug, trace)

# 2. Node's built-in inspector (Chrome DevTools):
node --inspect src/index.js
# open chrome://inspect — debugger, breakpoints, heap snapshots

# 3. Breakpoint debugging with the inspector CLI:
node --inspect-brk src/index.js     # pause on first line

# 4. Trace the event loop / blocking:
node --trace-warnings src/index.js

# 5. Find CPU hogs in production:
node --cpu-prof src/index.js        # writes a .cpuprofile you can open
```

```js
// 6. assert for invariants — fail fast on impossible states:
import assert from 'node:assert/strict';
assert.ok(user, 'user must exist before use');
assert.equal(total, items.length, 'count mismatch');
```

---

**Setup:** Centralize error responses so every route returns the same shape.

**Solution:**
```js
app.use((err, req, res, next) => {
  const status = err.status || 500;
  const code = err.code || 'INTERNAL_ERROR';

  if (status >= 500) logger.error({ err }, err.message);   // server bugs — log loud
  else logger.warn({ err, url: req.url }, err.message);    // client errors — quieter

  if (res.headersSent) return next(err);   // response already started — let Express close it

  res.status(status).json({ error: { code, message: err.message, details: err.details } });
});
```

**Key insight:** one handler, three decisions — log (5xx loudly), avoid double-send (`headersSent` check), and respond with a uniform envelope. Every route that calls `next(err)` or throws lands here automatically.

---

**Setup:** Retry a flaky external API with backoff, distinguishing transient from permanent failures.

**Solution:**
```js
async function fetchWithRetry(url, { retries = 3, baseDelay = 200 } = {}) {
  for (let attempt = 1; attempt <= retries; attempt++) {
    try {
      const res = await fetch(url);
      if (res.ok) return await res.json();
      if (res.status >= 400 && res.status < 500) {
        throw new AppError(res.status, 'API_REJECTED', `API said ${res.status}`);  // permanent — don't retry
      }
    } catch (err) {
      if (attempt === retries || err instanceof AppError) throw err;   // give up
      const delay = baseDelay * 2 ** (attempt - 1);                    // 200, 400, 800
      logger.warn({ url, attempt, delay }, 'retrying');
      await new Promise(r => setTimeout(r, delay));
    }
  }
}
```

**Key insight:** 4xx = client/API said no — retrying won't help. 5xx/network = transient — back off exponentially. Classifying errors into retryable vs not is the difference between a resilient service and one that hammers a failing API.

---

**Setup:** Why does the process crash on an unhandled rejection — and is that the right behavior?

**Solution:** An unhandled rejection means code that should have handled an error didn't — a resource (DB connection, file handle, in-flight response) is left in an unknown state. Node crashes by design so the state doesn't silently corrupt. The supervisor (PM2/Docker) restarts a clean process. Crash-with-restart beats limp-along-with-corruption.

**Key insight:** the fix is *not* a global `unhandledRejection` handler that swallows — it's finding and handling the error at its source. The global handler is for logging + controlled exit, not for pretending nothing happened.

---

## Practice (try before peeking)

1. Operational vs programmer errors — give an example of each and how to treat it.
2. Why must async handlers forward errors to `next(err)` on Express 4?
3. When is `process.exit(1)` in a global handler the right call?

<details><summary>Answers</summary>

1. Operational: file missing (`ENOENT`), network timeout, bad input — handle gracefully (fallback, retry, 4xx/5xx response). Programmer: `undefined.x`, wrong argument type — a bug; crash loudly in dev and fix it, don't swallow.
2. A rejected promise in an async handler, if not forwarded, leaves the client with no response and the error silently dropped — it never reaches the error middleware. `next(err)` routes it there so the client gets a proper 500 and the logger sees it.
3. When the error indicates unknown/corrupted state — an unhandled rejection or uncaught exception after resources are in flight. Exit so the supervisor restarts cleanly. Don't exit for handled, recoverable operational errors — those shouldn't reach the global handlers.

</details>

---

**Common traps:**
- Empty `catch {}` — hides bugs silently; at minimum log the error
- Not distinguishing 4xx (client) from 5xx (server) — wrong status codes confuse consumers
- Async handlers that don't forward errors — hanging clients + unhandled rejections
- `console.log` everywhere with no structure — impossible to search in production
- Swallowing errors in a `finally` that overrides the original error

---
