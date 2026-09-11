# Dependency Injection

FastAPI's **dependency injection** (`Depends`) is the elegant replacement for repeated boilerplate. A dependency is just a function; FastAPI calls it, resolves *its* parameters (which can be other dependencies), and injects the result into your endpoint. Used for **DB sessions, auth, config, pagination, and cross-cutting logic** — and it composes like a tree.

**The Intuition:** Dependency injection is like a cafeteria with a standard tray line. Instead of every cook (endpoint) fetching their own plate, napkin, and cutlery (DB session, auth, config), the line *hands each cook a fully assembled tray* based on what they asked for. Ask for a session → you get one, with cleanup handled automatically when you're done.

## The basic dependency

```python
from fastapi import Depends, FastAPI

def get_db():
    db = connect_to_db()          # set up the resource
    try:
        yield db                  # hand it to the endpoint
    finally:
        db.close()                # guaranteed cleanup when done

app = FastAPI()

@app.get("/items/{item_id}")
def read_item(item_id: int, db: Session = Depends(get_db)):
    return db.query(Item, item_id)   # db is already connected
```

**Why `yield`:** a generator dependency runs setup *before* your endpoint and teardown *after* — exactly like a context manager. The `finally` guarantees the connection closes even if the endpoint raises. This is how FastAPI does request-scoped resources without leaks.

## Auth — the canonical use

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials

bearer = HTTPBearer()

def get_current_user(creds: HTTPAuthorizationCredentials = Depends(bearer)):
    token = creds.credentials
    payload = verify_token(token)               # raises on bad/expired
    if not payload:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid token",
        )
    user = db.find_user(payload["sub"])
    if not user:
        raise HTTPException(status_code=401, detail="User not found")
    return user


@app.get("/profile")
def profile(user: User = Depends(get_current_user)):
    return {"user": user}                        # guaranteed authenticated

@app.get("/admin")
def admin(user: User = Depends(get_current_user)):
    if user.role != "admin":
        raise HTTPException(status_code=403, detail="Admins only")
    return {"ok": True}
```

**The payoff:** every endpoint that declares `Depends(get_current_user)` gets auth for free — one line, consistent behavior, and 401s are generated in one place.

## Dependencies with parameters

```python
from typing import Annotated

# A dependency factory — returns a dependency bound to params:
def require_roles(*roles: str):
    def checker(user: User = Depends(get_current_user)):
        if user.role not in roles:
            raise HTTPException(status_code=403, detail="Not allowed")
        return user
    return checker


@app.delete("/admin/users/{user_id}")
def delete_user(
    user_id: int,
    user: Annotated[User, Depends(require_roles("admin", "superadmin"))] = None,
):
    # Only admin/superadmin reach this body
    return {"deleted": user_id}
```

## Composing dependencies

```python
def get_session():
    s = SessionLocal()
    try:
        yield s
    finally:
        s.close()

def get_current_user(
    session: Session = Depends(get_session),          # dep → another dep
    creds: HTTPAuthorizationCredentials = Depends(bearer),
):
    payload = verify_token(creds.credentials)
    return session.query(User).get(payload["sub"])

@app.get("/account")
def account(
    user: User = Depends(get_current_user),           # the composed dep
    session: Session = Depends(get_session),          # reused — same instance
):
    ...
```

**Dependency graph:** FastAPI builds the tree, deduplicates (a dependency needed in two places runs **once** per request by default — same session object is reused), and resolves everything before your endpoint runs.

## Cache & reuse — `use_cache`

```python
# By default, repeated Depends of the same callable share ONE result per request:
def get_settings():
    return load_settings()        # runs once per request even if 3 endpoints ask

# If you need a fresh call each time:
def get_random():
    return random.random()

@app.get("/a")
def a(r: float = Depends(get_random, use_cache=False)):  # always fresh
    ...
```

## Overriding dependencies for testing

```python
# tests/test_app.py
from fastapi.testclient import TestClient
from main import app, get_current_user

def fake_user():
    return User(id=1, name="Test User")

app.dependency_overrides[get_current_user] = fake_user    # swap auth for tests

client = TestClient(app)

def test_profile_without_real_auth():
    res = client.get("/profile")
    assert res.status_code == 200
    assert res.json()["user"]["name"] == "Test User"
```

**This is the killer feature for testing:** you can swap the DB session, auth, or any dependency for a stub — *without changing the endpoint code*. `dependency_overrides` is how FastAPI tests stay fast and hermetic.

---

**Setup:** A pagination dependency shared by every list endpoint.

**Solution:**
```python
from typing import Annotated

def get_pagination(
    page: int = 1,
    limit: int = 20,
):
    return {"page": max(page, 1), "limit": min(limit, 100)}

Pag = Annotated[dict, Depends(get_pagination)]

@app.get("/posts")
def list_posts(pag: Pag):
    offset = (pag["page"] - 1) * pag["limit"]
    return db.get_posts(offset=offset, limit=pag["limit"])

@app.get("/comments")
def list_comments(pag: Pag):
    offset = (pag["page"] - 1) * pag["limit"]
    return db.get_comments(offset=offset, limit=pag["limit"])
```

**Key insight:** one dependency = consistent pagination params + bounds on every list endpoint. Change the max limit once; every endpoint inherits it.

---

**Setup:** Require an API key header for internal endpoints.

**Solution:**
```python
from fastapi import Depends, Header, HTTPException

API_KEYS = {"svc-reports": "k_123...", "svc-mailer": "k_456..."}

def require_api_key(x_api_key: str = Header(...)):
    if x_api_key not in API_KEYS:
        raise HTTPException(status_code=401, detail="Bad API key")
    return x_api_key

@app.get("/internal/stats")
def stats(key: str = Depends(require_api_key)):
    return {"uptime": 99.9}
```

**Key insight:** the dependency owns the *whole* auth decision — extracting the header, validating the key, raising the 401. The endpoint just declares `Depends(require_api_key)` and can assume it's authenticated.

---

**Setup:** Why does the same dependency called twice in one endpoint only run once?

**Solution:** FastAPI caches dependency results per request (`use_cache=True` by default) — the dependency callable runs once, and both call sites receive the same instance. That's why `Depends(get_session)` in two nested dependencies yields the *same* session, not two connections.

**Key insight:** caching per request is what makes request-scoped resources (a DB transaction, a request-ID logger) safe and cheap. When you genuinely need fresh state (random values, counters), opt out with `use_cache=False`.

---

## Practice (try before peeking)

1. What does a `yield`-based dependency give you that a plain function doesn't?
2. How do you override a dependency in tests?
3. Why is dependency caching per-request a good default?

<details><summary>Answers</summary>

1. Setup + guaranteed teardown: the code before `yield` runs before the endpoint, the code after `yield` runs after — even on errors. That's how connections/transactions close reliably (it's a context-manager-in-a-function).
2. `app.dependency_overrides[original_dep] = fake_dep` — map the original callable to a stub, and FastAPI resolves the stub everywhere that dependency is used. No endpoint changes needed.
3. Because resources like DB sessions and auth lookups should happen once per request, shared by whoever needs them — not once per usage (wasteful) and not once per app lifetime (stale). Per-request caching matches the request's lifecycle exactly.

</details>

---

**Common traps:**
- Forgetting the `finally` cleanup in a `yield` dependency — leaked connections
- Re-inventing auth per endpoint instead of `Depends(get_current_user)`
- Real I/O in dependency *defaults* (evaluated at import time) instead of inside the dependency
- `use_cache=False` forgotten when you need a fresh value
- Not clearing `dependency_overrides` between tests — overrides leak across test cases

---
