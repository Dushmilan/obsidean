# Cookies

Cookies are small pieces of data that a server tells the browser to store and send back with every subsequent request — they solve HTTP's stateless nature by giving the client a way to remember who it is, what it's doing, and what it's agreed to.

**The Intuition:** Think of a cookie like a coat-check ticket. When you arrive at a restaurant (first visit), the host gives you a numbered ticket (Set-Cookie). Every time you come back, you show the ticket so the host knows which coat is yours (session data, preferences, auth state). The ticket is small, carried by you (the client), and the host keeps the matching record on their end. Without it, the host treats you as a stranger every time you walk in.

## What it solves

1. **State** — HTTP is stateless; cookies let the server associate requests across time (login sessions, shopping carts)
2. **Personalization** — remember preferences (language, theme) without asking every visit
3. **Tracking** — identify users across pages (analytics, ads — for better or worse)

## How cookies work

```text
Client (Browser)                          Server
  ---- GET /login ----------------------->
                   <---- Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Lax
  ---- GET /dashboard ------------------>  Cookie: session=abc123
                   <---- Set-Cookie: prefs=dark; Max-Age=86400
  ---- GET /settings ------------------->  Cookie: session=abc123; prefs=dark
```

1. Server sends `Set-Cookie` header in response
2. Browser stores the cookie (domain, path, expiry governed by attributes)
3. Browser auto-attaches matching cookies in `Cookie` header on subsequent requests to same domain/path
4. Server reads `Cookie` header to restore state

## Cookie attributes

| Attribute | Purpose | Example |
|-----------|---------|---------|
| `Name=Value` | The actual data | `session=abc123` |
| `Domain` | Which domains receive it | `.example.com` (all subdomains) |
| `Path` | Which paths under the domain | `/api` only |
| `Expires` | Absolute expiry date | `Thu, 01 Jan 2027 00:00:00 GMT` |
| `Max-Age` | Relative expiry (seconds) | `86400` (1 day) |
| `HttpOnly` | JS can't read via `document.cookie` | Prevents XSS theft |
| `Secure` | HTTPS only | Won't send over HTTP |
| `SameSite` | Cross-site request policy | `Strict`, `Lax`, `None` |
| `Priority` | When browser must evict cookies | `High`, `Low`, `Medium` |

**Session vs persistent cookies:**
- No `Expires`/`Max-Age` → session cookie, deleted when browser closes
- Has `Expires`/`Max-Age` → persistent cookie, survives browser restart

## In Node.js (Express)

```js
import express from 'express';
const app = express();

// Setting cookies
app.get('/login', (req, res) => {
  const sessionId = generateSessionId();
  await saveSession(sessionId, { userId: 42 });

  res.cookie('session', sessionId, {
    httpOnly: true,     // JS can't read it — XSS can't steal it
    secure: true,       // HTTPS only
    sameSite: 'lax',    // CSRF protection
    maxAge: 1000 * 60 * 60 * 24,   // 1 day
    signed: true,       // tamper detection via HMAC
  });

  res.json({ ok: true });
});

// Reading cookies
app.use(express.cookieParser('secret-key-for-signing'));  // parses + verifies signed cookies

app.get('/profile', (req, res) => {
  const session = req.signedCookies.session;  // undefined if tampered/missing
  if (!session) return res.status(401).json({ error: 'Not authenticated' });
  // look up session data...
});

// Clearing cookies
app.get('/logout', (req, res) => {
  res.clearCookie('session');
  res.json({ ok: true });
});
```

## Security: the three flags and what they block

| Flag | Attack it stops | How |
|------|----------------|-----|
| `HttpOnly` | XSS (Cross-Site Scripting) | `document.cookie` returns nothing — injected scripts can't exfiltrate the cookie |
| `Secure` | Network sniffing / MITM | Cookie never travels over plain HTTP |
| `SameSite` | CSRF (Cross-Site Request Forgery) | Browser won't attach cookie to cross-origin requests (except top-level GETs with `Lax`) |

**SameSite values:**
- `Strict` — never sent on cross-site requests (even clicking a link from another site)
- `Lax` — sent on top-level navigation (clicking a link), but not on cross-site form POST or AJAX (default in modern browsers)
- `None` — always sent, but **requires** `Secure` (used for cross-site embedded content like third-party widgets)

## Cookies vs other storage

| Storage | Where | Survives? | JS access | Size |
|---------|-------|-----------|-----------|------|
| Cookie | Client + sent to server every request | Yes (until expiry) | Depends on `HttpOnly` | ~4KB |
| `localStorage` | Client only | Yes (forever) | Yes | ~5-10MB |
| `sessionStorage` | Client only | Tab only | Yes | ~5-10MB |
| Server session | Server (DB/Redis) | Depends on server | No | Unlimited |

**Key difference:** cookies travel with every HTTP request; `localStorage`/`sessionStorage` only if JS explicitly reads and sends them.

## Signed cookies

```js
// Signing prevents tampering — if client changes the value, HMAC check fails
res.cookie('userId', '42', { signed: true });

// Express + cookieParser(secret) stores signed version: s:42.HMACHERE
// On read: cookieParser verifies HMAC, returns original value or undefined if tampered
```

Signed cookies are **not encrypted** — the value is readable, just tamper-proof. For secrets, encrypt or use server-side sessions.

---

## Practice (try before peeking)

1. Why does `SameSite=Lax` still allow a cookie to be sent on a top-level GET navigation from another site?
2. What's the difference between a session cookie and a persistent cookie?
3. If you set `Secure` but forget `HttpOnly`, what attack vector remains open?

<details><summary>Answers</summary>

1. `Lax` is a compromise — top-level navigations (clicking `<a href>`) are the web's basic linking mechanism; blocking them would break the web. It only blocks cross-site state-changing requests (POST, AJAX, iframes).
2. A session cookie has no `Expires`/`Max-Age`, so the browser deletes it when the tab/browser closes. A persistent cookie has an explicit lifetime and survives restarts.
3. XSS — an injected script can read the cookie via `document.cookie` and exfiltrate it, even though it's only sent over HTTPS.

</details>

---

**Common traps:**
- Setting cookies without `HttpOnly` — XSS can steal session tokens via `document.cookie`
- Using `SameSite=None` without `Secure` — browsers reject it
- Putting secrets in the cookie value — cookies are readable by the client; encrypt or use server-side sessions
- Forgetting to set `Path` — cookie may be sent to unintended routes
- Using `localStorage` for auth tokens — no `HttpOnly` equivalent, XSS reads them directly
- Mixing `Max-Age` and `Expires` — `Max-Age` takes precedence in modern browsers

---

Cookies are the browser's built-in state mechanism — use `HttpOnly` + `Secure` + `SameSite` as your defaults, sign them for tamper detection, and keep secrets server-side.

**Up:** [[Node_Index]]
