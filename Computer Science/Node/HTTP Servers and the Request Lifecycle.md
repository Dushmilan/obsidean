# HTTP Servers & the Request Lifecycle

Every web framework in Node (Express, Fastify, Koa) wraps the built-in `http` module. Knowing the raw **request/response lifecycle** — how a connection becomes a `req`/`res`, how the response is written and ended, what headers do — demystifies everything above it.

**The Intuition:** An HTTP server is a mailroom. A letter (request) arrives with an envelope (headers: who it's from, what type of content, cookies) and a body (the actual content). You read the envelope, decide what to do, and send a reply envelope (status code + headers) with a reply body. The worker at the window (`http.createServer(callback)`) handles one letter per callback invocation.

## The minimal server

```js
import http from 'node:http';

const server = http.createServer((req, res) => {
  // req  — incoming request (method, url, headers, body stream)
  // res  — outgoing response (status, headers, body stream)

  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello from Node');           // end() sends the response
});

server.listen(3000, () => {
  console.log('listening on http://localhost:3000');
});
```

**The contract:** every request MUST get exactly one `res.end()`. `res.write(chunk)` streams body data; `res.end()` finishes it. Forgetting to end leaves the client hanging.

## The request object

```js
http.createServer((req, res) => {
  console.log(req.method);        // 'GET', 'POST', ...
  console.log(req.url);           // '/api/users?id=7'
  console.log(req.httpVersion);   // '1.1'

  // Headers — lowercased keys:
  console.log(req.headers['content-type']);
  console.log(req.headers['user-agent']);

  // Body — a readable stream:
  let body = '';
  req.on('data', (chunk) => { body += chunk; });
  req.on('end', () => {
    console.log('full body:', body);
    res.end('got it');
  });
});
```

**Key facts:**
- `req.url` is the *raw* path+query — parse it with `new URL(req.url, 'http://localhost')` or a router
- `req.headers` is always lowercase
- The body is a **stream** — you must collect it. For JSON you'd accumulate chunks then `JSON.parse`
- `req.method` decides GET/POST/PUT/DELETE behavior (raw Node: you branch manually)

## Status codes & headers

```js
// Setting a response properly:
res.writeHead(201, {
  'Content-Type': 'application/json',
  'Location': '/api/users/7',
});

res.end(JSON.stringify({ id: 7, name: 'Ada' }));
```

**Status code families:**
```text
2xx  Success        200 OK · 201 Created · 204 No Content
3xx  Redirect       301 Moved Permanently · 304 Not Modified
4xx  Client error   400 Bad Request · 401 Unauthorized · 403 Forbidden · 404 Not Found · 429 Too Many Requests
5xx  Server error   500 Internal Server Error · 503 Service Unavailable
```

**Common headers:**
```text
Content-Type       what the body is   (application/json, text/html, image/png)
Content-Length     body size in bytes (set automatically with end(string) in most cases)
Cache-Control      how clients may cache (public, max-age=3600, no-store)
Location           where a redirect goes (used with 3xx)
Set-Cookie         a cookie for the client to store & send back
```

## Routing by hand

```js
const server = http.createServer((req, res) => {
  const url = new URL(req.url, 'http://localhost');
  const path = url.pathname;

  if (req.method === 'GET' && path === '/') {
    res.setHeader('Content-Type', 'text/plain');
    res.end('Home');
  } else if (req.method === 'GET' && path === '/api/users') {
    res.setHeader('Content-Type', 'application/json');
    res.end(JSON.stringify([{ id: 1, name: 'Ada' }]));
  } else if (req.method === 'POST' && path === '/api/users') {
    let body = '';
    req.on('data', c => body += c);
    req.on('end', () => {
      const user = JSON.parse(body);            // assume valid JSON
      res.writeHead(201, { 'Content-Type': 'application/json' });
      res.end(JSON.stringify({ ...user, id: 2 }));
    });
  } else {
    res.writeHead(404, { 'Content-Type': 'text/plain' });
    res.end('Not found');
  }
});
```

**The pain point:** this is why frameworks exist. Raw Node gives you complete control but no routing, no body parsing, no static files, no middleware — all manual.

## Parsing URLs & query strings

```js
const url = new URL(req.url, 'http://localhost');
url.pathname;            // '/api/users'
url.searchParams.get('id');   // '7'
url.searchParams.get('page'); // '2'

// Path parameters by hand:
// GET /api/users/7
const match = url.pathname.match(/^\/api\/users\/(\d+)$/);
if (match) {
  const id = match[1];
}
```

## Keep-alive & connections

Modern HTTP reuses one TCP connection for many requests (keep-alive). `server.listen` also accepts callbacks/options:

```js
server.listen(3000, '0.0.0.0', () => console.log('ready'));
// 0.0.0.0 = all interfaces (needed for Docker/network access)
```

---

**Setup:** A JSON API endpoint that echoes back what it received with a 201.

**Solution:**
```js
const server = http.createServer((req, res) => {
  const url = new URL(req.url, 'http://localhost');

  if (req.method === 'POST' && url.pathname === '/api/echo') {
    let body = '';
    req.on('data', c => { body += c; });
    req.on('end', () => {
      res.writeHead(201, { 'Content-Type': 'application/json' });
      res.end(body);                        // echo the raw JSON
    });
    return;
  }

  res.writeHead(404, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify({ error: 'Not found' }));
});
```

**Key insight:** collecting the body via stream events, then responding with the right status + content-type. The early `return` after `res.end()` is the key — it stops the function from falling through to the 404.

---

**Setup:** A redirect from `/old` to `/new`.

**Solution:**
```js
if (req.method === 'GET' && url.pathname === '/old') {
  res.writeHead(301, { Location: '/new' });
  res.end();
}
```

**Key insight:** the `Location` header + 3xx status tells the browser where to go. `301` is permanent (browsers cache it, good for moved pages); `302`/`303` are temporary (POST→GET redirects, e.g. after a form submit).

---

**Setup:** Why does a hanging request (no `res.end()`) freeze the client?

**Solution:** The server only sends the response when `end()` (or `writeHead`+`end`) is called — the TCP connection stays open waiting. The client keeps waiting too (until its timeout), and the connection slot stays occupied. Always `end()` exactly once on every code path, including error paths.

**Key insight:** request handling is *you* completing the contract: parse, respond, end. Leaking unfinished responses is the classic source of "my API randomly hangs."

---

## Practice (try before peeking)

1. What's the minimum a request handler must do?
2. How do you read a JSON request body with raw `http`?
3. Why do real apps use Express/Fastify instead of raw `http`?

<details><summary>Answers</summary>

1. Respond and end — every code path must reach `res.end()`. The body/headers are optional; ending is not.
2. Collect the body stream: `req.on('data')` accumulates chunks, `req.on('end')` fires when complete — then `JSON.parse`. There's no built-in body parser in raw `http`.
3. Frameworks give routing, middleware, body parsing, error handling, static files, and ecosystem plugins — raw `http` requires hand-rolling all of it. Raw Node is for learning and for very minimal custom servers.

</details>

---

**Common traps:**
- Not calling `res.end()` on error paths — hanging clients
- Calling `res.end()` twice — `ERR_HTTP_HEADERS_SENT`
- Treating `req.url` as parsed (it's raw — parse with `URL` or a router)
- Reading the body as a string when the encoding matters (binary uploads → collect as Buffer)
- Blocking the event loop in a handler (sync file reads, heavy loops) — stalls every connection

---
