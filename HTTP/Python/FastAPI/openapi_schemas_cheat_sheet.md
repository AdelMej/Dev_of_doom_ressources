# OpenAPI Schemas Cheat Sheet (FastAPI / Python)

This cheat sheet focuses on **OpenAPI schemas (Pydantic models)** and
**how they are used in controllers**. It is framework-lean and
architecture-friendly.

------------------------------------------------------------------------

## What is an OpenAPI Schema?

An OpenAPI schema describes: - Request bodies - Responses - Validation
rules - API documentation (Swagger / ReDoc)

In FastAPI, schemas are defined using **Pydantic models**.

> Schemas describe *data contracts*, not business logic.

------------------------------------------------------------------------

## Basic Schema

``` python
from pydantic import BaseModel

class User(BaseModel):
    username: str
    email: str
```

Used for: - Request validation - Response serialization - Swagger
documentation

------------------------------------------------------------------------

## Field Constraints (Most Common)

``` python
from pydantic import BaseModel, Field

class LoginRequest(BaseModel):
    username: str = Field(
        ...,
        min_length=3,
        max_length=64,
        examples=["adel"],
    )
    password: str = Field(
        ...,
        min_length=8,
        max_length=128,
        format="password",
    )
```

### Common Field Options

-   `min_length`, `max_length`
-   `gt`, `ge`, `lt`, `le`
-   `regex`
-   `examples`
-   `description`
-   `format` (UI-only)

------------------------------------------------------------------------

## Request Body in Controller

``` python
from fastapi import APIRouter

router = APIRouter()

@router.post("/login")
def login(payload: LoginRequest):
    return {"message": "ok"}
```

What happens: - JSON → validated against schema - Invalid input → 422
automatically - Swagger auto-generated

------------------------------------------------------------------------

## Response Models (Strongly Recommended)

``` python
class TokenResponse(BaseModel):
    access_token: str
    token_type: str = "bearer"
```

``` python
@router.post("/login", response_model=TokenResponse)
def login(payload: LoginRequest):
    return {
        "access_token": "jwt",
        "token_type": "bearer",
    }
```

Benefits: - Output validation - Prevents accidental data leaks - Clean
OpenAPI docs

------------------------------------------------------------------------

## Hiding Fields from Responses

``` python
class UserInternal(BaseModel):
    id: int
    username: str
    password_hash: str

class UserPublic(BaseModel):
    id: int
    username: str
```

Never reuse internal models for responses.

------------------------------------------------------------------------

## Optional Fields

``` python
from typing import Optional

class UpdateUser(BaseModel):
    email: Optional[str] = None
    display_name: Optional[str] = None
```

Optional fields: - Not required in requests - Useful for PATCH endpoints

------------------------------------------------------------------------

## Query Parameters

``` python
from fastapi import Query

@router.get("/users")
def list_users(
    limit: int = Query(10, ge=1, le=100),
    offset: int = Query(0, ge=0),
):
    ...
```

These also appear in OpenAPI.

------------------------------------------------------------------------

## Path Parameters

``` python
@router.get("/users/{user_id}")
def get_user(user_id: int):
    ...
```

Constraints can be added via `Path()`.

------------------------------------------------------------------------

## Error Response Schemas

``` python
class ErrorResponse(BaseModel):
    detail: str
```

``` python
@router.get(
    "/secure",
    responses={
        401: {"model": ErrorResponse},
        403: {"model": ErrorResponse},
    },
)
def secure_endpoint():
    ...
```

------------------------------------------------------------------------

## Status Codes

``` python
from fastapi import status

@router.post(
    "/users",
    status_code=status.HTTP_201_CREATED,
)
def create_user(payload: User):
    ...
```

Always document non-200 responses.

------------------------------------------------------------------------

## Schema Reuse (Good Architecture)

``` python
class Password(str):
    @classmethod
    def __get_pydantic_json_schema__(cls, schema, handler):
        schema.update(
            minLength=8,
            maxLength=128,
            format="password",
        )
        return schema
```

``` python
class LoginRequest(BaseModel):
    username: str
    password: Password
```

Single source of truth for validation + docs.

------------------------------------------------------------------------

## What NOT to Put in Schemas

❌ Hashing logic\
❌ Database models\
❌ Business rules\
❌ Authorization logic\
❌ Framework-specific side effects

Schemas are **contracts**, not behavior.

------------------------------------------------------------------------

## Common Mistakes

-   Reusing DB models as API schemas
-   Returning ORM objects directly
-   Forgetting `response_model`
-   Exposing internal fields
-   Over-validating in controllers

------------------------------------------------------------------------

## Mental Model (Important)

-   **Schema** → shape & validation
-   **Controller** → orchestration
-   **Service** → business logic
-   **Repository** → persistence

OpenAPI only cares about the first two.

------------------------------------------------------------------------

## TL;DR

-   Use Pydantic models for OpenAPI schemas
-   Always define request & response models
-   Keep schemas dumb and explicit
-   Swagger is documentation, not security
-   Good schema design pays off immediately

------------------------------------------------------------------------

## References

-   OpenAPI Specification
-   FastAPI Documentation
-   Pydantic Documentation
