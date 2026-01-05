# 22 — Config & Environments (12-Factor FastAPI)

This cheat sheet shows how to structure configuration cleanly for **dev / test / prod**
without leaking infra into your code.

---

## 1. Core Rule (12-Factor)

**Config is NOT code.**

No:
- hardcoded secrets
- environment branching logic everywhere
- importing env vars inside services

Yes:
- one config module
- env-based overrides
- explicit settings objects

---

## 2. Recommended Structure

```
app/
  config/
    __init__.py
    base.py
    dev.py
    prod.py
    test.py
```

---

## 3. Base Settings (Shared)

```python
from pydantic import BaseSettings

class Settings(BaseSettings):
    app_name: str = "fastapi-app"
    debug: bool = False

    database_url: str
    secret_key: str
    access_token_minutes: int = 15

    class Config:
        env_file = ".env"
```

---

## 4. Environment-Specific Overrides

```python
class DevSettings(Settings):
    debug: bool = True
```

```python
class ProdSettings(Settings):
    debug: bool = False
```

---

## 5. Settings Factory

```python
def get_settings():
    env = os.getenv("ENV", "dev")

    if env == "prod":
        return ProdSettings()
    if env == "test":
        return TestSettings()
    return DevSettings()
```

---

## 6. Injecting Settings (Dependency)

```python
def get_settings_dep():
    return get_settings()
```

Usage:
```python
def endpoint(
    settings = Depends(get_settings_dep),
):
    ...
```

Never import settings directly in routers.

---

## 7. Database Config Example

```python
DATABASE_URL=postgresql+asyncpg://user:pass@db/app
```

SQLite (dev):
```python
DATABASE_URL=sqlite+aiosqlite:///./dev.db
```

---

## 8. Secrets Handling (Rules)

❌ Git  
❌ Dockerfile  
❌ Source code  

✅ Env vars  
✅ Secret managers  
✅ CI injection  

---

## 9. Logging Configuration

```python
LOG_LEVEL=INFO
```

Injected into logging config on startup.

---

## 10. Feature Flags (Optional)

```python
ENABLE_SIGNUP=true
ENABLE_BETA=false
```

Use flags sparingly.

---

## 11. Test Environment

Tests should:
- use isolated DB
- override settings dependency
- never touch prod config

---

## 12. Spring Boot Mapping

| FastAPI | Spring Boot |
|------|-------------|
| BaseSettings | @ConfigurationProperties |
| .env | application.yml |
| Depends(settings) | @Autowired |

---

## Final Mental Model

Config:
- loaded once
- injected everywhere
- environment-driven

If config leaks into services → architecture smell.

