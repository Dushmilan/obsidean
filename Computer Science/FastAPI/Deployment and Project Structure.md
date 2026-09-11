# Deployment & Project Structure

A FastAPI app ships as a **package** (a `src` layout with `main.py` as the entry point), runs under **uvicorn** behind a **reverse proxy** (Nginx/Traefik/Caddy), and deploys as a **Docker container** with environment-driven config. Structure and deployment decisions determine how maintainable and operatable the app is. See [[Computer Science/Node/Proxies|Proxies]] for deep dive on reverse proxy mechanics.

**The Intuition:** The project structure is the apartment's floor plan — `main.py` is the front door, `routers/` are the rooms, `schemas/` the coat closet (where inputs/outputs get dressed), `models/` the storage room. Deployment is the building's utilities — uvicorn is the boiler (runs the app), the reverse proxy is the doorman (routes traffic, terminates HTTPS), Docker is the moving box that ships the whole apartment identical to every environment.

## The standard layout

```text
app/
  __init__.py
  main.py               # app factory + middleware + router registration
  config.py             # Settings (env-driven)
  database.py           # engine, SessionLocal, get_db dependency
  models/               # SQLAlchemy models
    __init__.py
    user.py
    post.py
  schemas/              # Pydantic request/response models
    __init__.py
    user.py
    post.py
  routers/              # APIRouter modules
    __init__.py
    users.py
    posts.py
    auth.py
  services/             # business logic (optional — keeps routers thin)
    email.py
  utils.py
tests/
  conftest.py
  test_users.py
  test_auth.py
pyproject.toml          # dependencies & tooling config
.env.example            # env var names (no values)
Dockerfile
docker-compose.yml
```

## The app factory

```python
# app/main.py
from fastapi import FastAPI
from app.routers import users, posts, auth
from app.config import settings

def create_app() -> FastAPI:
    app = FastAPI(title=settings.app_name, version=settings.version)

    # middleware order matters (outermost first):
    app.add_middleware(TrustedHostMiddleware, allowed_hosts=settings.allowed_hosts)
    app.add_middleware(CORSMiddleware, allow_origins=settings.cors_origins, ...)

    app.include_router(auth.router)
    app.include_router(users.router)
    app.include_router(posts.router)

    @app.get("/health")
    def health():
        return {"status": "ok"}

    return app

app = create_app()     # the ASGI object uvicorn imports
```

**Why a factory:** tests can call `create_app()` with different settings; the module-level `app` is what uvicorn imports. Health endpoint = load-balancer/probe target.

## Running with uvicorn

```bash
# Dev — reload on code changes:
uvicorn app.main:app --reload --port 8000

# Prod — explicit workers, no reload:
uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 4

# Gunicorn + uvicorn workers (classic production combo):
gunicorn app.main:app -w 4 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

**Workers:** multiple processes each running the event loop — scale CPU-bound work and share nothing (DB connections pool per worker). For *async-first* apps, one worker often suffices; add workers for CPU-bound load.

## Configuration via env

```python
# app/config.py
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    app_name: str = "My API"
    environment: str = "development"
    database_url: str
    secret_key: str
    cors_origins: list[str] = ["http://localhost:5173"]
    allowed_hosts: list[str] = ["localhost"]

    model_config = SettingsConfigDict(env_file=".env")

settings = Settings()    # raises at import if database_url/secret_key missing
```

```bash
# .env.example  (committed — names only)
DATABASE_URL=postgresql+psycopg://user:pass@localhost:5432/appdb
SECRET_KEY=change-me
CORS_ORIGINS=["https://myapp.com"]
```

**The rule:** every environment variable has a name in `.env.example`, a default (safe for dev), and validation via the Settings model — deploy to prod by setting env vars, never by editing code.

## Docker

```dockerfile
# Dockerfile
FROM python:3.12-slim

WORKDIR /app

# Install deps first — layer caching: only rebuilds when requirements change
COPY pyproject.toml requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY app ./app
COPY alembic ./alembic        # migrations

EXPOSE 8000
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```yaml
# docker-compose.yml
services:
  api:
    build: .
    ports: ["8000:8000"]
    env_file: .env
    depends_on:
      db:
        condition: service_healthy
    command: uvicorn app.main:app --host 0.0.0.0 --port 8000

  db:
    image: postgres:16
    environment:
      POSTGRES_DB: appdb
      POSTGRES_USER: app
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes: ["pgdata:/var/lib/postgresql/data"]
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app"]
      interval: 5s
      retries: 5

volumes:
  pgdata:
```

**Why this works:** the container *is* the artifact — same image in dev/staging/prod. Env vars (`.env` / injected by the orchestrator) change behavior; code doesn't. `depends_on` + healthcheck means the API waits for a ready DB.

## Migrations in deployment

```bash
# Apply migrations BEFORE starting new code (or as a separate job):
alembic upgrade head
# In compose: an init step / entrypoint that runs migrations then starts uvicorn
```

**The order matters:** schema first, then the new app code that assumes it. Rolling deploys with migrations need forward+backward-compatible migrations.

## Production checklist

```text
☐ HTTPS — terminated at the reverse proxy (TLS certs)
☐ Reverse proxy — Nginx/Traefik/Caddy in front of uvicorn
☐ Health checks — /health for load balancer & container probes
☐ Migrations — alembic upgrade head as a deploy step
☐ Logging — structured, with request IDs; ship to a log aggregator
☐ Metrics — Prometheus endpoint (/metrics) for CPU, latency, error rates
☐ Secrets — env vars or a secret manager; never in the image
☐ Backups — database scheduled backups + restore drills
☐ Rate limiting — login/API abuse protection
☐ Resource limits — container CPU/memory caps
```

---

**Setup:** Take a working FastAPI app and make it deployable.

**Solution:**
1. Restructure into the `app/` package layout (routers, schemas, models, config).
2. Move all config to `Settings` (env-driven) with `.env.example` committed.
3. Add `alembic` migrations for the schema.
4. Write the `Dockerfile` + `docker-compose.yml` (api + postgres).
5. Add `/health` and `/metrics` endpoints.
6. Test the flow: `docker compose up` → migrations run → API responds → `/docs` reachable.

**Key insight:** deployability isn't a feature you bolt on at the end — it's the structure (config via env, package layout, health endpoint) and the artifact (container) that let the same code run anywhere.

---

**Setup:** Scale the API to 4 workers behind a reverse proxy.

**Solution:**
```bash
# Compose:
api:
  build: .
  command: uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 4
```
The reverse proxy load-balances to the single service; uvicorn runs 4 worker processes. Sessions/pools are per-worker — share nothing across workers.

**Key insight:** horizontal scaling in containers = run more replicas; workers = vertical scaling within one container. Both need *stateless* app code (session state in the DB/Redis, not in memory) or sticky sessions.

---

**Setup:** Why does the API start before the database and crash?

**Solution:** `depends_on` in compose starts the DB container first, but *ready* isn't the same as *started*. The fix is a `healthcheck` on the DB (`pg_isready`) plus `condition: service_healthy` on the API — the API only starts once the DB is accepting connections. Plus retries in the app (or a small wait loop) for robustness.

**Key insight:** startup order in orchestration is about *readiness*, not just launch order. Healthchecks are how the orchestrator knows "ready to accept traffic."

---

## Practice (try before peeking)

1. Why a package layout (`app/...`) instead of everything in one `main.py`?
2. What's the difference between uvicorn `--reload` and `--workers`?
3. Why does the container run migrations as a separate step?

<details><summary>Answers</summary>

1. Because the app grows — routers, models, schemas, tests, and config in one file become unmaintainable fast, and imports/circulars bite. The package layout separates concerns, makes `import app.routers.users` clean, and keeps the entry point (`main.py`) tiny.
2. `--reload` is a *dev* feature — watches files and restarts on change, single process. `--workers` is a *production* scaling lever — N independent processes serving traffic. Never use `--reload` in prod.
3. So schema changes apply before the new code runs, and so migrations run exactly once (not once per worker). If each worker ran `alembic upgrade head`, they'd race. A dedicated migration step/entrypoint keeps it deterministic.

</details>

---

**Common traps:**
- Running with `--reload` in production — restarts under load, single process
- Hardcoded config (secrets, URLs) in code — impossible to deploy safely
- No health endpoint — load balancers/containers can't probe readiness
- Migrations run per-worker — race conditions; use a single step
- No resource limits on the API container — a runaway worker kills the host
- Forgetting `.env.example` — nobody knows what vars the app needs

---
