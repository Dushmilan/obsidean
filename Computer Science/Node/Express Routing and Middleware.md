# Express: Routing & Middleware

Express is the classic Node web framework. Its two abstractions — **routing** (map methods+paths to handlers) and **middleware** (a chain of functions that process the request in order) — cover almost everything a server does. The middleware pipeline is the heart: every request flows through a stack, and each layer can modify, short-circuit, or pass it on.

**The Intuition:** Middleware is an assembly line. Each station (function) can inspect or modify the item (request), decide it's done and ship it (send a response), or pass it to the next station (`next()`). The route handler is the final station. Order matters — a station that must check everything (auth, logging) goes early; a station that needs the result goes later.

## The middleware pipeline

```js
import express from 'express';
const app = express();

// Middleware 1 — runs for EVERY request:
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} — ${new Date().toISOString()}`);
  next();                    // pass to the next layer
});

// Middleware 2 — body parsing:
app.use(express.json());     // parses JSON bodies into req.body

// Route handler — the final station:
app.get('/hello', (req, res) => {
  res.json({ message: 'Hello!' });
});

app.listen(3000);
```

**The three roles of `next()`:**
- `next()` — move to the next middleware
- `next('route')` — skip to the next matching *route*
- `next(err)` — jump to the **error handler** (skips all normal middleware)

## Routing

```js
// Methods & paths:
app.get('/users', listUsers);
app.post('/users', createUser);
app.put('/users/:id', updateUser);
app.delete('/users/:id', deleteUser);

// Path parameters:
app.get('/users/:id', (req, res) => {
  const id = req.params.id;      // '/users/7' → id = '7' (string!)
  res.json({ id });
});

// Query strings:
app.get('/search', (req, res) => {
  const q = req.query.q;         // '/search?q=react' → 'react'
  const page = Number(req.query.page ?? 1);
  res.json({ q, page });
});

// Multiple handlers per route — route-level middleware:
app.get('/admin', requireAuth, adminOnly, renderAdmin);
```

**Router instances** split the app into modules:

```js
// routes/users.js
import { Router } from 'express';
const router = Router();

router.get('/', listUsers);
router.get('/:id', getUser);

export default router;

// app.js
import usersRouter from './routes/users.js';
app.use('/api/users', usersRouter);   // mounts at /api/users
```

## Built-in & third-party middleware

```js
app.use(express.json());              // JSON body → req.body
app.use(express.urlencoded({ extended: true }));  // form bodies
app.use(express.static('public'));    // serve static files

import cors from 'cors';
app.use(cors());                      // allow cross-origin requests

import helmet from 'helmet';
app.use(helmet());                    // security headers
```

## Error handling

```js
// Async handlers — Express 5 catches rejected promises automatically.
// (Express 4: wrap manually with a try/catch or a helper.)

app.post('/api/users', async (req, res, next) => {
  const user = await createUser(req.body);   // rejection → next(err) automatically
  res.status(201).json(user);
});

// The error middleware — FOUR params, must come LAST:
app.use((err, req, res, next) => {
  console.error(err);
  const status = err.status || 500;
  res.status(status).json({ error: err.message });
});

// 404 catcher — anything that reached here matched nothing:
app.use((req, res) => {
  res.status(404).json({ error: 'Not found' });
});
```

**Error middleware signature matters:** exactly 4 params `(err, req, res, next)` — Express identifies it by `err.length === 4`. Errors thrown/rejected in routes land here.

## The response methods

```js
res.send('text');                      // plain text / html
res.json({ ok: true });                // JSON with proper content-type
res.status(201).json(created);         // status + body chain
res.redirect('/login');                // 302 + Location
res.sendStatus(204);                   // status only, no body
res.setHeader('X-Powered-By', '...');  // arbitrary header
```

## Middleware ordering — the subtle rules

```js
// BAD — json() AFTER the routes that need it:
app.get('/api', (req, res) => res.json({ body: req.body }));   // req.body = undefined
app.use(express.json());    // too late — GET already handled it

// GOOD — parsing early, protection early, routes after:
app.use(helmet());
app.use(express.json());
app.use(cors());
app.use('/api/users', usersRouter);
app.use(notFound);
app.use(errorHandler);
```

**The rule:** middleware applies only to requests that reach it. Anything registered before a short-circuiting route never runs for that request.

---

**Setup:** A request logger + auth guard as middleware.

**Solution:**
```js
// logger middleware:
app.use((req, res, next) => {
  const start = Date.now();
  res.on('finish', () => {
    console.log(`${req.method} ${req.url} ${res.statusCode} ${Date.now() - start}ms`);
  });
  next();
});

// auth middleware (protect a route):
function requireAuth(req, res, next) {
  const token = req.headers.authorization?.replace('Bearer ', '');
  if (!isValid(token)) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  req.user = decodeToken(token);      // attach to req for downstream handlers
  next();
}

app.get('/profile', requireAuth, (req, res) => {
  res.json({ user: req.user });
});
```

**Key insight:** middleware attaches context to `req` (like `req.user`) and the next layers read it — that's how auth, rate-limiting, and tenant resolution work across routes without duplication.

---

**Setup:** A router for posts, mounted under `/api/posts`, with a `:id` route.

**Solution:**
```js
// routes/posts.js
const router = Router();

router.get('/', (req, res) => {
  res.json([{ id: 1, title: 'Hello' }]);
});

router.post('/', async (req, res, next) => {
  try {
    const post = await savePost(req.body);
    res.status(201).json(post);
  } catch (err) {
    next(err);                    // hand to the error middleware
  }
});

router.get('/:id', (req, res) => {
  res.json({ id: req.params.id });
});

// app.js
app.use('/api/posts', postsRouter);
```

**Key insight:** `:id` becomes `req.params.id`. Mounting at `/api/posts` means the router's `/` is actually `/api/posts/` — paths compose, keeping each module self-contained.

---

**Setup:** Why is the error handler the last thing registered?

**Solution:** Error middleware only runs for errors *passed to it* (`next(err)` or a thrown/rejected error). Routes and normal middleware registered *after* it would run before it gets a chance — and their errors would have no handler. Last position means it catches everything that came before.

**Key insight:** order = priority. Middleware before routes (parsing, auth, logging), routes in the middle, 404 + error handler at the end. That single ordering rule produces well-behaved Express apps.

---

## Practice (try before peeking)

1. What does `next(err)` do differently from `next()`?
2. Why does `app.use(express.json())` need to come before the routes that read `req.body`?
3. How does Express recognize error-handling middleware?

<details><summary>Answers</summary>

1. `next(err)` skips all remaining normal middleware/routes and jumps straight to the error handler (the 4-param middleware). `next()` continues to the next layer in the normal chain.
2. Because middleware applies only to requests that pass through it. A route registered before `express.json()` handles the request before parsing happens, so `req.body` is never populated.
3. By its signature — exactly four parameters `(err, req, res, next)`. Express checks `fn.length === 4`. If you write three params, it's treated as a normal (non-error) middleware.

</details>

---

**Common traps:**
- Registering routes before `express.json()`/`cors()`/`helmet()` — they never apply
- Error middleware with only 3 params — silently treated as normal middleware
- Swallowed promise rejections in async handlers (Express 4 needs a wrapper)
- `next()` called after `res.end()` — double-send errors
- Forgetting the 404 catcher and error handler entirely — unhandled routes return Express's default HTML

---
