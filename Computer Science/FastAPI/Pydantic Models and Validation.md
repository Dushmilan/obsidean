# Pydantic Models & Validation

Pydantic is the validation engine under FastAPI. A **`BaseModel`** subclass declares fields with types; Pydantic parses, validates, coerces, and serializes them. Beyond basics: **field constraints** (`Field`), **validators** (custom logic), **nested models**, and `model_dump`/`model_validate` for explicit conversion.

**The Intuition:** A Pydantic model is a *form with a bouncer*. Each field has a type — the bouncer checks what arrives, coerces near-misses (string `"42"` → int `42`), and turns away anything that doesn't fit (with a receipt listing exactly what was wrong). The model instance is *guaranteed* to be valid — you never check types again after it's built.

## Declaring models

```python
from pydantic import BaseModel, Field, EmailStr

class User(BaseModel):
    id: int
    name: str
    email: EmailStr                      # validated email format
    age: int | None = None               # optional field
    created_at: datetime = datetime.now  # default callable → evaluated per instance
```

**Key behaviors:**
- **Coercion** — `"42"` → `42`, `"true"` → `True` where the type allows
- **Defaults** — `= None` makes it optional; callables run per instance
- **Strictness mode** — disable coercion with `model_config = ConfigDict(strict=True)` when you need it
- **Extra fields** — ignored by default; `model_config = ConfigDict(extra="forbid")` to reject them

## Field constraints

```python
from pydantic import BaseModel, Field

class Product(BaseModel):
    name: str = Field(min_length=1, max_length=100)
    price: float = Field(gt=0)                        # > 0
    quantity: int = Field(ge=0, le=10_000)            # 0 <= q <= 10000
    sku: str = Field(pattern=r"^[A-Z]{3}-\d{4}$")     # regex
    tags: list[str] = Field(default_factory=list)     # mutable default!
```

**The mutable-default trap:**
```python
# BAD — shared across instances:
def f(items: list[str] = []): ...

# GOOD — default_factory creates a fresh list per instance:
tags: list[str] = Field(default_factory=list)
```

## Validators — custom logic

```python
from pydantic import BaseModel, field_validator, model_validator

class Booking(BaseModel):
    check_in: date
    check_out: date

    @field_validator("check_out")
    @classmethod
    def check_out_after_in(cls, v, info):
        if info.data.get("check_in") and v <= info.data["check_in"]:
            raise ValueError("check_out must be after check_in")
        return v

    @model_validator(mode="after")
    @classmethod
    def whole_object_rule(cls, values):
        # needs MULTIPLE fields at once — model-level:
        if values["check_out"].weekday() == 5:      # Saturday
            raise ValueError("No Saturday check-outs")
        return values
```

**Two levels:**
- `@field_validator` — one field (gets `info.data` to peek at already-validated siblings)
- `@model_validator(mode="after")` — the whole validated model, for cross-field rules

## Nested models

```python
class Address(BaseModel):
    street: str
    city: str
    zip_code: str = Field(pattern=r"^\d{5}$")

class Customer(BaseModel):
    name: str
    address: Address                      # nested model
    orders: list[Order] = Field(default_factory=list)

# JSON:
# {
#   "name": "Ada",
#   "address": {"street": "1 Main St", "city": "Metropolis", "zip_code": "12345"},
#   "orders": [...]
# }
```

Nesting composes validation down the tree — a bad zip inside the address fails with a path pointing exactly at `address.zip_code`.

## Explicit conversion

```python
# dict/JSON → model:
user = User.model_validate(raw_dict)       # same validation FastAPI does for bodies
# model → dict/JSON:
data = user.model_dump()                   # {"id": 1, "name": "Ada", ...}
json_str = user.model_dump_json()          # serialized JSON string

# Build from partial data:
created = User.model_validate({**raw, "id": next_id})
```

**`model_dump` vs `model_dump_json`:** the former gives a Python dict (for internal use), the latter a JSON string (for responses/storage). FastAPI does the JSON conversion for you on responses — `model_dump` is for when you need the dict yourself.

## Using models inside FastAPI

```python
from fastapi import FastAPI, HTTPException

class Item(BaseModel):
    name: str = Field(min_length=1)
    price: float = Field(gt=0)

items_db: dict[int, Item] = {}

@app.post("/items", status_code=201)
def create_item(item: Item) -> Item:        # body → validated Item; return → JSON
    item_id = len(items_db) + 1
    items_db[item_id] = item
    return item

@app.get("/items/{item_id}")
def read_item(item_id: int) -> Item:
    item = items_db.get(item_id)
    if not item:
        raise HTTPException(status_code=404, detail="Item not found")
    return item
```

---

**Setup:** Validate an email with a custom domain check.

**Solution:**
```python
class Signup(BaseModel):
    email: EmailStr
    password: str = Field(min_length=8)

    @field_validator("email")
    @classmethod
    def company_domain_only(cls, v):
        if not v.endswith("@acme.com"):
            raise ValueError("Only @acme.com emails allowed")
        return v
```

**Key insight:** `EmailStr` handles format; the validator enforces the business rule. Combined, an invalid format fails before your rule even runs — layered validation with clear error messages.

---

**Setup:** A product model where price can't be negative and the discount can't exceed 100%.

**Solution:**
```python
class Product(BaseModel):
    name: str
    price: float = Field(gt=0)
    discount_percent: float = Field(ge=0, le=100)

    @property
    def final_price(self) -> float:
        return round(self.price * (1 - self.discount_percent / 100), 2)
```

**Key insight:** constraint *fields* handle the two ranges declaratively; the derived value is a `@property`, not stored state — you never risk it being out of sync. (FastAPI can even include properties in responses via `response_model` with `model_config = ConfigDict(from_attributes=True)` when needed.)

---

**Setup:** Why `tags: list[str] = []` causes bugs and `default_factory=list` doesn't?

**Solution:** A mutable default (`[]`) is created *once* at class definition and *shared* by every instance — appending to one instance's list appends to all. `default_factory=list` calls `list()` per instance, giving each a fresh list. Pydantic raises errors for mutable defaults; `Field(default_factory=...)` is the fix.

**Key insight:** "don't use mutable defaults" is a general Python rule (`def f(x=[])`), and Pydantic turns it from a silent bug into an explicit `default_factory` contract.

---

## Practice (try before peeking)

1. `Field(ge=0)` vs `Field(gt=0)` — what's the difference?
2. When do you need a `model_validator` instead of a `field_validator`?
3. What's `model_dump()` for, if FastAPI serializes responses anyway?

<details><summary>Answers</summary>

1. `ge=0` allows `0` (greater-or-equal); `gt=0` requires strictly positive (0 is rejected). Pick based on whether zero is a valid value — e.g., quantity `ge=0`, price `gt=0`.
2. When the rule needs *multiple fields at once* — e.g., `check_out > check_in`, a total that must not exceed a limit. A field validator only sees its own field (plus already-validated siblings via `info.data`); a model validator sees the whole validated object.
3. For converting a model to a Python dict when *you* need the dict — storing in a DB layer, passing to non-JSON code, building payloads. FastAPI serializes responses for you, but internal code often needs the plain dict.

</details>

---

**Common traps:**
- Mutable defaults (`= []`) — shared state across instances; use `default_factory`
- Relying on coercion when you need strict types — set `strict=True` for that model
- Field validators that try to read fields validated *later* — order matters; use a model validator for cross-field logic
- Not constraining `Field` (unbounded strings, negative quantities) — validate at the boundary
- Forgetting `min_length` on names, `pattern` on codes — the small rules that catch real garbage

---
