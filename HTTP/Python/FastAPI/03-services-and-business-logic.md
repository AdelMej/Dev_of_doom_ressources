# FastAPI Services & Business Logic Cheat Sheet

## Purpose

This document defines how to design **services** in a FastAPI
application.

Services are the **core of the application**: - they contain business
rules - they orchestrate workflows - they are framework-agnostic

If services are clean, the whole system stays clean.

------------------------------------------------------------------------

## What Is a Service?

A service is a **pure Python class** responsible for: - enforcing domain
rules - coordinating repositories - performing transformations - raising
domain exceptions

A service knows **nothing** about: - HTTP - FastAPI - status codes -
request / response objects

------------------------------------------------------------------------

## Service Responsibilities

### Services **DO**

-   implement business workflows
-   validate domain constraints
-   hash passwords
-   generate tokens
-   coordinate multiple repositories
-   raise meaningful domain exceptions

### Services **NEVER**

-   import FastAPI
-   return `Response`
-   use `Depends`
-   access request objects
-   decide HTTP status codes

------------------------------------------------------------------------

## Service Example

``` python
from app.auth.repository import UserRepository
from app.auth.schemas import RegisterRequest
from app.auth.exceptions import UserAlreadyExistsError
from app.auth.security import hash_password

class AuthService:
    def __init__(self, repo: UserRepository):
        self.repo = repo

    def register(self, payload: RegisterRequest):
        if self.repo.exists_by_email(payload.email):
            raise UserAlreadyExistsError(payload.email)

        user = self.repo.create(
            email=payload.email,
            password_hash=hash_password(payload.password),
        )

        return user
```

------------------------------------------------------------------------

## Dependency Injection

Services should receive dependencies **explicitly**.

### Preferred

``` python
class AuthService:
    def __init__(self, repo: UserRepository):
        self.repo = repo
```

### FastAPI wiring (outside the service)

``` python
from fastapi import Depends
from app.auth.repository import get_user_repository
from app.auth.service import AuthService

def get_auth_service(
    repo = Depends(get_user_repository),
):
    return AuthService(repo)
```

------------------------------------------------------------------------

## One Service, One Responsibility

Bad: - `AuthService` handling users, tokens, emails, audit logs

Good: - `AuthService` → auth logic - `TokenService` → tokens -
`EmailService` → emails

Services can **call each other**, but avoid cycles.

------------------------------------------------------------------------

## Error Handling Strategy

### Services raise **domain exceptions**

``` python
class UserAlreadyExistsError(Exception):
    pass
```

### Services never raise HTTP exceptions

❌ `HTTPException` ❌ status codes ❌ response bodies

Mapping to HTTP happens **globally**.

------------------------------------------------------------------------

## Transaction Boundaries

Services should: - assume repositories are atomic - not manage DB
sessions directly

If transactions are needed: - use a higher-level service - or a
unit-of-work abstraction

------------------------------------------------------------------------

## Testing Services

Services are the **easiest layer to test**.

Why? - no FastAPI - no HTTP - no ASGI

Example:

``` python
def test_register_user():
    repo = FakeUserRepository()
    service = AuthService(repo)

    user = service.register(RegisterRequest(
        email="a@b.com",
        password="Strong123",
    ))

    assert user.email == "a@b.com"
```

------------------------------------------------------------------------

## Naming Conventions

-   `*Service`
-   verbs for methods (`create_user`, `register`, `authenticate`)
-   avoid `handle_*` or `process_*`

------------------------------------------------------------------------

## Anti-Patterns

🚫 Service importing FastAPI\
🚫 Service returning dicts shaped for HTTP\
🚫 Service catching `HTTPException`\
🚫 Fat services doing everything

------------------------------------------------------------------------

## Mental Model

    Controller (HTTP)
       ↓
    Service (business rules)
       ↓
    Repository (persistence)

Services are **where correctness lives**.

------------------------------------------------------------------------

## TL;DR

-   Services = business logic
-   Pure Python
-   No FastAPI imports
-   Explicit dependencies
-   Raise domain exceptions only

> If your service layer is clean, scaling is boring --- and boring is
> good.
