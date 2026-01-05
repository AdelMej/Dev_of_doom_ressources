# 19 — FastAPI Lifespan, Startup & Shutdown Cheat Sheet

This cheat sheet explains **how FastAPI boots**, where to initialize resources, and how to shut them down cleanly.
This is critical for DB engines, pools, caches, schedulers, and background workers.

---

## 1. Old way (still valid): startup / shutdown events

```python
from fastapi import FastAPI

app = FastAPI()

@app.on_event("startup")
async def on_startup():
    print("App starting")

@app.on_event("shutdown")
async def on_shutdown():
    print("App stopping")
```

✅ Simple  
❌ Harder to share state cleanly  
❌ Deprecated style long-term (still supported)

---

## 2. Recommended way: Lifespan context manager

```python
from fastapi import FastAPI
from contextlib import asynccontextmanager

@asynccontextmanager
async def lifespan(app: FastAPI):
    # startup
    print("Startup")
    yield
    # shutdown
    print("Shutdown")

app = FastAPI(lifespan=lifespan)
```

✅ Explicit lifecycle  
✅ Clean resource ownership  
✅ Future-proof

---

## 3. What belongs in lifespan?

### Good candidates
- Database engine creation
- Connection pools
- Redis clients
- Message brokers
- Schedulers
- External SDK initialization

### Bad candidates
- Business logic
- Request-specific logic
- Authorization
- Validation

---

## 4. Example: Async SQLAlchemy engine

```python
from sqlalchemy.ext.asyncio import create_async_engine

@asynccontextmanager
async def lifespan(app: FastAPI):
    engine = create_async_engine(DATABASE_URL)
    app.state.engine = engine

    yield

    await engine.dispose()
```

Access later:
```python
engine = request.app.state.engine
```

---

## 5. Example: Redis client

```python
import redis.asyncio as redis

@asynccontextmanager
async def lifespan(app: FastAPI):
    redis_client = redis.from_url("redis://localhost")
    app.state.redis = redis_client

    yield

    await redis_client.close()
```

---

## 6. Sharing state safely

Use `app.state`:

```python
app.state.db_engine
app.state.redis
app.state.settings
```

❌ Do NOT use globals  
❌ Do NOT re-create per request  

---

## 7. Lifespan + dependency injection

```python
from fastapi import Depends, Request

def get_engine(request: Request):
    return request.app.state.engine
```

Then:
```python
def route(engine = Depends(get_engine)):
    ...
```

---

## 8. Multiple resources pattern

```python
@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.engine = create_engine()
    app.state.redis = create_redis()
    app.state.http = create_http_client()

    yield

    await app.state.http.aclose()
    await app.state.redis.close()
    await app.state.engine.dispose()
```

Order matters 🔥  
Shutdown = reverse of startup

---

## 9. Lifespan vs Background Tasks

| Use case | Tool |
|--------|------|
App boot logic | Lifespan |
Per-request async work | async/await |
Fire-and-forget | BackgroundTasks |
Heavy jobs | Worker (Celery, RQ) |

---

## 10. Lifespan in tests

```python
from fastapi.testclient import TestClient

def test_app():
    with TestClient(app) as client:
        response = client.get("/")
```

✅ Lifespan runs automatically

---

## 11. Common mistakes

❌ Creating DB engine in dependencies  
❌ Opening connections per request  
❌ Putting auth logic in lifespan  
❌ Using globals instead of app.state  

---

## 12. Mental model

> Lifespan = process lifecycle  
> Dependencies = request lifecycle  
> Services = business lifecycle  

---

## 13. Rule of thumb

If it must:
- exist once per app
- be reused across requests
- be closed cleanly

➡️ **It belongs in lifespan**

---

## TL;DR

- Use `lifespan` for startup/shutdown
- Store shared resources in `app.state`
- Inject them via dependencies
- Never mix lifespan with business logic

---

Next good topics:
- Middleware architecture
- Rate limiting patterns
- JWT refresh & rotation
- Config management (12-factor)
