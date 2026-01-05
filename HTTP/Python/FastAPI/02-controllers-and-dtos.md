# FastAPI Controllers & DTOs Cheat Sheet

## Purpose

This document explains how to design **controllers (routers)** and
**DTOs (schemas)** in FastAPI while keeping a clean architecture.

Goals: - keep HTTP logic isolated - avoid business logic leaks - ensure
predictable validation - prevent spaghetti controllers

------------------------------------------------------------------------

## Terminology Mapping

  Concept        FastAPI Name
  -------------- ------------------------------
  Controller     `APIRouter`
  DTO            Pydantic Model (`BaseModel`)
  Request DTO    Input schema
  Response DTO   Output schema

------------------------------------------------------------------------

## Controller (Router) Responsibilities

### What a controller **does**

-   define endpoints
-   handle HTTP status codes
-   validate input via DTOs
-   call services
-   map domain errors to HTTP errors (or let global handler do it)

### What a controller **never does**

-   business logic
-   database access
-   password hashing
-   token generation
-   data transformation beyond DTO mapping

------------------------------------------------------------------------

## Controller Example

``` python
from fastapi import APIRouter, Depends, status
from app.auth.schemas import RegisterRequest, UserResponse
from app.auth.service import AuthService

router = APIRouter(prefix="/auth", tags=["auth"])

@router.post(
    "/register",
    response_model=UserResponse,
    status_code=status.HTTP_201_CREATED,
)
def register_user(
    payload: RegisterRequest,
    service: AuthService = Depends(),
):
    return service.register(payload)
```

------------------------------------------------------------------------

## DTO (Schema) Responsibilities

### What DTOs **do**

-   validate incoming data
-   define API contracts
-   serialize responses
-   provide type safety

### What DTOs **never do**

-   business logic
-   database access
-   password hashing
-   HTTP status handling

------------------------------------------------------------------------

## Request DTO Example

``` python
from pydantic import BaseModel, EmailStr, Field

class RegisterRequest(BaseModel):
    email: EmailStr
    password: str = Field(min_length=8)
```

------------------------------------------------------------------------

## Response DTO Example

``` python
from pydantic import BaseModel, UUID4

class UserResponse(BaseModel):
    id: UUID4
    email: EmailStr

    class Config:
        from_attributes = True
```

------------------------------------------------------------------------

## DTO Design Rules

-   Separate request and response models
-   Never reuse ORM models as DTOs
-   Never expose internal fields (passwords, hashes)
-   Prefer explicit fields over `dict` or `Any`

------------------------------------------------------------------------

## Validation Rules

-   Use Pydantic for validation only
-   Prefer field-level validation
-   Cross-field validation only when necessary

``` python
from pydantic import field_validator

class PasswordDTO(BaseModel):
    password: str

    @field_validator("password")
    def strong_password(cls, value):
        if value.lower() == value:
            raise ValueError("Password must contain uppercase")
        return value
```

------------------------------------------------------------------------

## Error Handling Strategy

### Validation Errors

-   Automatically handled by FastAPI
-   Mapped to `422 Unprocessable Entity`

### Business Errors

-   Raised in services
-   Handled by global exception handlers

Controllers should **not** catch business exceptions.

------------------------------------------------------------------------

## Naming Conventions

  Type           Suffix
  -------------- -------------
  Request DTO    `*Request`
  Response DTO   `*Response`
  Internal DTO   `*DTO`

------------------------------------------------------------------------

## Controller Size Rule

If a controller: - exceeds \~150 lines - contains `if/else` chains -
starts transforming data

➡️ **You are doing service logic --- stop and refactor**

------------------------------------------------------------------------

## Mental Flow

    HTTP Request
       ↓
    Controller (Router)
       ↓
    DTO Validation
       ↓
    Service Call
       ↓
    Response DTO

------------------------------------------------------------------------

## TL;DR

-   Controllers = HTTP only
-   DTOs = validation + serialization
-   No logic in controllers
-   No logic in DTOs
-   Services own the business

> Thin controllers, strong services, boring DTOs.
