# Middleware & CORS

**Middleware** wraps the whole request/response cycle — every request passes through it before hitting a route, and its response passes back through it on the way out. **CORS** (Cross-Origin Resource Sharing) is the browser security rule that decides whether a frontend on one origin may call your API from another — and it's configured as middleware.

**The Intuition:** Middleware is the security checkpoint at the entrance *and* exit of a building. Every visitor (request) passes through on the way in; every reply passes through on the way out. You can stamp things at the entrance (add request headers), inspect on exit (log status), or block entirely. CORS is a specific bouncer rule: "this visitor's home origin is allowed / not allowed to enter."

## Writing middleware

```python
import time
from fastapi import FastAPI, Request

app = FastAPI()

@app.middleware("http")
async def add_process_time_header(request: Request, call_next):
    start = time.perf_counter()

    response = await call_next(request)      # pass through to the route

    process_time = time.perf_counter() - start
    response.headers["X-Process-Time"] = str(process_time)   # stamp on the way out
    return response
```

**The contract:** middleware is `async`; it receives the `Request`, calls `call_next(request)` to continue the chain, and gets back the `Response`. Everything before `call_next` runs on the way *in*; everything after runs on the way *out*.

## Middleware use cases

```python
import time
from fastapi import FastAPI, Request

app = FastAPI()

# 1. Request logging:
@app.middleware("http")
async def log_requests(request: Request, call_next):
    start = time.perf_counter()
    response = await call_next(request)
    duration = time.perf_counter() - start
    print(f"{request.method} {request.url.path} -> {response.status_code} ({duration*1000:.1f}ms)")
    return response


# 2. Simple auth gate for an entire app:
@app.middleware("http")
async def require_token(request: Request, call_next):
    if request.url.path.startswith("/internal"):
        if request.headers.get("x-api-key") != "secret":
            from fastapi.responses import JSONResponse
            return JSONResponse(status_code=401, content={"error": "Unauthorized"})
    return await call_next(request)


# 3. Compression:
from starlette.middleware.gzip import GZipMiddleware
app.add_middleware(GZipMiddleware, minimum_size=1000)
```

**Route-scoped vs app-wide:** middleware applies to *everything*. For per-route protection use dependencies (`Depends(get_current_user)`) — middleware is for cross-cutting concerns (logging, headers, gzip), dependencies for per-route logic.

## CORS — the what and why

```python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173", "https://myapp.com"],  # frontend origins
    allow_credentials=True,
    allow_methods=["*"],          # or ["GET", "POST", "PUT", "DELETE"]
    allow_headers=["*"],          # or ["Authorization", "Content-Type"]
)
```

**The problem:** a browser at `http://localhost:5173` (React dev server) calling `http://localhost:8000/api` is a **cross-origin** request. Browsers block it by default — the API's response is unreachable unless the API *tells the browser it's OK* via CORS headers.

**The mechanics:**
```text
1. Browser sends a preflight OPTIONS request (for non-simple requests: custom headers, non-GET)
2. Server responds with Access-Control-Allow-Origin etc.
3. If allowed, the real request proceeds; if not, the browser blocks it.

The headers the server sends:
Access-Control-Allow-Origin: http://localhost:5173   ← which origins are OK
Access-Control-Allow-Methods: GET, POST, ...
Access-Control-Allow-Headers: Authorization, ...
```

**Danger:** `allow_origins=["*"]` with `allow_credentials=True` is **invalid** — browsers reject it. If you need cookies/auth, list real origins explicitly. Wildcard origins mean *any* website can read your API from a browser.

## TrustedHost middleware

```python
from starlette.middleware.trustedhost import TrustedHostMiddleware

app.add_middleware(
    TrustedHostMiddleware,
    allowed_hosts=["myapp.com", "*.myapp.com", "localhost"],
)
# Rejects requests with a Host header not in the list — blocks DNS-rebinding attacks.
```

## Middleware ordering

```python
# Middleware added FIRST runs LAST (outermost wraps everything):
app.add_middleware(TrustedHostMiddleware, allowed_hosts=["..."])
app.add_middleware(CORSMiddleware, ...)
app.add_middleware(GZipMiddleware, ...)

# Order matters: TrustedHost outermost (reject bad hosts early),
# then CORS (policy for cross-origin), then gzip (compress the body).
```

---

**Setup:** Log every request's duration and status.

**Solution:**
```python
import logging, time
from fastapi import FastAPI, Request

logger = logging.getLogger("uvicorn")

@app.middleware("http")
async def access_log(request: Request, call_next):
    start = time.perf_counter()
    response = await call_next(request)
    ms = (time.perf_counter() - start) * 1000
    logger.info("%s %s -> %s (%d ms)", request.method, request.url.path, response.status_code, ms)
    return response
```

**Key insight:** `call_next` is the boundary — time before it and after it brackets the entire route handling. This single middleware gives every endpoint request logging for free, without touching route code.

---

**Setup:** Allow the React dev server to call your API with `Authorization` headers.

**Solution:**
```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:5173"],     # the exact frontend origin
    allow_credentials=True,
    allow_methods=["GET", "POST", "PUT", "DELETE", "OPTIONS"],
    allow_headers=["Authorization", "Content-Type"],
)
```

**Key insight:** explicit origin, explicit methods, explicit headers — the least-privilege CORS config. Because the frontend sends `Authorization`, the browser preflights; these headers are exactly what the preflight asks about.

---

**Setup:** Why does `allow_origins=["*"]` break when you also need cookies or auth tokens?

**Solution:** The CORS spec forbids `Access-Control-Allow-Origin: *` together with `Access-Control-Allow-Credentials: true` — the browser rejects the combination. Wildcard means "anyone", credentials means "tie to my users" — the two can't coexist safely. You must enumerate the real origins.

**Key insight:** CORS is the browser's *read* gate — it doesn't stop servers or curl, it stops a webpage on origin A from silently using a logged-in user's session against origin B. Explicit origins are the price of credential-based auth.

---

## Practice (try before peeking)

1. What happens between `call_next(request)` and the response returning in middleware?
2. Why does CORS not apply to server-to-server calls?
3. Wildcard `allow_origins=["*"]` — when is it acceptable?

<details><summary>Answers</summary>

1. The request continues down the middleware stack into the route, the route runs and returns its response, and control returns to your middleware — so everything *after* `call_next` sees the finished response and can modify it (headers, logging, compression).
2. Because CORS is enforced by the *browser*, not the server. Server-to-server requests (curl, Python requests, microservices) don't run the preflight/same-origin policy — they just call the API. CORS protects browser-visible endpoints only.
3. For public, unauthenticated data with no credentials — e.g., a public API meant to be read from any origin. The moment you have cookies, auth tokens, or any per-user data, list explicit origins.

</details>

---

**Common traps:**
- `allow_origins=["*"]` + `allow_credentials=True` — invalid combo, browser rejects
- Forgetting to include `OPTIONS` in `allow_methods` — preflight fails
- Putting route-level auth in middleware when dependencies are the right tool
- CORS middleware added after routes but expecting it to apply (it still does — middleware wraps everything; but order among middlewares matters)
- Assuming CORS protects the API — it protects the browser; secure the API itself with real auth

---
