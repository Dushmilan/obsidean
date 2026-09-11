# Path, Query & Body Parameters

FastAPI derives every input from the function signature: **path parameters** (`/items/{item_id}`) from the path, **query parameters** from the URL's `?`, and **body** from the JSON payload. `Query`, `Path`, and `Body` give fine-grained control (constraints, defaults, aliases, examples) and `HTTPException` handles the error paths.

**The Intuition:** The function signature is the *menu*. Where a parameter lives (path, query, body) is decided by how it's declared: path params appear in the route string, query params are simple scalar types with defaults, and anything that looks like a Pydantic model is the body. FastAPI reads the menu and does the waiter's job.

## Path parameters

```python
@app.get("/users/{user_id}")
def get_user(user_id: int):
    return {"user_id": user_id}

# /users/7   → {"user_id": 7}
# /users/abc → 422 (must be an int)
```

```python
from fastapi import Path

@app.get("/users/{user_id}")
def get_user(
    user_id: int = Path(ge=1),          # path param with constraints
):
    return {"user_id": user_id}

# /users/0 → 422 — must be >= 1
```

**Path order matters:** a route with a literal segment must come before a catch-all param with the same prefix:
```python
@app.get("/users/me")        # specific — declared FIRST
def current_user(): ...

@app.get("/users/{user_id}") # parameterized — declared after
def get_user(user_id: int): ...
```
FastAPI matches in declaration order — `/users/me` must win over `/users/{user_id}`.

## Query parameters

```python
from fastapi import Query

@app.get("/search")
def search(
    q: str = Query(min_length=1, max_length=100),   # required query param (no default)
    page: int = Query(1, ge=1),                     # default 1, >= 1
    limit: int = Query(20, ge=1, le=100),           # bounded
    sort: str = Query("created", pattern=r"^(created|title)$"),  # enum-like
    tags: list[str] = Query([]),                    # /search?tags=a&tags=b
):
    return {"q": q, "page": page, "limit": limit, "sort": sort, "tags": tags}
```

**The rule:** a parameter with no default is **required**; with a default it's **optional**. FastAPI infers whether it's a query param: *scalar types with defaults* → query, *Pydantic models* → body, *name in the path* → path.

## Body parameters

```python
from pydantic import BaseModel

class Order(BaseModel):
    items: list[str]
    shipping: str = "standard"

@app.post("/orders")
def create_order(order: Order):          # body → validated Order
    return {"received": order.items, "shipping": order.shipping}
```

### One body, multiple models

```python
from fastapi import Body

class Item(BaseModel):
    name: str
    price: float

class User(BaseModel):
    name: str

@app.put("/items/{item_id}")
def update_item(
    item_id: int,
    item: Item,                          # body field "item"
    user: User = Body(embed=True),       # body field "user" (nested under key)
):
    # Body(embed=True) expects: {"item": {...}, "user": {...}}
    return {"item": item, "by": user}
```

### Mixing body and scalars

```python
@app.post("/items")
def create(
    item: Item,                          # the body
    x_token: str = Header(...),          # header
    q: int | None = None,                # query
):
    ...
```

## Headers & cookies

```python
from fastapi import Header, Cookie

@app.get("/whoami")
def whoami(
    user_agent: str | None = Header(None),   # case-insensitive header
    x_token: str = Header(..., alias="X-Token"),  # non-standard name
    session_id: str | None = Cookie(None),   # from the Cookie header
):
    return {"user_agent": user_agent}
```

## `Query`/`Path`/`Body` — the full toolbox

```python
from fastapi import Query, Path, Body

Query(
    default=...,            # value or Ellipsis (required)
    ge=0, le=100,           # numeric bounds
    min_length=1, max_length=50,
    pattern=r"^[a-z]+$",
    alias="x-name",         # different wire name than the param name
    description="...",      # shows in docs
    examples=["react"],     # doc example values
    deprecated=True,        # mark in docs
)
```

## Error handling — HTTPException

```python
from fastapi import HTTPException

@app.get("/users/{user_id}")
def get_user(user_id: int):
    user = db.get(user_id)
    if not user:
        raise HTTPException(
            status_code=404,
            detail=f"User {user_id} not found",   # returned in the error body
        )
    return user

# with a custom response:
raise HTTPException(
    status_code=403,
    detail="You can't do that",
    headers={"X-Error": "forbidden"},       # extra headers on the error response
)
```

**Custom exception handlers** override the default error shape:
```python
from fastapi import Request
from fastapi.responses import JSONResponse

class NotFoundError(Exception):
    pass

@app.exception_handler(NotFoundError)
async def not_found_handler(request: Request, exc: NotFoundError):
    return JSONResponse(status_code=404, content={"error": str(exc)})
```

---

**Setup:** An endpoint with pagination query params and a path id.

**Solution:**
```python
from fastapi import FastAPI, Path, Query

app = FastAPI()

@app.get("/posts/{post_id}")
def get_post(
    post_id: int = Path(ge=1),
    include_comments: bool = Query(False),
    page: int = Query(1, ge=1),
    limit: int = Query(20, ge=1, le=100),
):
    post = db.get_post(post_id)
    if not post:
        raise HTTPException(status_code=404, detail="Post not found")
    if include_comments:
        post["comments"] = db.get_comments(post_id, page=page, limit=limit)
    return post
```

**Key insight:** every input is validated and self-documenting. The docs page shows the path constraint (`>= 1`), the query defaults, and example values — clients can't send `post_id=0` or `limit=10000` without a 422.

---

**Setup:** A search endpoint with a required query `q` and optional filters.

**Solution:**
```python
@app.get("/search")
def search(
    q: str = Query(min_length=2, max_length=200),     # required
    category: str | None = None,
    min_price: float | None = Query(None, ge=0),
    max_price: float | None = Query(None, ge=0),
):
    results = db.search(
        q=q,
        category=category,
        min_price=min_price,
        max_price=max_price,
    )
    return {"results": results, "count": len(results)}

# /search?q=react → works (optional filters absent)
# /search → 422 (q is required)
```

**Key insight:** `q` has no default → required. Every filter is `None` by default → optional. Numeric bounds reject nonsense (`min_price=-5`). The signature alone defines the full API surface.

---

**Setup:** Why does `/users/me` stop working once you add `/users/{user_id}` after it?

**Solution:** FastAPI matches routes in declaration order. If `{user_id}` is declared first, `me` matches it and fails validation (422 — `me` isn't an int). Declaring the literal `/users/me` first gives it priority; the parameterized route only catches what doesn't match earlier routes.

**Key insight:** route resolution is ordered — literal paths before parameterized ones with the same prefix. Same rule as Express and most routers.

---

## Practice (try before peeking)

1. How does FastAPI know a parameter is a query param vs a body?
2. How do you make a query parameter required?
3. Why raise `HTTPException` instead of returning `None` on a missing resource?

<details><summary>Answers</summary>

1. By its declaration: parameters that appear in the path string are path params; Pydantic models are the body; other scalar types (with defaults or not) are query params. `Header`/`Cookie`/`Path`/`Query`/`Body` make it explicit when the default inference isn't what you want.
2. Give it no default — `q: str` or `q: str = Query(...)` (Ellipsis). A missing required query param returns 422 with a message naming the field.
3. `HTTPException` produces the correct *HTTP semantics* — a 404 status and a structured `detail` in the body, which the docs and clients understand. Returning `None` would send a 200 with a null body — the client can't distinguish "not found" from "found nothing."

</details>

---

**Common traps:**
- Ordering `/users/me` after `/users/{user_id}` — route hijack + 422s
- Forgetting a default → accidentally required query params
- No bounds on `page`/`limit`/`id` — allow garbage values
- Using `Body(embed=True)` for a single model when you don't need the wrapping key
- Returning raw DB rows instead of validating/structuring the response

---
