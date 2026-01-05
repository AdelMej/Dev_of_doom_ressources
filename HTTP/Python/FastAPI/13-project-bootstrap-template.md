# FastAPI Project Bootstrap Template

## Purpose

This document is a **copy-paste starter template** for new FastAPI
projects.

Goals: - sane defaults - clean architecture - feature-first layout -
zero infra coupling

Use this as the **baseline for every new service**.

------------------------------------------------------------------------

## Recommended Folder Structure

    project/
    ├── app/
    │   ├── main.py
    │   ├── core/
    │   │   ├── config.py
    │   │   ├── logging.py
    │   │   └── exceptions.py
    │   ├── auth/
    │   │   ├── router.py
    │   │   ├── service.py
    │   │   ├── repository.py
    │   │   ├── models.py
    │   │   ├── schemas.py
    │   │   ├── dependencies.py
    │   │   └── exceptions.py
    │   └── __init__.py
    ├── tests/
    │   └── auth/
    ├── requirements.txt
    ├── .env.example
    ├── .gitignore
    └── README.md

------------------------------------------------------------------------

## main.py (Minimal)

``` python
from fastapi import FastAPI
from app.auth.router import router as auth_router

app = FastAPI()

app.include_router(auth_router, prefix="/auth", tags=["auth"])
```

No uvicorn logic inside the app.

------------------------------------------------------------------------

## Running the App

Development:

``` bash
fastapi dev app/main.py
```

Production:

``` bash
gunicorn -k uvicorn.workers.UvicornWorker app.main:app
```

------------------------------------------------------------------------

## Example Router

``` python
from fastapi import APIRouter
from app.auth.schemas import RegisterRequest

router = APIRouter()

@router.post("/register")
def register(data: RegisterRequest):
    return {"status": "ok"}
```

Routers only: - validate input - call services - return responses

------------------------------------------------------------------------

## Example Service

``` python
class AuthService:
    def register(self, data):
        pass
```

No FastAPI imports here.

------------------------------------------------------------------------

## Example Repository

``` python
class UserRepository:
    def create(self, user):
        pass
```

Repositories only talk to the database.

------------------------------------------------------------------------

## Configuration Layer

``` python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    DATABASE_URL: str
    ENV: str = "dev"

settings = Settings()
```

------------------------------------------------------------------------

## Exception Strategy

-   domain exceptions in features
-   global mapping in core.exceptions
-   HTTP errors never leak domain details

------------------------------------------------------------------------

## Testing Layout

    tests/
    ├── unit/
    │   └── services/
    ├── integration/
    │   └── repositories/
    └── api/
        └── auth/

------------------------------------------------------------------------

## What This Template Enforces

✅ feature-first architecture\
✅ testability\
✅ infra separation\
✅ scalability\
✅ sanity

------------------------------------------------------------------------

## Anti-Patterns Prevented

🚫 fat controllers\
🚫 business logic in routers\
🚫 ORM leaks\
🚫 uvicorn in code\
🚫 god modules

------------------------------------------------------------------------

## TL;DR

-   copy structure
-   implement features
-   wire routers
-   deploy anywhere

> Start clean or refactor forever.
