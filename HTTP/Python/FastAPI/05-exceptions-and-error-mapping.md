# FastAPI Exceptions & Error Mapping Cheat Sheet

## Purpose

This document defines a **clean, centralized error-handling strategy**
for FastAPI applications.

Goals: - keep services framework-agnostic - avoid HTTP logic leakage -
ensure consistent error responses - make errors observable and
debuggable

------------------------------------------------------------------------

## Error Handling Layers

    Service / Domain
       ↓ (raise domain exceptions)
    Global Exception Handlers
       ↓ (map to HTTP)
    Client

Golden rule: \> **Business code raises domain errors --- FastAPI decides
HTTP.**

------------------------------------------------------------------------

## Domain Exceptions (Feature-Level)

### Responsibilities

-   represent business failures
-   carry semantic meaning
-   be framework-agnostic

### Example (`auth/exceptions.py`)

``` python
class UserAlreadyExistsError(Exception):
    def __init__(self, email: str):
        self.email = email
        super().__init__(f"User with email {email} already exists")
```

### Rules

-   NO FastAPI imports
-   NO HTTP status codes
-   NO response formatting

------------------------------------------------------------------------

## Raising Exceptions in Services

``` python
from app.auth.exceptions import UserAlreadyExistsError

def register(self, payload):
    if self.repo.exists_by_email(payload.email):
        raise UserAlreadyExistsError(payload.email)
```

Services **never** catch their own domain errors.

------------------------------------------------------------------------

## Global Exception Handlers

### Location

-   `app/exceptions.py`
-   registered in `main.py`

### Example: Mapping Domain → HTTP

``` python
from fastapi import Request, status
from fastapi.responses import JSONResponse
from app.auth.exceptions import UserAlreadyExistsError

def user_already_exists_handler(
    request: Request,
    exc: UserAlreadyExistsError,
):
    return JSONResponse(
        status_code=status.HTTP_409_CONFLICT,
        content={
            "error": "USER_ALREADY_EXISTS",
            "message": str(exc),
        },
    )
```

------------------------------------------------------------------------

## Registering Handlers

``` python
from fastapi import FastAPI
from app.exceptions import user_already_exists_handler
from app.auth.exceptions import UserAlreadyExistsError

app = FastAPI()

app.add_exception_handler(
    UserAlreadyExistsError,
    user_already_exists_handler,
)
```

------------------------------------------------------------------------

## Validation Errors (DTO)

Handled automatically by FastAPI:

-   Status: `422 Unprocessable Entity`
-   Source: Pydantic validation

Optional override:

``` python
from fastapi.exceptions import RequestValidationError
```

Only override if you need a **custom error format**.

------------------------------------------------------------------------

## Database Exceptions

### Strategy

-   catch DB/ORM exceptions globally
-   never expose raw DB errors to clients

Example:

``` python
from sqlalchemy.exc import IntegrityError

def integrity_error_handler(request, exc):
    return JSONResponse(
        status_code=409,
        content={
            "error": "DATABASE_CONSTRAINT_VIOLATION",
            "message": "Data conflict",
        },
    )
```

------------------------------------------------------------------------

## Authentication / Authorization Errors

Preferred approach: - raise custom domain errors - map to `401` / `403`

``` python
class InvalidCredentialsError(Exception):
    pass
```

Mapping: - `401 Unauthorized` → invalid auth - `403 Forbidden` →
insufficient permissions

------------------------------------------------------------------------

## Error Response Format (Recommended)

``` json
{
  "error": "ERROR_CODE",
  "message": "Human readable message"
}
```

Optional additions: - `details` - `trace_id`

Avoid leaking stack traces.

------------------------------------------------------------------------

## Logging Errors

Rules: - log errors **once**, globally - never log inside services for
control flow - attach request context

``` python
logger.exception("Unhandled error")
```

------------------------------------------------------------------------

## Anti-Patterns

🚫 Raising `HTTPException` in services\
🚫 Catching domain exceptions in controllers\
🚫 Returning error dicts from services\
🚫 Exposing DB error messages

------------------------------------------------------------------------

## Mental Model

    Service: "Something is wrong"
    FastAPI: "Here is the HTTP meaning"

------------------------------------------------------------------------

## TL;DR

-   Services raise domain exceptions
-   FastAPI maps exceptions to HTTP
-   One global error format
-   Zero HTTP logic in business code

> Errors are part of the API contract --- treat them seriously.
