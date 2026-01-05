# 14 — Async SQLAlchemy Cheat Sheet

This cheat sheet summarizes **how to correctly use SQLAlchemy in async mode** with FastAPI (or any async framework).

---

## 1. When to use async SQLAlchemy

Use async SQLAlchemy when:
- Your app is async (FastAPI, Starlette)
- You expect concurrent I/O (many requests waiting on DB)
- You plan to use Postgres/MySQL in prod

Async **does NOT** make queries faster — it improves concurrency.

---

## 2. Supported async drivers

| Database   | Driver      | URL prefix |
|------------|-------------|-----------|
| SQLite     | aiosqlite   | sqlite+aiosqlite |
| Postgres   | asyncpg     | postgresql+asyncpg |
| MySQL      | aiomysql    | mysql+aiomysql |

---

## 3. Engine setup

```python
from sqlalchemy.ext.asyncio import create_async_engine

engine = create_async_engine(
    DATABASE_URL,
    echo=False,
    pool_pre_ping=True,
)
```

SQLite dev only:
```python
connect_args={"check_same_thread": False}
```

---

## 4. Async session factory

```python
from sqlalchemy.ext.asyncio import async_sessionmaker

AsyncSessionLocal = async_sessionmaker(
    engine,
    expire_on_commit=False,
)
```

Never use `sessionmaker()` (sync).

---

## 5. DB session dependency (FastAPI)

```python
async def get_db():
    async with AsyncSessionLocal() as session:
        yield session
```

This is **HTTP lifecycle logic**, not business logic.

---

## 6. Repository pattern (async-safe)

```python
class UserRepository:
    def __init__(self, session: AsyncSession):
        self.session = session

    async def get_by_id(self, user_id: int):
        stmt = select(User).where(User.id == user_id)
        result = await self.session.execute(stmt)
        return result.scalar_one_or_none()
```

Repositories:
- async
- framework-agnostic
- no Depends()

---

## 7. Executing queries

```python
result = await session.execute(stmt)
```

Access helpers:
```python
scalar_one()
scalar_one_or_none()
scalars().all()
first()
```

---

## 8. Inserts / updates / deletes

```python
session.add(entity)
await session.commit()
await session.refresh(entity)
```

Rollback on errors:
```python
await session.rollback()
```

---

## 9. Transactions

### Automatic (recommended)

```python
async with session.begin():
    session.add(obj)
```

### Manual

```python
try:
    ...
    await session.commit()
except:
    await session.rollback()
    raise
```

---

## 10. Async rules (DO NOT BREAK)

❌ Do not use sync Session  
❌ Do not call blocking DB code  
❌ Do not share sessions across requests  
❌ Do not hide sessions inside services  

---

## 11. SQLite async reality

- Uses threads internally
- Not real async I/O
- Safe for dev
- Never for prod

---

## 12. Switching databases

Only change:
```python
DATABASE_URL
```

No refactor required.

---

## 13. Testing async DB code

Use:
```python
pytest
pytest-asyncio
```

Example:
```python
@pytest.mark.asyncio
async def test_repo():
    ...
```

---

## 14. Mental model (lock this in)

- AsyncSession = unit of work
- Repository = DB boundary
- Service = business logic
- Dependency = lifecycle / HTTP glue

---

## 15. Final truth

Async SQLAlchemy is:
- boring
- explicit
- predictable

That’s why it scales.
