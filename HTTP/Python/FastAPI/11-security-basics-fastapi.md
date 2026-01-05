# FastAPI Security Basics Cheat Sheet

## Purpose

This document defines **baseline security practices** for FastAPI
applications.

Goals: - protect credentials and secrets - avoid common security
footguns - establish safe defaults - stay framework-agnostic where
possible

Golden rule: \> **Security is about reducing blast radius, not achieving
perfection.**

------------------------------------------------------------------------

## Security Layers

    Client
      ↓
    Reverse Proxy (TLS, rate limits)
      ↓
    FastAPI (auth, validation)
      ↓
    Service (domain rules)
      ↓
    Database

Security must exist at **multiple layers**.

------------------------------------------------------------------------

## Secrets Management

### Never Hardcode Secrets

🚫 API keys\
🚫 JWT secrets\
🚫 DB passwords

Use environment variables:

``` env
DATABASE_URL=...
JWT_SECRET=...
```

Load via settings:

``` python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    JWT_SECRET: str

settings = Settings()
```

------------------------------------------------------------------------

## Password Handling

Rules: - never store plaintext passwords - never log passwords - always
hash with a strong algorithm

Recommended: - `bcrypt` - `argon2`

Example:

``` python
from passlib.context import CryptContext

pwd_context = CryptContext(schemes=["bcrypt"])

def hash_password(password: str) -> str:
    return pwd_context.hash(password)

def verify_password(password: str, hash: str) -> bool:
    return pwd_context.verify(password, hash)
```

------------------------------------------------------------------------

## Authentication Strategy

Preferred: - Bearer tokens (JWT or opaque tokens)

Avoid: - session cookies (unless needed) - rolling your own crypto

JWT rules: - short expiration - rotate secrets - validate signature &
expiration

------------------------------------------------------------------------

## Authorization Strategy

Authentication ≠ Authorization.

Use: - role-based access control (RBAC) - permission checks

Implement via **dependencies**, not services.

Example:

``` python
def require_admin(user = Depends(get_current_user)):
    if not user.is_admin:
        raise ForbiddenError()
```

------------------------------------------------------------------------

## Input Validation

FastAPI + Pydantic already provide: - type validation - length checks -
format validation

Still: - validate business constraints in services - never trust client
input

------------------------------------------------------------------------

## SQL Injection Protection

If using an ORM: - use parameterized queries - never build SQL strings
manually

🚫 f-strings in SQL\
🚫 string concatenation

------------------------------------------------------------------------

## CORS Configuration

Restrict origins explicitly:

``` python
from fastapi.middleware.cors import CORSMiddleware

app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://example.com"],
    allow_methods=["GET", "POST"],
    allow_headers=["Authorization"],
)
```

Never use:

``` python
allow_origins=["*"]
```

in production.

------------------------------------------------------------------------

## Rate Limiting

FastAPI does not include rate limiting by default.

Use: - reverse proxy (Nginx, Traefik) - API gateway - external
middleware

Rate limiting is **infrastructure**, not business logic.

------------------------------------------------------------------------

## HTTPS / TLS

Rules: - TLS is mandatory in production - terminate TLS at proxy or load
balancer - never serve HTTPS directly from FastAPI

FastAPI should only see HTTP internally.

------------------------------------------------------------------------

## Security Headers

Handled by proxy: - `X-Content-Type-Options` - `X-Frame-Options` -
`Content-Security-Policy`

FastAPI should not manage headers manually.

------------------------------------------------------------------------

## Dependency Attacks

Rules: - pin dependency versions - audit dependencies - update regularly

Tools: - `pip-audit` - Dependabot - Renovate

------------------------------------------------------------------------

## Logging & Security

Never log: - passwords - tokens - authorization headers - personal data

Redact sensitive fields explicitly.

------------------------------------------------------------------------

## Error Handling & Security

Rules: - generic messages to clients - detailed logs internally - never
expose stack traces

Good:

``` json
{ "error": "UNAUTHORIZED" }
```

Bad:

``` json
{ "error": "JWT signature invalid at line 42" }
```

------------------------------------------------------------------------

## Common FastAPI Security Mistakes

🚫 `allow_origins=["*"]`\
🚫 Long-lived JWTs\
🚫 Secrets in git\
🚫 Auth logic in services\
🚫 Logging auth headers

------------------------------------------------------------------------

## Mental Model

    Auth = identity
    AuthZ = permission
    Validation = input safety
    Secrets = blast radius

------------------------------------------------------------------------

## TL;DR

-   Secrets via environment
-   Hash passwords properly
-   Auth via dependencies
-   TLS via proxy
-   Validate inputs
-   Log carefully

> Secure defaults beat clever tricks every time.
