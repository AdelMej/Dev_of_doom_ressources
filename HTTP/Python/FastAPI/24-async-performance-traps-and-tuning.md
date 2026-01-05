# 24 — Async Performance Traps & Tuning (FastAPI)

This cheat sheet explains **why async apps are often slow**, and how to avoid the
most common performance traps in FastAPI.

---

## 1. Async ≠ Fast (Core Truth)

Async:
- improves concurrency
- does NOT make code faster
- hides blocking bugs very well

One blocking call can kill your throughput.

---

## 2. The #1 Performance Killer: Blocking I/O

❌ Blocking examples:
```python
time.sleep(1)
requests.get("https://api.example.com")
psycopg2.connect(...)
```

✅ Async equivalents:
```python
await asyncio.sleep(1)
httpx.AsyncClient().get(...)
asyncpg / async SQLAlchemy
```

Rule:
If it blocks → it must NOT be in async code.

---

## 3. Sync Code in Async Routes

FastAPI allows sync routes:
```python
@router.get("/sync")
def sync_endpoint():
    ...
```

These run in a threadpool.

⚠️ Threadpool is limited.
Too many sync calls → starvation.

---

## 4. Database Performance Rules

- One session per request
- Never share sessions
- Use connection pooling
- Avoid implicit transactions

Bad:
```python
await session.execute(...)
await session.execute(...)
```

Good:
```python
async with session.begin():
    ...
```

---

## 5. N+1 Query Trap

Classic ORM problem.

Fix with:
- joins
- selectinload
- explicit queries

Async does NOT save you here.

---

## 6. Overusing Dependencies

Dependencies run:
- even if result unused
- per request
- possibly multiple times

Heavy work in dependencies = hidden cost.

Rule:
Dependencies = validation, extraction, gating.

---

## 7. Background Tasks Misuse

FastAPI background tasks:
- run in same process
- share CPU
- can block event loop

Do NOT use for:
- long jobs
- heavy CPU
- external APIs at scale

Use workers instead.

---

## 8. JSON Serialization Costs

Large responses:
- cost CPU
- block event loop

Tips:
- avoid huge payloads
- paginate aggressively
- avoid nested objects

---

## 9. Logging Can Kill Performance

❌ Sync logging + disk IO
❌ Logging inside loops

Use:
- async-safe logging
- log levels
- structured logs

---

## 10. Uvicorn / Gunicorn Tuning

Key knobs:
- workers (CPU bound)
- max-concurrency
- keep-alive

Rule:
Async scales with **connections**, not CPU.

---

## 11. CPU-Bound Work

Never do CPU-heavy work in async app.

Examples:
- crypto
- image processing
- data crunching

Offload to:
- workers
- job queues
- separate services

---

## 12. Measuring Performance (Mandatory)

Use:
- timing middleware
- request metrics
- load testing (locust, k6)

Never optimize blind.

---

## 13. Spring Boot Comparison

| Problem | FastAPI | Spring |
|------|--------|--------|
| Blocking call | Silent killer | Thread starvation |
| ORM misuse | N+1 | N+1 |
| Async abuse | Easy | Harder |

Different runtime, same mistakes.

---

## Final Mental Model

Async apps fail when:
- blocking sneaks in
- responsibilities blur
- concurrency is misunderstood

Async = discipline, not magic.

