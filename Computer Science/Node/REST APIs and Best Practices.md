# REST APIs & Best Practices

REST is a *style* for APIs: resources identified by URLs, manipulated with standard HTTP methods, and described with status codes. Beyond the basics, the craft is in **naming, validation, pagination, versioning, consistent error shapes**, and **idempotency** — the habits that make an API predictable to consume.

**The Intuition:** REST treats your data like a set of files on a web server. `GET /users` lists, `POST /users` creates, `GET /users/7` reads one, `PUT /users/7` replaces, `PATCH /users/7` updates a field, `DELETE /users/7` removes. The URL names the *thing*; the method names the *action*. If your API reads like a table of contents, it's probably restful.

## Resources & methods

```text
Collection          Item
─────────           ─────
GET    /users       → list (often paginated)
POST   /users       → create (201 + Location)
GET    /users/7     → read one (200)
PUT    /users/7     → replace (200/204)
PATCH  /users/7     → partial update (200)
DELETE /users/7     → remove (200/204/404)

Nested:  GET /users/7/posts       posts by user 7
         POST /users/7/posts      create a post by user 7
```

**Key conventions:**
- Nouns, plural, lowercase: `/users`, not `/getUser` or `/User`
- **No actions in URLs** — `POST /users/7/activate` is a smell; prefer `PATCH /users/7 { "status": "active" }`
- HTTP methods carry the verb; the URL carries the noun

## Status codes — say what happened

```js
// 200 OK / 201 Created:
app.post('/api/users', async (req, res) => {
  const user = await db.create(req.body);
  res.status(201).location(`/api/users/${user.id}`).json(user);
});

// 204 No Content — delete or update that returns nothing:
app.delete('/api/users/:id', async (req, res) => {
  await db.remove(req.params.id);
  res.sendStatus(204);
});

// 404 — resource missing:
app.get('/api/users/:id', async (req, res) => {
  const user = await db.find(req.params.id);
  if (!user) return res.status(404).json({ error: 'User not found' });
  res.json(user);
});
```

**Code families:**
```text
200  OK                    · generic success
201  Created               · POST that made something
204  No Content            · success, nothing to return
400  Bad Request           · invalid input / malformed body
401  Unauthorized          · no / bad credentials
403  Forbidden             · authenticated but not allowed
404  Not Found             · no such resource
409  Conflict              · state conflict (e.g., duplicate email)
422  Unprocessable Entity  · valid syntax, fails validation
429  Too Many Requests     · rate limited
500  Internal Server Error · the server broke
```

## Validation — trust nothing

```js
// Validate before touching the database:
import { z } from 'zod';

const createUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
  age: z.number().int().min(13).optional(),
});

app.post('/api/users', async (req, res) => {
  const parsed = createUserSchema.safeParse(req.body);
  if (!parsed.success) {
    return res.status(400).json({
      error: 'Validation failed',
      details: parsed.error.flatten(),   // field → messages
    });
  }
  const user = await db.create(parsed.data);
  res.status(201).json(user);
});
```

**The rule:** validate at the boundary — every field the client controls gets checked (types, lengths, enums, required vs optional). A schema library (zod, joi, yup) keeps this declarative and typed.

## Pagination, filtering, sorting

```js
// Limit/offset:
app.get('/api/posts', async (req, res) => {
  const limit = Math.min(Number(req.query.limit ?? 20), 100);   // cap it
  const offset = Math.max(Number(req.query.offset ?? 0), 0);

  const [items, total] = await db.findAndCount({ limit, offset });

  res.json({
    items,
    pagination: {
      total,
      limit,
      offset,
      hasMore: offset + items.length < total,
      next: offset + limit < total ? `/api/posts?limit=${limit}&offset=${offset + limit}` : null,
    },
  });
});

// Filter & sort — query params as field=value:
// GET /api/posts?author=7&sort=createdAt&order=desc
```

**The intuition:** never return unbounded lists. Clients need to page through, filter server-side (don't make them download everything and filter locally), and sort deterministically.

## Versioning

```text
Choice 1 — URL prefix:      /api/v1/users, /api/v2/users
Choice 2 — header:          Accept: application/vnd.myapi.v1+json

URL versioning is the most common and most visible — breaking changes bump the major.
```

**The rule:** breaking changes (renamed fields, removed endpoints, changed status codes) get a new version; additive changes don't. Old versions live until clients migrate.

## Idempotency

```text
Idempotent — same request, same result, no side effect differences on repeat:
GET, PUT, DELETE (safe to retry)

NOT idempotent by default:
POST  (two identical POSTs create two resources)

Fix for POST: accept an Idempotency-Key header and dedupe:
```

```js
app.post('/api/payments', async (req, res) => {
  const key = req.headers['idempotency-key'];
  if (key) {
    const existing = await db.findByIdempotencyKey(key);
    if (existing) return res.status(existing.status).json(existing.body);  // replay
  }
  const result = await processPayment(req.body);
  if (key) await db.storeIdempotencyKey(key, result);
  res.status(201).json(result);
});
```

**Why it matters:** networks retry. A client that times out and retries a `POST` shouldn't create two orders or charge twice. `PUT` is naturally idempotent (replace with the same body is the same result); `POST` needs the key.

## Consistent error shape

```js
// Always the same envelope:
function apiError(res, status, code, message, details) {
  return res.status(status).json({ error: { code, message, details } });
}

app.post('/api/users', async (req, res, next) => {
  try {
    const parsed = createUserSchema.safeParse(req.body);
    if (!parsed.success) {
      return apiError(res, 400, 'VALIDATION_ERROR', 'Invalid input', parsed.error.flatten());
    }
    const user = await db.create(parsed.data);
    res.status(201).json({ data: user });
  } catch (err) {
    if (err.code === 'ER_DUP_ENTRY') {
      return apiError(res, 409, 'DUPLICATE', 'Email already exists');
    }
    next(err);
  }
});
```

**The intuition:** every error looks the same — machine-readable `code`, human `message`, optional `details`. Clients can branch on the code and display the message. Ad-hoc error shapes are a nightmare for consumers.

---

**Setup:** A full CRUD API for books with validation and consistent errors.

**Solution:**
```js
import { Router } from 'express';
import { z } from 'zod';

const bookSchema = z.object({
  title: z.string().min(1),
  author: z.string().min(1),
  year: z.number().int().min(1000).max(2100),
});

const router = Router();

router.get('/api/books', async (req, res) => {
  const limit = Math.min(Number(req.query.limit ?? 20), 100);
  const offset = Math.max(Number(req.query.offset ?? 0), 0);
  const { items, total } = await db.books.find({ limit, offset });
  res.json({ items, total, limit, offset });
});

router.post('/api/books', async (req, res, next) => {
  const parsed = bookSchema.safeParse(req.body);
  if (!parsed.success) return res.status(400).json({ error: 'Validation failed', details: parsed.error.flatten() });
  try {
    const book = await db.books.create(parsed.data);
    res.status(201).location(`/api/books/${book.id}`).json(book);
  } catch (err) { next(err); }
});

router.get('/api/books/:id', async (req, res) => {
  const book = await db.books.find(req.params.id);
  if (!book) return res.status(404).json({ error: 'Book not found' });
  res.json(book);
});

router.delete('/api/books/:id', async (req, res) => {
  const ok = await db.books.remove(req.params.id);
  if (!ok) return res.status(404).json({ error: 'Book not found' });
  res.sendStatus(204);
});
```

**Key insight:** verbs match methods, resources are nouns, validation at the boundary, `201`/`204`/`404` chosen deliberately, pagination on lists. This small CRUD shape is the template for a whole service.

---

**Setup:** A client retries a payment `POST` after a timeout — how do you prevent double-charging?

**Solution:** Accept an `Idempotency-Key` header. Before processing, look up the key; if a stored result exists, return it verbatim instead of charging again. Store the result keyed by the idempotency key when you first succeed.

**Key insight:** the client owns retry decisions; the server owns dedupe. With the key, a retry is a *replay* — same response, no second side effect. This is exactly how Stripe-style APIs behave.

---

**Setup:** You changed `/users/:id` from returning `{name}` to `{firstName, lastName}` — how do you ship it?

**Solution:** Bump the API version: keep `/api/v1/users/:id` returning the old shape and add `/api/v2/users/:id` with the new one. Keep v1 live until its clients migrate, then retire it.

**Key insight:** additive changes (new fields, new endpoints) are backward-compatible — no version bump needed. Breaking changes (renames, deletions, status-code changes) need the bump. Clients pin the version they're built against.

---

## Practice (try before peeking)

1. `PUT` vs `PATCH` — what's the real difference?
2. Why `409 Conflict` over `400` for a duplicate email?
3. What's the point of an `Idempotency-Key` on `POST`?

<details><summary>Answers</summary>

1. `PUT` replaces the whole resource (send the complete representation; missing fields are reset). `PATCH` applies a partial update (send only the fields that change). `PUT` is idempotent; `PATCH` may not be.
2. `400` says "your input is malformed." `409` says "your request is well-formed but conflicts with the current state" — a duplicate email is exactly that: valid input, impossible to satisfy. The distinction lets clients distinguish fix-your-body from change-your-data.
3. Retries are real (timeouts, network failures). Without a key, two identical `POST`s create two resources / charge twice. The key makes the second request a replay of the first — deduped server-side.

</details>

---

**Common traps:**
- Verbs in URLs (`/api/deleteUser`) — restructure to methods + nouns
- Returning raw database errors to clients (leaks internals; use consistent error envelopes)
- Unbounded lists — no pagination on `GET /resource`
- Skipping validation on `PATCH` (partial bodies still need schema checks)
- Changing a response shape without versioning — silently breaks consumers

---
