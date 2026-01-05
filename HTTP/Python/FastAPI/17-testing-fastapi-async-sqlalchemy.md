# 17 — Testing FastAPI + Async SQLAlchemy Cheat Sheet

This cheat sheet explains **how to test FastAPI apps using async SQLAlchemy** without flakiness, hidden state, or SQLite lies.

---

## 1. Testing goals (be explicit)

Good backend tests should:
- be deterministic
- isolate side effects
- run fast locally
- reflect production behavior

Async does not change these goals — it just adds footguns.

---

## 2. Test stack (recommended)

```text
pytest
pytest-asyncio
httpx (AsyncClient)
```

Install:
```bash
pip install pytest pytest-asyncio httpx
```

---

## 3. Async test basics

```python
import pytest

@pytest.mark.asyncio
async def test_example():
    assert True
```

Never use `asyncio.run()` inside tests.

---

## 4. App client (FastAPI)

```python
from httpx import AsyncClient

async with AsyncClient(app=app, base_url="http://test") as client:
    resp = await client.get("/health")
```

This runs **in-memory** — no real HTTP server.

---

## 5. Database strategy (THIS MATTERS)

### ❌ Wrong approach
- sharing dev DB
- relying on SQLite behavior
- global sessions

### ✅ Correct approaches
Choose ONE:

1. SQLite per test (fast, limited)
2. Transaction rollback per test
3. Ephemeral Postgres (Docker / CI)

---

## 6. SQLite test setup (simple, dev-only)

```python
DATABASE_URL = "sqlite+aiosqlite:///:memory:"
```

Create schema once per test session.

⚠️ Does NOT test concurrency or isolation correctly.

---

## 7. Postgres in CI (recommended)

Use:
- Docker
- Testcontainers
- GitHub Actions services

Apply:
```bash
alembic upgrade head
```

Before running tests.

---

## 8. DB session override (FastAPI)

Override dependency:

```python
app.dependency_overrides[get_db] = get_test_db
```

Example:

```python
async def get_test_db():
    async with TestSessionLocal() as session:
        yield session
```

This is the **key** to clean tests.

---

## 9. Transaction rollback per test (advanced)

```python
async with engine.begin() as conn:
    await conn.run_sync(Base.metadata.create_all)
    async with AsyncSession(bind=conn) as session:
        async with session.begin():
            yield session
            await session.rollback()
```

Keeps DB clean without re-creating schema.

---

## 10. Testing repositories

Call them directly:

```python
repo = UserRepository(session)
user = await repo.get_by_email("a@b.com")
```

No FastAPI involved.

---

## 11. Testing services

```python
service = AuthService(repo)
await service.register(data)
```

Mock repositories if needed.

Services should not depend on HTTP.

---

## 12. Testing routers (integration)

```python
resp = await client.post(
    "/auth/register",
    json={"email": "...", "password": "..."}
)
```

This tests:
- routing
- dependencies
- validation
- serialization

---

## 13. Auth testing (JWT)

Generate test tokens:
- same secret
- short expiry
- fake users

Attach:
```python
headers={"Authorization": f"Bearer {token}"}
```

---

## 14. Common async test footguns

❌ Forgetting `await`
❌ Mixing sync + async clients
❌ Shared DB state
❌ Order-dependent tests
❌ SQLite-only confidence

---

## 15. What NOT to mock

Do NOT mock:
- FastAPI internals
- SQLAlchemy core
- validation logic

Mock boundaries only.

---

## 16. Mental model (lock this in)

- Repo tests → DB correctness
- Service tests → business rules
- Router tests → HTTP glue
- CI tests → production realism

---

## 17. Final truth

> If your tests only pass on SQLite,
> they are lying to you.

Good tests make refactors boring.
