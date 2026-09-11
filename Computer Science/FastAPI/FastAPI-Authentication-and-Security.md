# Authentication & Security

FastAPI ships with **`OAuth2PasswordBearer`** and friends for the standard flow: client sends username+password, server verifies against a **bcrypt hash**, issues a **JWT**, and protected routes verify it via a dependency. Hardening extras: **HTTPS, security headers, rate limiting, env-based secrets**, and never trusting the client.

**The Intuition:** Login is a guarded door. The client knocks with credentials → the server checks the bcrypt *hash* (the ground-up password, never the original), then hands out a *signed pass* (JWT). Every subsequent request just shows the pass — the server verifies the signature without asking for the password again. The pass has an expiry, so it can't be reused forever.

## Password hashing with bcrypt

```python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"], deprecated="auto")

def hash_password(password: str) -> str:
    return pwd_context.hash(password)          # salted + slow — never store plaintext

def verify_password(plain: str, hashed: str) -> bool:
    return pwd_context.verify(plain, hashed)
```

**The rules:** never store plaintext; bcrypt is deliberately slow (salted, CPU-costly) so offline brute-force is impractical; `verify` compares in constant time.

## The OAuth2 password flow

```python
from fastapi import Depends, FastAPI, HTTPException, status
from fastapi.security import OAuth2PasswordBearer, OAuth2PasswordRequestForm
from jose import JWTError, jwt
from datetime import datetime, timedelta

SECRET_KEY = "your-32-plus-char-random-secret"     # from env, never in code!
ALGORITHM = "HS256"
ACCESS_TOKEN_EXPIRE_MINUTES = 30

oauth2_scheme = OAuth2PasswordBearer(tokenUrl="/token")

def create_access_token(data: dict) -> str:
    payload = data.copy()
    payload.update({"exp": datetime.utcnow() + timedelta(minutes=ACCESS_TOKEN_EXPIRE_MINUTES)})
    return jwt.encode(payload, SECRET_KEY, algorithm=ALGORITHM)

def decode_token(token: str) -> dict:
    try:
        return jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
    except JWTError:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Could not validate credentials",
            headers={"WWW-Authenticate": "Bearer"},
        )
```

**Token endpoint:**
```python
@app.post("/token")
def login(form: OAuth2PasswordRequestForm = Depends(), db: Session = Depends(get_db)):
    user = db.scalars(select(User).where(User.username == form.username)).first()
    if not user or not verify_password(form.password, user.password_hash):
        raise HTTPException(status_code=401, detail="Incorrect username or password")
    return {"access_token": create_access_token({"sub": str(user.id)}), "token_type": "bearer"}
```

**The flow:**
```text
POST /token  {username, password}  →  200 {access_token, token_type}
Authorization: Bearer <access_token>  on every protected request
```

`OAuth2PasswordRequestForm` parses the standard `application/x-www-form-urlencoded` login body; `tokenUrl="/token"` tells the docs UI exactly where to get tokens (the "Authorize" button works for free).

## The auth dependency

```python
def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: Session = Depends(get_db),
) -> User:
    payload = decode_token(token)                # 401 on bad/expired
    user = db.get(User, int(payload["sub"]))
    if not user:
        raise HTTPException(status_code=401, detail="User not found")
    return user


@app.get("/users/me")
def read_me(user: User = Depends(get_current_user)):
    return user


# Role check — compose dependencies:
def require_admin(user: User = Depends(get_current_user)):
    if user.role != "admin":
        raise HTTPException(status_code=403, detail="Admins only")
    return user

@app.delete("/admin/users/{user_id}")
def delete_user(user_id: int, admin: User = Depends(require_admin)):
    ...
```

**401 vs 403:** invalid/missing credentials → 401; valid credentials but insufficient role → 403. FastAPI's `oauth2_scheme` automatically returns 401 when no token is present.

## Security hardening

```python
from fastapi.middleware.cors import CORSMiddleware
from starlette.middleware.trustedhost import TrustedHostMiddleware

# 1. HTTPS in production (terminated at the reverse proxy)
# 2. CORS — explicit origins only:
app.add_middleware(CORSMiddleware, allow_origins=["https://myapp.com"], allow_credentials=True, ...)

# 3. Trusted hosts:
app.add_middleware(TrustedHostMiddleware, allowed_hosts=["myapp.com", "localhost"])
```

```python
# 4. Rate limiting — brute-force protection on /token:
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)
app.state.limiter = limiter

@app.post("/token")
@limiter.limit("5/minute")                # 5 login attempts/min/IP
def login(request: Request, form: OAuth2PasswordRequestForm = Depends()):
    ...
```

## Secrets & config

```python
# .env — never committed
SECRET_KEY=9f8e7d6c...                     # 32+ random bytes
DATABASE_URL=postgresql+psycopg://...

# config.py — fail fast if missing:
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    secret_key: str
    database_url: str
    access_token_expire_minutes: int = 30
    model_config = SettingsConfigDict(env_file=".env")

settings = Settings()                      # raises at boot if required vars missing
```

**The rules:** secrets in env vars (or a vault), `.env` gitignored, commit `.env.example` with names only, rotate and invalidate leaked secrets, never log tokens or passwords.

## Common attack surfaces in FastAPI

```text
SQL injection    — user input in raw SQL → always use SQLAlchemy parameterized queries
Mass assignment  — client sets fields it shouldn't → Pydantic request models whitelist fields
JWT tampering    — forged payload → signature check fails (never trust payload without verify)
Token in URL     — leaks via history/logs → always the Authorization header
Long-lived tokens— stolen token reusable → short expiry + refresh tokens
```

---

**Setup:** Signup + login with hashed passwords and JWT.

**Solution:**
```python
class UserCreate(BaseModel):
    username: str = Field(min_length=3, max_length=50)
    email: EmailStr
    password: str = Field(min_length=8)

class UserOut(BaseModel):
    id: int
    username: str
    email: EmailStr
    model_config = ConfigDict(from_attributes=True)

@app.post("/signup", response_model=UserOut, status_code=201)
def signup(data: UserCreate, db: Session = Depends(get_db)):
    existing = db.scalars(select(User).where(User.username == data.username)).first()
    if existing:
        raise HTTPException(status_code=409, detail="Username taken")
    user = User(username=data.username, email=data.email, password_hash=hash_password(data.password))
    db.add(user)
    db.commit()
    db.refresh(user)
    return user
```

**Key insight:** the request model validates *before* anything hits the DB; the password is hashed at the boundary; the response model can't leak the hash. Login then verifies with `verify_password` and returns a token.

---

**Setup:** Protect a route so only the resource owner can modify it.

**Solution:**
```python
@app.patch("/posts/{post_id}")
def update_post(
    post_id: int,
    data: PostUpdate,
    user: User = Depends(get_current_user),     # authenticated
    db: Session = Depends(get_db),
):
    post = db.get(Post, post_id)
    if not post:
        raise HTTPException(status_code=404, detail="Post not found")
    if post.author_id != user.id:               # ownership check
        raise HTTPException(status_code=403, detail="Not your post")
    for field, value in data.model_dump(exclude_unset=True).items():
        setattr(post, field, value)
    db.commit()
    db.refresh(post)
    return post
```

**Key insight:** auth dependency proves *who*; the ownership check proves *allowed* — two distinct gates. `exclude_unset=True` means only fields the client actually sent get updated (partial PATCH semantics).

---

**Setup:** Why must the JWT secret be long, random, and not in source control?

**Solution:** The signature is HMAC over header+payload using the secret. Anyone with the secret can mint tokens for *any* user (e.g., an admin token). A short/guessable secret is brute-forceable; a committed secret is exposed in every repo clone and history. 32+ random bytes in env only.

**Key insight:** JWT security rests entirely on secret secrecy — it's the crown jewels. Compromise = total impersonation. Rotate it immediately if it ever leaks.

---

## Practice (try before peeking)

1. Why bcrypt for password storage?
2. What does `OAuth2PasswordBearer(tokenUrl="/token")` actually do?
3. 401 vs 403 — which does an expired token produce and why?

<details><summary>Answers</summary>

1. bcrypt is slow and salted — offline guessing costs thousands of attempts/sec instead of millions, and identical passwords hash differently. Fast hashes (MD5/SHA) are designed to be fast — the wrong property for passwords.
2. It's a security scheme that (a) tells FastAPI to read the `Authorization: Bearer` header, (b) returns 401 automatically when the header is missing/invalid, and (c) advertises `tokenUrl` in the docs so the "Authorize" button posts to `/token` and stores the token for interactive testing.
3. An expired/invalid token → 401: the credentials themselves are bad. 403 is only for *valid* credentials lacking permission. An expired token isn't "authenticated but forbidden" — it's "not authenticated at all."

</details>

---

**Common traps:**
- Hardcoding `SECRET_KEY` in source — leaked in every clone
- Storing plaintext passwords — total account compromise on DB leak
- Trusting JWT payload without verifying the signature
- Returning the password hash in responses (no response model / wrong model)
- No rate limit on `/token` — unlimited brute-force attempts
- Logging tokens or full request bodies

---
