# 20 — FastAPI Middleware Deep Dive

This cheat sheet explains **what middleware really is**, how it differs from dependencies,
and when to use it without shooting yourself in the foot.

---

## 1. What Middleware Is (and Is NOT)

**Middleware runs around the entire request lifecycle.**

Order:
Request →
Middleware (before) →
Dependencies →
Router →
Service →
Response →
Middleware (after)

Middleware is:
- Cross-cutting
- Global or router-scoped
- HTTP-level only

Middleware is NOT:
- Business logic
- Authorization rules
- DTO validation
- Database logic

---

## 2. Middleware vs Dependencies (Critical Distinction)

| Concern | Middleware | Dependency |
|------|-----------|-----------|
| Logging | ✅ | ❌ |
| Metrics | ✅ | ❌ |
| CORS | ✅ | ❌ |
| JWT validation | ❌ | ✅ |
| Authorization | ❌ | ✅ |
| DB session | ❌ | ✅ |
| Request context | ⚠️ | ✅ |

Rule:
If it **decides whether the request is allowed**, it is NOT middleware.

---

## 3. Typical Middleware Use Cases

### ✅ Good Use Cases
- Request logging
- Correlation IDs
- Timing / latency
- Headers injection
- Compression
- CORS
- Rate limiting (edge-level)

### ❌ Bad Use Cases
- User authorization
- JWT validation
- Role checking
- Database queries
- Calling services

---

## 4. Basic Middleware Example

```python
from fastapi import Request
from starlette.middleware.base import BaseHTTPMiddleware
import time

class TimingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        start = time.time()
        response = await call_next(request)
        response.headers["X-Process-Time"] = str(time.time() - start)
        return response
```

Register:
```python
app.add_middleware(TimingMiddleware)
```

---

## 5. Middleware Order Matters

Middleware runs in **reverse order of registration** on response.

```python
app.add_middleware(MiddlewareA)
app.add_middleware(MiddlewareB)
```

Execution:
- Request: A → B → handler
- Response: handler → B → A

---

## 6. Why JWT Should NOT Be Middleware

JWT validation:
- Needs endpoint-specific scopes
- Needs route-level control
- Needs dependency injection

Middleware cannot:
- Access Depends()
- Know which endpoint is called
- Short-circuit cleanly per route

Correct place: **dependencies**

---

## 7. Sharing Data from Middleware

You MAY attach data to request.state:

```python
request.state.request_id = uuid4()
```

Then access in dependencies or routes.

⚠️ Use sparingly.

---

## 8. Middleware + Async Gotchas

- Never block (no sync IO)
- Never open DB sessions
- Never swallow exceptions
- Always return response

Bad:
```python
time.sleep(1)
```

Good:
```python
await asyncio.sleep(1)
```

---

## 9. Middleware vs Filters (Spring Boot Mapping)

| FastAPI | Spring Boot |
|------|------------|
| Middleware | Filter |
| Dependency | Argument Resolver / Security |
| Router | Controller |
| Service | Service |
| Exception handler | @ControllerAdvice |

FastAPI just makes the layers explicit.

---

## 10. When to Add Middleware (Rule of Thumb)

Add middleware only if:
- It applies to MOST requests
- It does NOT depend on business rules
- It does NOT touch the database
- It is stateless

If unsure → use a dependency instead.

---

## 11. Recommended Middleware Stack

Typical prod stack:
- CORS
- Request ID
- Logging
- Timing
- Compression
- Rate limiting (optional)

Everything else belongs elsewhere.

---

## Final Mental Model

Middleware = HTTP plumbing  
Dependencies = request preconditions  
Services = business logic  

Mixing them = architectural debt.

