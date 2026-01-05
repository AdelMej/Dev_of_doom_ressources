# 25 - FastAPI Middleware Cheat Sheet

## What is Middleware?
Middleware is code that runs **for every single HTTP request**, before and after routing.
It operates at the **HTTP layer**, not the business or domain layer.

---

## Request Lifecycle

Request
↓
Middleware (ALL requests)
↓
Routing
↓
Dependencies (per-route)
↓
Controller
↓
Service
↓
Repository
↓
Response

---

## When to Use Middleware

Use middleware for **cross-cutting concerns**:

- Request / response logging
- Metrics & timing
- Correlation / request IDs
- CORS
- GZip / compression
- Security headers
- Global rate limiting
- Maintenance mode
- Panic / exception recovery
- Attaching raw context to request.state

Example:
```python
@app.middleware("http")
async def timing_middleware(request: Request, call_next):
    start = time.time()
    response = await call_next(request)
    response.headers["X-Process-Time"] = str(time.time() - start)
    return response
```

---

## What Middleware MUST NOT Do

❌ Authorization decisions  
❌ Business logic  
❌ Route-specific rules  
❌ ORM / database access  
❌ Service or repository calls  
❌ Scope / role enforcement  

If middleware needs to know **which endpoint** is called, it's already wrong.

---

## Middleware vs Dependencies

| Aspect | Middleware | Dependency |
|----|----|----|
| Runs on | Every request | Selected routes |
| Knows endpoint | ❌ No | ✅ Yes |
| Purpose | Cross-cutting | Guard / adaptation |
| Can access DB | ❌ No | ⚠️ Rarely |
| Business logic | ❌ No | ❌ No |

---

## Middleware + Dependencies (Correct Split)

### Middleware
- Extract raw JWT
- Attach to request.state

```python
request.state.token = extract_token(request)
```

### Dependency
- Validate token
- Load user
- Check permissions

```python
def get_current_user(token=Depends(get_token)):
    ...
```

---

## Rule of Thumb

- Applies to **all requests** → Middleware
- Applies to **some routes** → Dependency
- Applies to **domain rules** → Service

---

## Common Anti-Patterns

🚨 Checking request.path for permissions  
🚨 Importing services in middleware  
🚨 Using middleware as a guard system  
🚨 Mixing HTTP and domain logic  

---

## Final Takeaway

Middleware should be **boring, predictable, and unconditional**.
If it becomes smart, it belongs somewhere else.

