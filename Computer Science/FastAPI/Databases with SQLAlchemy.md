# Databases with SQLAlchemy

SQLAlchemy is Python's ORM. In FastAPI it's wired through **sessions** created per-request (via dependency injection) and **models** declared as Python classes. Modern FastAPI stacks use **SQLAlchemy 2.0** — `DeclarativeBase`, typed `Mapped`/`mapped_column`, and either sync or async engines.

**The Intuition:** SQLAlchemy is a translator between Python objects and database rows. Your `User` class is the table schema *and* the row type — you create, query, and update with Python objects, and SQLAlchemy emits the SQL. A **session** is the conversation with the database: it tracks what you've changed and flushes it as a unit.

## Setup — engine & session

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, DeclarativeBase

DATABASE_URL = "postgresql+psycopg://user:pass@localhost:5432/appdb"

engine = create_engine(DATABASE_URL, pool_pre_ping=True)
SessionLocal = sessionmaker(bind=engine, autoflush=False)

class Base(DeclarativeBase):
    pass
```

**Key knobs:**
- `pool_pre_ping=True` — tests connections before use (survives dropped DB connections)
- `autoflush=False` — don't surprise-flush mid-query; flush explicitly
- The engine is app-wide; sessions are per-request

## Models

```python
from sqlalchemy import String, Integer, ForeignKey, DateTime, func
from sqlalchemy.orm import Mapped, mapped_column, relationship

class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)
    email: Mapped[str] = mapped_column(String(255), unique=True, index=True)
    name: Mapped[str] = mapped_column(String(100))
    created_at: Mapped[datetime] = mapped_column(DateTime, server_default=func.now())

    posts: Mapped[list["Post"]] = relationship(back_populates="author")

class Post(Base):
    __tablename__ = "posts"

    id: Mapped[int] = mapped_column(primary_key=True)
    title: Mapped[str] = mapped_column(String(200))
    author_id: Mapped[int] = mapped_column(ForeignKey("users.id"))

    author: Mapped[User] = relationship(back_populates="posts")
```

**`Mapped[...]` + `mapped_column`** is SQLAlchemy 2.0's typed style — the Python type *is* the column type for most cases, and type-checkers understand your models.

## The per-request session (dependency)

```python
from fastapi import Depends, FastAPI

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()                     # guaranteed close per request

app = FastAPI()

@app.get("/users/{user_id}")
def get_user(user_id: int, db: Session = Depends(get_db)):
    user = db.get(User, user_id)       # SELECT by PK
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

**Why per-request:** one session per request = one transaction scope, no leaked connections, no cross-request state. The `yield` dependency closes it even when the endpoint raises.

## CRUD with sessions

```python
from sqlalchemy import select

# Create:
user = User(email="ada@example.com", name="Ada")
db.add(user)
db.commit()                    # persist
db.refresh(user)               # load DB-generated values (id, created_at)

# Read:
user = db.get(User, user_id)                       # by PK
users = db.scalars(select(User).limit(20)).all()   # list
by_email = db.scalars(select(User).where(User.email == "ada@example.com")).first()

# Update:
user.name = "Ada Lovelace"
db.commit()

# Delete:
db.delete(user)
db.commit()
```

**The session lifecycle:** `add` stages, `commit` flushes+commits the transaction, `refresh` re-reads DB-side defaults. Queries use SQLAlchemy's `select()` — safe from injection because values are bound parameters.

## Transactions & rollback

```python
def transfer(db: Session, from_id: int, to_id: int, amount: float):
    a = db.get(Account, from_id)
    b = db.get(Account, to_id)
    a.balance -= amount
    b.balance += amount
    try:
        db.commit()                # both changes commit atomically
    except Exception:
        db.rollback()              # neither change persists
        raise
```

One `commit` = one transaction. If anything fails, `rollback()` restores the session state and the DB applies nothing.

## Relationships & eager loading (avoid N+1)

```python
# N+1 — one query for users + one per user for posts:
users = db.scalars(select(User).limit(20)).all()
for u in users:
    for p in u.posts: ...          # lazy load — fires a query per user!

# Eager load — one JOIN query:
from sqlalchemy.orm import selectinload
users = db.scalars(
    select(User).options(selectinload(User.posts)).limit(20)
).all()
```

**The trap:** accessing `u.posts` triggers a lazy SELECT unless you eager-loaded. `selectinload` (or `joinedload`) fetches the relationship in the same query — the fix for the classic N+1 ORM problem.

## Async SQLAlchemy

```python
from sqlalchemy.ext.asyncio import create_async_engine, async_sessionmaker

engine = create_async_engine("postgresql+asyncpg://user:pass@localhost:5432/appdb")
AsyncSessionLocal = async_sessionmaker(engine, expire_on_commit=False)

async def get_db():
    async with AsyncSessionLocal() as db:
        yield db

@app.get("/users/{user_id}")
async def get_user(user_id: int, db: AsyncSession = Depends(get_db)):
    result = await db.get(User, user_id)          # await every DB call
    if not result:
        raise HTTPException(status_code=404, detail="User not found")
    return result
```

**Async =** the DB call awaits without blocking the event loop — needed when your endpoints are `async def` and you care about concurrency. The async engine requires an async driver (`asyncpg`, `aiomysql`).

---

**Setup:** Create and return a user, with Pydantic request/response models.

**Solution:**
```python
class UserCreate(BaseModel):
    email: str
    name: str

class UserOut(BaseModel):
    id: int
    email: str
    name: str
    created_at: datetime
    model_config = ConfigDict(from_attributes=True)   # serialize ORM objects

@app.post("/users", response_model=UserOut, status_code=201)
def create_user(data: UserCreate, db: Session = Depends(get_db)):
    user = User(email=data.email, name=data.name)
    db.add(user)
    db.commit()
    db.refresh(user)                  # get DB-generated id / created_at
    return user
```

**Key insight:** `from_attributes=True` lets Pydantic read attributes off the ORM object directly — no manual dict conversion. The response model also *hides* anything not in it (no password leakage).

---

**Setup:** Paginated user list with a post count per user, without N+1.

**Solution:**
```python
from sqlalchemy import func, select
from sqlalchemy.orm import selectinload

@app.get("/users")
def list_users(
    page: int = Query(1, ge=1),
    limit: int = Query(20, ge=1, le=100),
    db: Session = Depends(get_db),
):
    offset = (page - 1) * limit
    users = db.scalars(
        select(User)
        .options(selectinload(User.posts))
        .order_by(User.created_at.desc())
        .offset(offset)
        .limit(limit)
    ).all()

    total = db.scalar(select(func.count()).select_from(User))

    return {"items": users, "pagination": {"page": page, "limit": limit, "total": total}}
```

**Key insight:** `selectinload` bundles the posts into the query (no N+1); `func.count()` gets the total in one scalar query; pagination is offset/limit at the SQL level — the DB does the filtering, not Python.

---

**Setup:** Why use `db.refresh(user)` after `commit`?

**Solution:** Columns with DB-side defaults (`id` autoincrement, `created_at` server default) aren't known to the session until the row is written — and after `commit`, SQLAlchemy expires attributes by default. `refresh` re-reads the row from the DB so `user.id` and `user.created_at` are populated and usable (e.g., in the response).

**Key insight:** DB-generated values are the DB's business. `refresh` is how you learn what the database decided. (Async sessions: set `expire_on_commit=False` and refresh explicitly.)

---

## Practice (try before peeking)

1. Why a session per request instead of one app-wide session?
2. What's the N+1 problem and how do you fix it here?
3. Sync vs async engine — how do you choose?

<details><summary>Answers</summary>

1. Sessions hold transaction state and track object changes — sharing one across requests leaks transactions and state between users, and connections never release. Per-request sessions scope the transaction to one request and close reliably via the `yield` dependency.
2. Loading a list, then lazily loading each item's relationship triggers 1 + N queries. Fix: `selectinload`/`joinedload` in the original query so the related rows come back in one statement.
3. Async engine when your endpoints are `async def` and you want non-blocking DB I/O (asyncpg). Sync engine when endpoints are `def` — FastAPI threadpools handle it. The driver must match: `asyncpg` for async, `psycopg` for sync.

</details>

---

**Common traps:**
- One app-global session — cross-request state leaks and connection leaks
- Accessing relationships without eager loading — N+1 query storms
- Forgetting `commit` — nothing persists (silently)
- Swallowing commit errors without `rollback` — poisoned session
- Using a sync driver with an async engine (or vice versa) — runtime errors

---
