# Async, Concurrency & Background Tasks

FastAPI runs on **ASGI** — async by default. Endpoints can be `async def` (event loop) or `def` (thread pool), and both coexist. **Background tasks** (`BackgroundTasks`) let you respond first and do slow work (emails, reports, webhooks) after the response is sent. The event loop mental model from Node applies — one loop, cooperative yielding, never block it.

**The Intuition:** A restaurant: the *event loop* is the host, seating tables (requests) one at a time and *never waiting* — when a table's food isn't ready, the host seats someone else and comes back later. `async def` endpoints are tables that cook awaits; `def` endpoints are tables that cook synchronously but get seated in a *separate kitchen* (threadpool) so the host isn't stuck. Background tasks are orders you take, promise delivery, and let the kitchen finish *after* the customer has left.

## async def vs def — the concurrency model

```python
import asyncio
import time

# def — runs in a THREADPOOL. For blocking work (sync DB, requests, CPU):
@app.get("/def")
def sync_endpoint():
    time.sleep(2)                    # blocking — but it's in a worker thread
    return {"took": 2}

# async def — runs on the EVENT LOOP. For awaitable work:
@app.get("/async")
async def async_endpoint():
    await asyncio.sleep(2)           # yields — the loop serves others meanwhile
    return {"took": 2}

# WRONG — blocking call inside async def stalls the WHOLE loop:
@app.get("/bad")
async def bad_endpoint():
    time.sleep(2)                    # the event loop is frozen for 2s — all requests wait
    return {"took": 2}
```

**The rules:**
- `await`-able work only → `async def`
- Blocking work (sync DB driver, `requests`, `time.sleep`, heavy CPU) → `def` (threadpool)
- Never mix: a blocking call inside `async def` blocks *everything*
- FastAPI detects `def` endpoints and runs them in the thread pool automatically

## Parallelism with asyncio

```python
import asyncio
import httpx

# SEQUENTIAL — 3 slow calls, ~3× the latency:
async def sequential():
    a = await fetch("/api/a")
    b = await fetch("/api/b")
    c = await fetch("/api/c")

# PARALLEL — all 3 at once, ~1× the latency:
async def parallel():
    async with httpx.AsyncClient() as client:
        r1, r2, r3 = await asyncio.gather(
            client.get("/api/a"),
            client.get("/api/b"),
            client.get("/api/c"),
        )
        return [r.json() for r in (r1, r2, r3)]
```

**`asyncio.gather`** runs multiple coroutines concurrently and returns when all complete. Same mental model as `Promise.all` — independent awaits grouped for speed; sequential awaits for dependent steps.

## Background tasks

```python
from fastapi import BackgroundTasks

def send_welcome_email(email: str):
    # slow blocking work — SMTP call
    ...

def generate_report(user_id: int):
    ...

@app.post("/users")
async def create_user(
    user: User,
    background_tasks: BackgroundTasks,
):
    # 1. Fast work now:
    stored = db.create(user)

    # 2. Queue slow work to run AFTER the response:
    background_tasks.add_task(send_welcome_email, stored.email)
    background_tasks.add_task(generate_report, stored.id)

    # 3. Respond immediately — the tasks run after the response is sent:
    return stored
```

**The contract:**
- Tasks run *after* the response is sent — the client isn't kept waiting
- They're fire-and-forget — no result comes back to the client
- For *durable* work (must survive crashes/restarts) use a real job queue (Celery, RQ, ARQ) instead
- Background tasks share the process — a crash loses queued tasks

## Streaming responses

```python
from fastapi.responses import StreamingResponse

async def generate_rows():
    for i in range(1_000_000):
        yield f"row {i}\n"           # streamed, not buffered

@app.get("/big")
async def big_export():
    return StreamingResponse(generate_rows(), media_type="text/plain")
```

**Why:** the response body streams chunk-by-chunk instead of building a million-row string in memory. Great for exports, logs, and long polls.

## Concurrency limits & resource control

```python
import asyncio

# Limit how many concurrent external calls — don't hammer an API:
sem = asyncio.Semaphore(10)

async def limited_call(url: str):
    async with sem:                   # max 10 in flight at once
        async with httpx.AsyncClient() as client:
            return await client.get(url)

async def fetch_all(urls: list[str]):
    return await asyncio.gather(*(limited_call(u) for u in urls))
```

---

**Setup:** A dashboard endpoint that fetches three independent data sources in parallel.

**Solution:**
```python
import asyncio
import httpx

@app.get("/dashboard")
async def dashboard():
    async with httpx.AsyncClient(timeout=10) as client:
        stats, users, alerts = await asyncio.gather(
            client.get("http://stats/internal"),
            client.get("http://users/internal"),
            client.get("http://alerts/internal"),
        )
    return {
        "stats": stats.json(),
        "users": users.json(),
        "alerts": alerts.json(),
    }
```

**Key insight:** `gather` fires all three HTTP calls concurrently — total latency ≈ the slowest call, not the sum. If these were three sequential awaits, the endpoint would be 3× slower for no reason.

---

**Setup:** After a user signs up, email them — but don't make them wait for the SMTP call.

**Solution:**
```python
@app.post("/signup", status_code=201)
def signup(user: User, background_tasks: BackgroundTasks):
    created = db.create_user(user)
    background_tasks.add_task(send_verification_email, created.email, created.token)
    return created
```

**Key insight:** the response returns instantly; the email send happens after. The client sees 201 immediately, and the slow SMTP work doesn't occupy a request. For critical deliveries (payment receipts), a durable queue is safer — background tasks are best-effort in-process.

---

**Setup:** Why does a synchronous DB call inside `async def` stall every other request?

**Solution:** Inside `async def`, your code runs *on* the event loop. A blocking call (sync driver, `time.sleep`) occupies the loop — nothing else can run until it returns. All concurrent requests freeze. `def` endpoints avoid this because FastAPI runs them in a worker thread; or use an async driver (`asyncpg`, SQLAlchemy async) and `await`.

**Key insight:** the event loop is single-threaded and cooperative — it only moves when you `await`. Blocking inside async is the one unforgivable sin in ASGI apps; it converts a concurrent server into a serial one.

---

## Practice (try before peeking)

1. `async def` vs `def` — how does FastAPI decide where each runs?
2. When are background tasks *not* enough?
3. What does `asyncio.gather` do — and when would you avoid it?

<details><summary>Answers</summary>

1. `async def` runs on the event loop; `def` runs in a threadpool worker. FastAPI makes the call automatically — you just pick the right one for whether the endpoint does awaitable work or blocking work.
2. When work must survive restarts, be retried, or scale across processes — background tasks are in-process and fire-and-forget. Email queues, report generation, or anything with delivery guarantees belong in Celery/RQ/ARQ or a message broker.
3. `gather` runs coroutines concurrently and awaits all results — the tool for independent I/O. Avoid it when calls depend on each other (await in sequence) or when you need to bound concurrency (pair with a `Semaphore`).

</details>

---

**Common traps:**
- Blocking calls inside `async def` — freezes the event loop
- `time.sleep` instead of `asyncio.sleep` in async code
- Using background tasks for critical/durable work — they're best-effort
- Sequential awaits where `gather` belongs — needless 3× latency
- Forgetting a `Semaphore` — unbounded concurrent external calls can overwhelm the upstream service

---
