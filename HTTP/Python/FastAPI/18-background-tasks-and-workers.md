# 18 — Background Tasks & Async Workers Cheat Sheet

This cheat sheet explains **what background tasks actually are** in FastAPI, when async helps, and when you need real workers.

---

## 1. First: async ≠ background

This is the biggest misconception.

- `async` → non-blocking I/O
- background → work not tied to HTTP response

Async code can still block your app if misused.

---

## 2. FastAPI BackgroundTasks (what they really are)

```python
from fastapi import BackgroundTasks

def send_email(email: str):
    ...

@router.post("/register")
async def register(
    data: RegisterRequest,
    background: BackgroundTasks,
):
    background.add_task(send_email, data.email)
    return {"status": "ok"}
```

Reality:
- runs **after response**
- runs in **same process**
- dies if process dies
- NOT durable
- NOT distributed

Good for:
- emails
- logging
- small side effects

---

## 3. Async background tasks (event loop)

```python
async def task():
    await something()

asyncio.create_task(task())
```

⚠️ Dangerous in web apps:
- task lifecycle unmanaged
- cancelled on shutdown
- hard to monitor
- no retries

Avoid unless you fully control lifecycle.

---

## 4. What background tasks are NOT good for

❌ heavy computation  
❌ long-running jobs  
❌ retries / reliability  
❌ cross-service workflows  

If it matters → don’t do it in-process.

---

## 5. When you need real workers

Use workers when you need:
- durability
- retries
- scheduling
- parallelism
- scaling independently from API

Examples:
- payment processing
- video processing
- ML inference
- bulk emails

---

## 6. Common worker options

| Tool      | Model |
|----------|------|
| Celery   | Prefork / threads |
| RQ       | Redis-based |
| Dramatiq| Lightweight |
| Arq      | Async-native |
| Huey     | Simple |

For async Python, **Arq** is often the cleanest.

---

## 7. Async + DB in workers

Workers:
- use their own DB sessions
- never reuse API sessions
- same repositories, same services

Architecture stays clean.

---

## 8. Background tasks + transactions (IMPORTANT)

Do NOT do this:

```python
background.add_task(send_email, user)
await session.commit()
```

Background tasks:
- run outside transaction scope
- may observe partial state

Correct:
- commit first
- enqueue second

---

## 9. Lifespan hooks (clean startup/shutdown)

```python
from fastapi import FastAPI

app = FastAPI()

@app.on_event("startup")
async def startup():
    ...

@app.on_event("shutdown")
async def shutdown():
    ...
```

Used for:
- opening connections
- closing pools
- warming caches

---

## 10. Async CPU work (hard rule)

Async does NOT help CPU-bound work.

If it burns CPU:
- move to worker
- or thread pool
- or separate service

---

## 11. Mental model (lock this in)

- async → concurrency
- background → response decoupling
- workers → reliability

Mixing them blindly causes pain.

---

## 12. Decision table

| Task | Solution |
|----|---------|
| Send email | BackgroundTasks |
| Log analytics | BackgroundTasks |
| Resize image | Worker |
| Payment | Worker |
| Report generation | Worker |
| Cache refresh | Worker |

---

## 13. Anti-patterns (do not do this)

❌ Background DB writes without commit  
❌ Long tasks in FastAPI process  
❌ create_task() everywhere  
❌ Assuming async = safe  

---

## 14. Final truth

> If losing the task is acceptable,
> background tasks are fine.

> If losing the task is NOT acceptable,
> use workers.
