# 23 — Rate Limiting & Abuse Protection (FastAPI)

This cheat sheet explains **how to protect your API** from abuse,
without polluting your business logic.

---

## 1. What Rate Limiting Is (and Is NOT)

Rate limiting is:
- HTTP-level protection
- Traffic control
- Abuse prevention

It is NOT:
- Authorization
- Business rules
- User permissions

Correct layer: **middleware or dependency**

---

## 2. Common Abuse Scenarios

- Credential stuffing (login brute force)
- Token abuse
- Scraping
- Accidental infinite loops
- DDoS-lite from legit clients

Every public API needs limits.

---

## 3. Where Rate Limiting Lives

| Layer | Why |
|----|----|
| Middleware | Global limits |
| Dependency | Per-endpoint / per-user limits |
| Service | ❌ Never |

---

## 4. Simple In-Memory Limiter (DEV ONLY)

```python
import time
from fastapi import HTTPException

REQUESTS = {}

def rate_limit(key: str, limit: int, window: int):
    now = time.time()
    window_start = now - window

    hits = [t for t in REQUESTS.get(key, []) if t > window_start]
    if len(hits) >= limit:
        raise HTTPException(429, "Too many requests")

    hits.append(now)
    REQUESTS[key] = hits
```

❌ Not safe for prod.

---

## 5. Redis-Based Rate Limiting (PROD)

Why Redis:
- shared state
- fast
- atomic ops

Concept:
- key = user/IP
- value = counter + TTL

---

## 6. Dependency-Based Limiting Example

```python
def rate_limit_dep(limit: int, window: int):
    async def checker(request: Request):
        key = request.client.host
        # increment Redis counter here
    return checker
```

Usage:
```python
@router.post("/login")
def login(
    _: None = Depends(rate_limit_dep(5, 60)),
):
    ...
```

---

## 7. Rate Limiting Strategies

- Per IP
- Per user
- Per token
- Per endpoint
- Sliding window
- Fixed window
- Token bucket

Pick simple first.

---

## 8. Login-Specific Protection

Login should have:
- stricter limits
- captcha after failures
- IP + username combo

Never same limits as normal endpoints.

---

## 9. Headers to Return

```http
X-RateLimit-Limit
X-RateLimit-Remaining
Retry-After
```

Helps clients behave.

---

## 10. Middleware vs Dependency Choice

Use middleware when:
- applies to all routes

Use dependency when:
- endpoint-specific
- role-based limits

---

## 11. Common Mistakes

❌ Rate limiting in service  
❌ Per-process memory counters  
❌ No limits on auth endpoints  
❌ Different logic per router  

---

## 12. Spring Boot Mapping

| FastAPI | Spring Boot |
|------|-------------|
| Dependency | HandlerInterceptor |
| Middleware | Filter |
| Redis | Bucket4j |

---

## Final Mental Model

Rate limiting is a **gate**, not a rule.

If it knows about business → wrong layer  
If it protects HTTP → correct layer  

