# FastAPI Dependencies & Auth Guards Cheat Sheet

## Purpose

This document explains how to use **FastAPI dependencies** correctly and
how to design **authentication / authorization guards** without
polluting business logic.

Goals: - clean dependency injection - reusable guards - zero auth logic
in services - predictable request flow

------------------------------------------------------------------------

## What Is a Dependency?

A dependency is a **function FastAPI calls before the endpoint** to: -
provide objects - enforce rules - abort requests early

Dependencies are **infrastructure**, not business logic.

------------------------------------------------------------------------

## Dependency Use Cases

Dependencies are ideal for: - database session injection - service
wiring - authentication - authorization - request-scoped context (user,
tenant, locale)

They are NOT for: - business workflows - data persistence - complex
logic chains

------------------------------------------------------------------------

## Dependency Injection Flow

    Request
      ↓
    Dependencies (auth, db, guards)
      ↓
    Controller (router)
      ↓
    Service

If a request should fail → **fail in a dependency, not in the
controller**.

------------------------------------------------------------------------

## Database Dependency Example

``` python
from sqlalchemy.orm import Session
from app.database import SessionLocal

def get_db() -> Session:
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

Used by repositories, not services.

------------------------------------------------------------------------

## Service Wiring via Dependencies

``` python
from fastapi import Depends
from app.auth.repository import get_user_repository
from app.auth.service import AuthService

def get_auth_service(
    repo = Depends(get_user_repository),
) -> AuthService:
    return AuthService(repo)
```

Router usage:

``` python
def register(
    payload: RegisterRequest,
    service: AuthService = Depends(get_auth_service),
):
    return service.register(payload)
```

------------------------------------------------------------------------

## Authentication Guard (Current User)

### Token Extraction

``` python
from fastapi import Depends, Header
from app.auth.exceptions import InvalidCredentialsError

def get_token(authorization: str = Header(...)):
    if not authorization.startswith("Bearer "):
        raise InvalidCredentialsError()
    return authorization.removeprefix("Bearer ")
```

------------------------------------------------------------------------

### Resolve Current User

``` python
def get_current_user(
    token: str = Depends(get_token),
    repo = Depends(get_user_repository),
):
    user = repo.get_by_token(token)
    if not user:
        raise InvalidCredentialsError()
    return user
```

This dependency: - authenticates - aborts early - returns a user object

------------------------------------------------------------------------

## Authorization Guard (Roles / Permissions)

``` python
def require_admin(user = Depends(get_current_user)):
    if not user.is_admin:
        raise ForbiddenError()
    return user
```

Usage:

``` python
@router.delete("/users/{id}")
def delete_user(
    user = Depends(require_admin),
):
    ...
```

------------------------------------------------------------------------

## Dependency Scopes

FastAPI dependencies are: - request-scoped by default - cached per
request

Avoid global state inside dependencies.

------------------------------------------------------------------------

## Dependencies vs Middleware

  Use                Dependency   Middleware
  ------------------ ------------ ------------
  Auth               ✅           ❌
  DB session         ✅           ❌
  Request logging    ❌           ✅
  Headers mutation   ❌           ✅

Rule: \> **Business decisions → dependencies**\
\> **Cross-cutting infrastructure → middleware**

------------------------------------------------------------------------

## Error Handling in Dependencies

Dependencies should: - raise domain exceptions - never return error
responses

Example:

``` python
raise InvalidCredentialsError()
```

Mapped globally to `401`.

------------------------------------------------------------------------

## Testing Dependencies

-   unit test guards with fake repos
-   integration test full request flow

Avoid mocking FastAPI internals.

------------------------------------------------------------------------

## Anti-Patterns

🚫 Services using `Depends`\
🚫 Dependencies containing business workflows\
🚫 Controllers validating tokens manually\
🚫 Middleware doing authorization

------------------------------------------------------------------------

## Mental Model

    Dependency = Gatekeeper
    Controller = Dispatcher
    Service = Brain

------------------------------------------------------------------------

## TL;DR

-   Dependencies inject infrastructure
-   Guards fail early
-   Auth logic lives in dependencies
-   Services stay pure

> If auth touches your service layer, you already lost.
