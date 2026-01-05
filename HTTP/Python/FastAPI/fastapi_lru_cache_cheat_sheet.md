# FastAPI `@lru_cache` Cheat Sheet

## What is `@lru_cache`?

`@lru_cache` is a Python decorator from `functools` that **caches the return value of a function**.
When used with FastAPI dependencies, it effectively creates a **singleton per process**.

```python
from functools import lru_cache

@lru_cache
def get_settings():
    return Settings()
```

---

## When SHOULD you use `@lru_cache`?

Use it **only** for dependencies that are:

- ✅ Stateless
- ✅ Immutable (or effectively immutable)
- ✅ Safe to share across requests
- ✅ Global in scope

### Good examples

```python
@lru_cache
def get_settings() -> Settings:
    return Settings()

@lru_cache
def get_password_hasher() -> PasswordHasher:
    return BcryptPasswordHasher()

@lru_cache
def get_jwt_service() -> JwtService:
    return JwtService(secret=..., algorithm="HS256")

@lru_cache
def get_logger() -> Logger:
    return configure_logger()
```

---

## When you MUST NOT use `@lru_cache`

Do **not** cache dependencies that are:

- ❌ Request-scoped
- ❌ User-scoped
- ❌ Stateful or mutable
- ❌ Transactional

### Bad examples

```python
def get_db_session():          # ❌
    ...

def get_current_user():        # ❌
    ...

def get_request():             # ❌
    ...

def get_auth_service():        # ❌ usually
    ...
```

Caching these can cause:
- state leaks across users
- broken transactions
- race conditions
- impossible-to-debug bugs

---

## Services: cache or not?

**Default rule:** ❌ Do NOT cache services.

Services are usually:
- cheap to construct
- composed of request-scoped dependencies
- orchestration logic

### Rare exception (OK to cache)

```python
@lru_cache
def get_health_service() -> HealthService:
    return HealthService()
```

Only valid if:
- service is fully stateless
- all dependencies are cached
- no request data is involved

---

## Performance reality check

- `@lru_cache` does **not** increase concurrency
- It avoids unnecessary object creation
- Password hashing performance is CPU-bound
- Real scaling comes from:
  - more workers
  - thread pools
  - async-friendly design

---

## Mental model (memorize this)

> Cache **behavior and configuration**, not **flow and state**

Or simply:

> “If two users hit this endpoint at the same time,
> is it safe for them to share this object?”

- Yes → cache it
- No → don’t cache it

---

## TL;DR Table

| Dependency type        | Cache?       |
| ---------------------- | ------------ |
| Settings / config      | ✅ Yes        |
| Password hasher        | ✅ Yes        |
| JWT helper             | ✅ Yes        |
| Logger                 | ✅ Yes        |
| Redis / HTTP client    | ✅ Yes        |
| Repository             | ❌ No         |
| DB session             | ❌ No         |
| Service                | ❌ Usually no |
| Current user / request | ❌ Never      |

---

## Final takeaway

`@lru_cache` is a **precision tool**, not a default.

Use it to declare **lifetime**, not to chase performance.
