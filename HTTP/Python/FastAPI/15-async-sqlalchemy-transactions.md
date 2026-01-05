# 15 — Async SQLAlchemy Transactions & Isolation Cheat Sheet

This cheat sheet explains **how transactions actually work** in async SQLAlchemy and how to avoid subtle data corruption bugs.

---

## 1. What a transaction really is

A transaction guarantees:
- Atomicity (all or nothing)
- Consistency
- Isolation
- Durability

In SQLAlchemy:
> A transaction is tied to a **session**, not a query.

---

## 2. AsyncSession = Unit of Work

```python
async with AsyncSessionLocal() as session:
    ...
```

- One session = one logical unit of work
- Never share a session across requests
- Never store a session globally

---

## 3. The recommended pattern (use this 90% of the time)

```python
async with session.begin():
    session.add(entity)
    session.add(other_entity)
```

Behavior:
- commits automatically on success
- rolls back automatically on exception
- no manual commit needed

This is the **safest pattern**.

---

## 4. Manual transaction control

Only use when you need fine-grained behavior.

```python
try:
    session.add(entity)
    await session.commit()
except Exception:
    await session.rollback()
    raise
```

If you forget rollback → session becomes unusable.

---

## 5. Nested transactions (SAVEPOINT)

```python
async with session.begin():
    ...
    async with session.begin_nested():
        ...
```

Used for:
- partial rollback
- complex workflows
- test isolation

Supported by Postgres, limited in SQLite.

---

## 6. Isolation levels (important)

Isolation controls **what concurrent transactions can see**.

Common levels:
- READ COMMITTED (default, recommended)
- REPEATABLE READ
- SERIALIZABLE

### Setting isolation level

```python
engine = create_async_engine(
    DATABASE_URL,
    isolation_level="READ COMMITTED",
)
```

Postgres default is usually perfect.

---

## 7. SQLite isolation caveat

SQLite:
- locks the whole database on write
- ignores many isolation guarantees
- behaves differently than Postgres

Do NOT rely on SQLite for concurrency semantics.

---

## 8. Transaction boundaries (where they belong)

✅ Repository: NO  
✅ Service: YES  
❌ Router: NO  
❌ Dependency: NO  

Transactions are **business workflows**, not HTTP concerns.

---

## 9. Anti-patterns (do not do this)

❌ Commit inside repository  
❌ Multiple commits per request  
❌ Commit in dependencies  
❌ Implicit commits you don’t control  

---

## 10. Read-only operations

No transaction needed:

```python
result = await session.execute(stmt)
```

SQLAlchemy may still open an implicit one — that’s fine.

---

## 11. Handling DB errors safely

```python
from sqlalchemy.exc import IntegrityError

try:
    ...
except IntegrityError:
    await session.rollback()
    raise
```

Always rollback on DB exceptions.

---

## 12. Testing transactions

Use:
- database rollback per test
- nested transactions
- or recreate schema per test

Never rely on SQLite to test concurrency.

---

## 13. Mental model (lock this in)

- Session = transaction boundary
- begin() = safe guardrails
- commit() = explicit control
- rollback() = mandatory on failure

---

## 14. Golden rule

> If you don’t know where your transaction starts and ends,
> you already have a bug.

---

## 15. Final truth

Correct transaction handling:
- prevents ghost bugs
- prevents data corruption
- makes concurrency boring

And boring is good.
