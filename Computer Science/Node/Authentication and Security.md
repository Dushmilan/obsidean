# Authentication & Security

Authentication is *who you are*; authorization is *what you can do*. The standard Node stack: hash passwords with **bcrypt**, issue **JWTs** (or manage **sessions**), protect routes with middleware, and harden the server with **helmet, rate limiting, and safe defaults**. The security mindset: never trust the client, never log secrets, never roll your own crypto.

**The Intuition:** A password hash is a one-way meat grinder — you can grind beef into patties, but you can't turn a patty back into beef. You never store the password; you store the *ground* version and re-grind the login attempt to compare. A JWT is a tamper-proof nametag: signed by the server, so a client can't edit it without breaking the signature. Sessions are a coat-check stub: the server holds the real data, the client holds a random ticket.

## Hashing passwords with bcrypt

```js
import bcrypt from 'bcrypt';

const SALT_ROUNDS = 12;

// On signup:
const hash = await bcrypt.hash(password, SALT_ROUNDS);
await db.createUser({ email, passwordHash: hash });

// On login:
const user = await db.findByEmail(email);
const ok = await bcrypt.compare(password, user.passwordHash);
if (!ok) throw new AppError(401, 'INVALID_CREDENTIALS', 'Bad email or password');
```

**Why bcrypt (and never plain/`md5`/`sha256`):** bcrypt is *deliberately slow* (salt rounds) and automatically salted — the same password for two users produces different hashes. Fast hashes (MD5/SHA) let attackers brute-force millions/sec; bcrypt throttles that to thousands/sec. **Never roll your own crypto — use vetted libraries.**

## JWT — stateless tokens

```js
import jwt from 'jsonwebtoken';

const SECRET = process.env.JWT_SECRET;      // NEVER hardcode; keep it 32+ random bytes

function signToken(user) {
  return jwt.sign(
    { sub: user.id, role: user.role },       // payload — claims
    SECRET,
    { expiresIn: '1h' }
  );
}

function verifyToken(token) {
  try {
    return jwt.verify(token, SECRET);        // throws if expired/tampered
  } catch {
    throw new AppError(401, 'INVALID_TOKEN', 'Session expired or invalid');
  }
}
```

**The flow:** login → server signs a token → client stores it (usually `Authorization: Bearer <token>` header) → every request sends it → middleware verifies → `req.user` is set.

**JWT anatomy:**
```text
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiI3Iiwicm9sZSI6ImFkbWluIn0.9QqY...
└── header (alg) ──┘└──── payload (claims) ────┘└── signature ──┘
The signature is HMAC over header+payload with the SECRET.
Change one byte → signature no longer matches → verification fails.
```

## Protecting routes with middleware

```js
function requireAuth(req, res, next) {
  const header = req.headers.authorization;
  const token = header?.startsWith('Bearer ') ? header.slice(7) : null;
  if (!token) return res.status(401).json({ error: 'Missing token' });

  const payload = verifyToken(token);         // throws 401 on bad/expired
  req.user = { id: payload.sub, role: payload.role };
  next();
}

function requireRole(...roles) {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ error: 'Forbidden' });
    }
    next();
  };
}

app.get('/profile', requireAuth, (req, res) => res.json({ user: req.user }));
app.delete('/admin/users/:id', requireAuth, requireRole('admin'), deleteUser);
```

**401 vs 403:** 401 = "prove who you are" (no/invalid credentials). 403 = "I know who you are, you're not allowed" (authenticated but lacking permission).

## Sessions vs JWT

```text
Sessions:
  server stores session data keyed by a random ID; client holds the cookie.
  ✅ revocable instantly, ✅ easy logout
  ❌ server memory/store per session

JWT:
  stateless — the token carries the claims; server just verifies.
  ✅ no server storage, ✅ scales horizontally easily
  ❌ hard to revoke before expiry (blacklist or short expiry + refresh tokens)

For most apps: short-lived JWT (15m–1h) + refresh token, or sessions if you
need instant revocation. Never put secrets in the JWT payload (it's readable).
```

## Cookies — the secure defaults

> See [[Cookies]] for a deep dive on cookie mechanics, attributes, and the full security picture.

```js
res.cookie('session', sessionId, {
  httpOnly: true,     // JS can't read it — XSS can't steal it
  secure: true,       // HTTPS only
  sameSite: 'lax',    // CSRF protection
  maxAge: 1000 * 60 * 60 * 24,   // 1 day
});
```

**The threats each flag stops:**
- `httpOnly` — XSS can't read the cookie via `document.cookie`
- `secure` — never sent over plain HTTP
- `sameSite` — other sites' requests can't carry it (CSRF)

## Server hardening

```js
import helmet from 'helmet';
import rateLimit from 'express-rate-limit';

app.use(helmet());                                  // security headers (X-Frame-Options, HSTS, ...)
app.disable('x-powered-by');                        // don't advertise the stack

const limiter = rateLimit({
  windowMs: 60_000,
  max: 100,                                         // 100 requests/min/IP
  message: { error: 'Too many requests' },
});
app.use('/api', limiter);

// Stricter on auth endpoints — brute-force protection:
app.use('/api/login', rateLimit({ windowMs: 60_000, max: 5 }));
```

## Secrets & env — never in code

```bash
# .env — NEVER committed
JWT_SECRET=9f8e...      # 32+ random bytes
DATABASE_URL=postgres://...
```

```js
import 'dotenv/config';
const SECRET = process.env.JWT_SECRET;
if (!SECRET) throw new Error('JWT_SECRET is missing');   // fail fast at boot
```

**The rules:**
- Secrets in env vars (or a secret manager), never in source or config committed to git
- `.env` in `.gitignore`; commit `.env.example` with placeholder names
- Rotate secrets periodically; invalidate leaked ones immediately
- Never log passwords, tokens, or full request bodies

## Common attacks & defenses

```text
XSS   — injected script runs in your page            → escape output, CSP via helmet, httpOnly cookies
SQLi  — user input smuggled into SQL                 → parameterized queries ONLY (never string concat)
CSRF  — another site triggers state-changing requests → sameSite cookies, CSRF tokens for stateful forms
NoSQLi— operators ($gt, $ne) smuggled into queries   → validate/whitelist fields before querying
DoS   — floods, expensive endpoints                  → rate limiting, timeouts, pagination caps
Mass assignment — client sets fields it shouldn't     → whitelist fields in updates (zod pick / explicit)
```

```js
// Parameterized queries — NEVER interpolate user input into SQL:
// BAD:   `SELECT * FROM users WHERE email = '${email}'`    ← SQL injection
// GOOD:  'SELECT * FROM users WHERE email = $1', [email]
```

---

**Setup:** Full signup/login flow with hashed passwords and JWT.

**Solution:**
```js
import bcrypt from 'bcrypt';
import jwt from 'jsonwebtoken';
import { z } from 'zod';

const signupSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
});

app.post('/api/signup', async (req, res, next) => {
  const parsed = signupSchema.safeParse(req.body);
  if (!parsed.success) return res.status(400).json({ error: 'Validation failed' });

  const { email, password } = parsed.data;
  const existing = await db.findByEmail(email);
  if (existing) return res.status(409).json({ error: 'Email already registered' });

  const passwordHash = await bcrypt.hash(password, 12);
  const user = await db.createUser({ email, passwordHash });
  res.status(201).json({ token: signToken(user), user: { id: user.id, email: user.email } });
});

app.post('/api/login', async (req, res, next) => {
  const user = await db.findByEmail(req.body.email);
  const ok = user && await bcrypt.compare(req.body.password, user.passwordHash);
  if (!ok) return res.status(401).json({ error: 'Invalid email or password' });
  res.json({ token: signToken(user), user: { id: user.id, email: user.email } });
});
```

**Key insight:** same 401 message for "no such user" and "wrong password" — don't leak which. Passwords are hashed with bcrypt before storage. The token carries only identity claims, and it's verified by signature on every request.

---

**Setup:** A token in the URL (`?token=...`) vs the `Authorization` header — which and why?

**Solution:** The header. Tokens in URLs end up in browser history, server access logs, and referrer headers — all places secrets leak. The `Authorization: Bearer <token>` header is never logged by well-configured servers and isn't part of the URL.

**Key insight:** where a secret *travels* matters as much as how it's generated. Headers for tokens; `httpOnly` cookies for session IDs — never query strings.

---

**Setup:** Why "password123" hashed with bcrypt is still weak, and what else you need.

**Solution:** bcrypt stops *offline* brute-force (guessing the hash), but common passwords are found instantly by dictionary/credential-stuffing attacks — the hash doesn't matter if the password is guessable. Add: minimum-length rules, breach-list checks, and rate limiting on login to slow *online* guessing.

**Key insight:** hashing protects the *database leak* scenario; password policy + rate limiting protect the *login* scenario. Both layers are needed.

---

## Practice (try before peeking)

1. Why bcrypt over MD5/SHA for passwords?
2. 401 vs 403 — when do you send each?
3. Three flags on a session cookie and the attack each blocks?

<details><summary>Answers</summary>

1. bcrypt is deliberately slow and salted — offline brute-force costs thousands of attempts/sec instead of millions, and identical passwords hash differently (salting). MD5/SHA are designed to be fast, which is exactly what you don't want for password storage.
2. 401 when credentials are missing/invalid — "prove who you are." 403 when credentials are valid but the user lacks permission — "I know who you are, and you can't do this."
3. `httpOnly` blocks XSS from reading the cookie via JS; `secure` blocks transmission over plain HTTP; `sameSite` blocks cross-site requests from carrying the cookie (CSRF).

</details>

---

**Common traps:**
- Hardcoding `JWT_SECRET` in source — leaked on any commit history exposure
- Storing plaintext passwords — a DB leak is a total account breach
- Putting secrets in JWT payloads (readable without the secret) or in URLs
- String-concatenating SQL with user input (injection) — always parameterize
- Verbose error messages that reveal internals ("ORA-00942: table missing") to clients
- Rolling your own crypto/signing instead of using vetted libraries

---
