# FastAPI Lifespan Cheat Sheet

## What is Lifespan?
Lifespan is the modern FastAPI mechanism to manage **application startup and shutdown** logic.
It replaces the deprecated `@app.on_event("startup")` and `@app.on_event("shutdown")`.

Use it for:
- Configuration validation
- Initializing global infrastructure (DB engines, clients)
- Graceful shutdown / cleanup

---

## Basic Structure

```python
from fastapi import FastAPI
from contextlib import asynccontextmanager

@asynccontextmanager
async def lifespan(app: FastAPI):
    # Startup logic
    yield
    # Shutdown logic

app = FastAPI(lifespan=lifespan)
```

---

## Startup Phase (Before `yield`)

Runs **once**, before the app starts serving requests.

Typical uses:
- Load and validate settings
- Initialize database engines
- Initialize clients (Redis, S3, etc.)

Example:

```python
@asynccontextmanager
async def lifespan(app: FastAPI):
    _ = settings.POSTGRES_APP_USER  # fail fast
    _ = settings.POSTGRES_APP_USER_PASSWORD
    yield
```

If an exception is raised here → **the app does not start**.

---

## Shutdown Phase (After `yield`)

Runs when the app is stopping.

Typical uses:
- Dispose DB engines
- Close async clients
- Flush buffers / metrics

Example:

```python
@asynccontextmanager
async def lifespan(app: FastAPI):
    yield
    await engine.dispose()
```

---

## Async-Friendly (Important)

Lifespan fully supports async operations:

```python
@asynccontextmanager
async def lifespan(app: FastAPI):
    await init_async_resources()
    yield
    await cleanup_async_resources()
```

---

## What NOT to do in Lifespan

❌ Run database migrations  
❌ Execute application queries  
❌ Depend on request context  
❌ Put business logic

Lifespan is **infrastructure-only**.

---

## Comparison: Old vs New

### ❌ Deprecated

```python
@app.on_event("startup")
async def startup():
    ...
```

### ✅ Correct

```python
app = FastAPI(lifespan=lifespan)
```

---

## Recommended Pattern (Clean Architecture)

- `infra/settings.py` → Pydantic Settings
- `infra/persistence/...` → engines & sessions
- `main.py` → lifespan orchestration only

Lifespan = orchestration, not logic.

---

## Mental Model

Think of lifespan as:

> “Bootloader for your backend.”

If it fails → the system never starts.

---

## TL;DR

- Lifespan replaces startup/shutdown events
- Use it to validate config and init infra
- Fail fast, no retries
- Async-safe
- Clean and explicit lifecycle boundaries
