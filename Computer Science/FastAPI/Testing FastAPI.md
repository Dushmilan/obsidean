# Testing FastAPI

FastAPI tests use **`TestClient`** (Starlette's ASGI test client) + **pytest**. The superpower is **`dependency_overrides`** — swap the DB, auth, or any dependency for a stub, and test the endpoint logic in isolation. Combine with an in-memory/SQLite DB for fast, hermetic integration tests.

**The Intuition:** A test client is a fake browser sitting inside your process — it sends real HTTP requests to your app without a network. Dependency overrides are *interchangeable organs*: the app's own DB session or auth check gets swapped for a controlled stand-in, so each test knows exactly what the app will find.

## The basic test

```python
# test_main.py
from fastapi.testclient import TestClient
from main import app

client = TestClient(app)

def test_health():
    res = client.get("/health")
    assert res.status_code == 200
    assert res.json() == {"status": "ok"}
```

```bash
pytest -q                 # run tests
pytest test_main.py -k health   # filter by name
```

**What TestClient does:** it drives your app through the ASGI interface in-process — routes, middleware, dependencies, serialization all run for real. No server process, no ports, fast tests.

## Testing CRUD with a real (test) database

```python
# conftest.py — shared fixtures
import pytest
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker
from fastapi.testclient import TestClient

from database import Base, get_db
from main import app

engine = create_engine("sqlite:///:memory:", connect_args={"check_same_thread": False})
TestingSession = sessionmaker(bind=engine, autocommit=False, autoflush=False)

@pytest.fixture
def client():
    Base.metadata.create_all(engine)          # fresh schema per test

    def override_get_db():
        db = TestingSession()
        try:
            yield db
        finally:
            db.close()

    app.dependency_overrides[get_db] = override_get_db
    with TestClient(app) as c:
        yield c
    app.dependency_overrides.clear()          # reset for the next test
```

```python
def test_create_and_get_user(client):
    res = client.post("/users", json={"email": "ada@example.com", "name": "Ada"})
    assert res.status_code == 201
    user_id = res.json()["id"]

    res = client.get(f"/users/{user_id}")
    assert res.status_code == 200
    assert res.json()["name"] == "Ada"
```

**The pattern:** override `get_db` with a fresh SQLite session, create the schema, run the test, clear overrides. Each test gets an isolated database — no cleanup choreography, no shared state.

## Overriding auth

```python
from main import app, get_current_user
from models import User

def make_user(id=1, role="admin"):
    return User(id=id, username="test", email="t@example.com", role=role, password_hash="x")

def test_admin_endpoint(client_with_auth):
    app.dependency_overrides[get_current_user] = lambda: make_user(role="admin")
    res = client.get("/admin/stats")
    assert res.status_code == 200

def test_non_admin_forbidden():
    app.dependency_overrides[get_current_user] = lambda: make_user(role="user")
    res = client.get("/admin/stats")
    assert res.status_code == 403
```

**The win:** auth logic is tested once (in its own tests); endpoint tests override it entirely — no real tokens, no setup dance. Every combination of role can be tested by swapping one function.

## Testing validation & errors

```python
def test_validation_error(client):
    res = client.post("/users", json={"email": "not-an-email"})   # missing name too
    assert res.status_code == 422
    body = res.json()
    assert body["detail"][0]["loc"] == ["body", "email"]          # where it failed

def test_404(client):
    res = client.get("/users/9999")
    assert res.status_code == 404
    assert res.json()["detail"] == "User not found"
```

**The contract:** validation failures return **422** with structured `detail` (field locations + messages); domain 404s come from your `HTTPException`. Tests pin both so clients can rely on them.

## Testing background tasks

```python
# main.py
@app.post("/signup", status_code=201)
def signup(data: UserCreate, db: Session = Depends(get_db), background_tasks: BackgroundTasks = None):
    ...
    background_tasks.add_task(send_email, email)

# test
def test_signup_schedules_email(client, monkeypatch):
    sent = []
    monkeypatch.setattr("main.send_email", lambda email: sent.append(email))

    res = client.post("/signup", json={...})
    assert res.status_code == 201
    assert "test@example.com" in sent          # task ran after response
```

**The catch:** with `TestClient`, background tasks run *after* the response is returned — but within the test's `with` block lifetime. `monkeypatch` replaces the real mail-sending function so the test asserts it was called without sending email.

## Testing async endpoints

```python
# pytest-asyncio — or TestClient handles it for you:
def test_async_endpoint(client):
    res = client.get("/dashboard")        # async endpoint — TestClient awaits it
    assert res.status_code == 200
```

For direct async unit tests (services, not endpoints), use `pytest-asyncio`:
```python
import pytest

@pytest.mark.asyncio
async def test_service():
    result = await my_async_service(7)
    assert result.ok
```

---

**Setup:** Test the full signup → login → me flow.

**Solution:**
```python
def test_auth_flow(client):
    # signup
    res = client.post("/signup", json={"username": "ada", "email": "ada@example.com", "password": "secret123"})
    assert res.status_code == 201

    # login
    res = client.post("/token", data={"username": "ada", "password": "secret123"})  # form-encoded!
    assert res.status_code == 200
    token = res.json()["access_token"]

    # use the token
    res = client.get("/users/me", headers={"Authorization": f"Bearer {token}"})
    assert res.status_code == 200
    assert res.json()["username"] == "ada"

    # wrong password → 401
    res = client.post("/token", data={"username": "ada", "password": "wrong"})
    assert res.status_code == 401
```

**Key insight:** `/token` expects *form-encoded* data (`data=`, not `json=`) because that's what `OAuth2PasswordRequestForm` parses. The flow test exercises the real auth path end-to-end — hash, verify, sign, decode.

---

**Setup:** Test that the admin delete endpoint rejects a regular user.

**Solution:**
```python
def test_delete_user_requires_admin(client):
    app.dependency_overrides[get_current_user] = lambda: make_user(role="user")
    res = client.delete("/admin/users/1")
    assert res.status_code == 403          # authenticated but not allowed
```

**Key insight:** swapping `get_current_user` for a `role="user"` stub exercises the *authorization* path (the 403) without any token machinery. The role-check logic is what's under test — cleanly isolated.

---

**Setup:** Why override `get_db` instead of testing against the real database?

**Solution:** The real DB is shared, slow, and stateful — tests become order-dependent and flaky. An overridden dependency with SQLite (or a test Postgres) gives each test a clean, fast, isolated database. The endpoint logic runs unchanged; only the storage behind it swaps.

**Key insight:** `dependency_overrides` is the contract that makes FastAPI tests hermetic — the app doesn't know (or care) that it's running against SQLite. That's the whole point of dependency injection: replaceability at test time.

---

## Practice (try before peeking)

1. What exactly does `TestClient` exercise — and not exercise?
2. Why `data=` for `/token` but `json=` for `/signup`?
3. Why clear `app.dependency_overrides` after each test?

<details><summary>Answers</summary>

1. It exercises the full ASGI stack in-process — routing, middleware, dependencies, validation, serialization — with real HTTP semantics. It doesn't exercise a real network, a real server process, or the OS socket layer (that's the job of a separate smoke test).
2. Because `/token` uses `OAuth2PasswordRequestForm`, which parses `application/x-www-form-urlencoded` — that's the `data=` (form) argument. `/signup` takes a Pydantic body model — that's `json=`. The wire format must match what the endpoint declares.
3. Overrides persist on the app between tests — a leaked override (e.g., auth stubbed for one test) silently changes every test that runs after. `clear()` (ideally in a fixture teardown) resets the app to its real dependencies.

</details>

---

**Common traps:**
- Forgetting to clear `dependency_overrides` — overrides leak across tests
- `json=` for form-encoded endpoints (and vice versa) — mysterious 422s
- Testing against the shared dev database — flaky, order-dependent tests
- Not testing the 422/404 paths — validation and not-found are core behavior
- Real side effects in tests (actual emails, external APIs) — mock at the boundary

---
