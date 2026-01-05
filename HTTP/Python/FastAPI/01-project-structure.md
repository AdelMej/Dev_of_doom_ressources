# FastAPI Project Structure Cheat Sheet

## Purpose

This document defines a **clean, scalable, feature-first** project
structure for FastAPI applications.

Goals: - prevent spaghetti code - enforce clear boundaries - keep the
application server-agnostic - scale from small projects to production
(containers / pods)

------------------------------------------------------------------------

## Canonical Project Layout

    project-root/
      app/
        __init__.py
        main.py
        exceptions.py
        logging.py              # optional
        auth/
          __init__.py
          router.py
          schemas.py
          service.py
          repository.py
          dependencies.py
          models.py
          exceptions.py
      docs/
      .venv/
      requirements.txt
      .gitignore

------------------------------------------------------------------------

## Responsibilities by File

### `app/main.py`

**Application entry point (ASGI)**

Responsibilities: - create the `FastAPI()` instance - register routers -
register global exception handlers - configure logging

Rules: - NO routes - NO business logic - NO database access - NO
`uvicorn` or server code

------------------------------------------------------------------------

### `router.py` (per feature)

**HTTP boundary**

Responsibilities: - define endpoints - HTTP status codes - request /
response models - dependency injection (`Depends`)

Rules: - NO business logic - NO database access - NO password hashing

------------------------------------------------------------------------

### `schemas.py`

**DTOs / validation**

Responsibilities: - Pydantic models - input validation - response
serialization

Rules: - NO business logic - NO database access - NO FastAPI imports

------------------------------------------------------------------------

### `service.py`

**Business logic**

Responsibilities: - workflows - domain rules - authentication logic -
password hashing - token generation

Rules: - NO FastAPI imports - NO HTTP logic - NO database session
handling

------------------------------------------------------------------------

### `repository.py`

**Persistence layer**

Responsibilities: - database queries - CRUD operations - ORM interaction

Rules: - NO business rules - NO HTTP logic - NO FastAPI imports

------------------------------------------------------------------------

### `models.py`

**ORM models**

Responsibilities: - ORM entity definitions - schema mapping

Rules: - NO queries - NO business logic - NO FastAPI imports

------------------------------------------------------------------------

### `dependencies.py`

**FastAPI dependency injection**

Responsibilities: - `Depends()` providers - auth guards - DB session
injection - current user resolution

Rules: - NO business logic

------------------------------------------------------------------------

### `exceptions.py` (feature-level)

**Domain exceptions**

Responsibilities: - custom exception classes - meaningful domain errors

Rules: - NO HTTP responses - NO FastAPI imports

------------------------------------------------------------------------

### `app/exceptions.py` (global)

**Exception to HTTP mapping**

Responsibilities: - translate domain exceptions to HTTP responses -
handle request validation errors - handle authentication / authorization
errors - handle database exceptions

------------------------------------------------------------------------

## Import Rules

-   Always use **absolute imports**
-   Always import from the package root (`app.`)
-   Every importable directory **must** contain `__init__.py`

Correct:

``` python
from app.auth.router import router
```

Incorrect:

``` python
from auth.router import router
```

------------------------------------------------------------------------

## Routing Rules

-   Use `APIRouter` exclusively
-   One router per feature
-   `app` only includes routers

Never use:

``` python
@app.get(...)
```

in real projects.

------------------------------------------------------------------------

## Server Rules

-   Server is started **outside** application code
-   Use `fastapi dev` for development
-   Use `uvicorn` / `gunicorn` for production

Never call:

``` python
uvicorn.run(...)
```

inside Python code.

------------------------------------------------------------------------

## Mental Model

    HTTP → Router → Service → Repository → Database
               ↓
             Schemas

> The FastAPI application is a **dumb ASGI unit**. Infrastructure
> decides how it runs.

------------------------------------------------------------------------

## TL;DR

-   Feature-first structure
-   Thin routers
-   Pure services
-   Explicit repositories
-   Absolute imports only
-   Server logic lives outside the app
