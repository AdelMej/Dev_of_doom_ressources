# FastAPI + OpenAPI (Swagger) Schemas Cheat Sheet

This document is a **practical cheat sheet** for handling OpenAPI
schemas in FastAPI without losing your sanity.

------------------------------------------------------------------------

## Mental Model (Read This First)

FastAPI has **two worlds**:

  World     Purpose
  --------- ---------------------
  Runtime   Code execution
  OpenAPI   Static API contract

**Exception handlers, business logic, and dynamic behavior are INVISIBLE
to OpenAPI.**\
Swagger only trusts what you declare on the route.

------------------------------------------------------------------------

## Golden Rules

-   OpenAPI does **not** read exception handlers
-   `response_model` is **success-only**
-   Errors must be declared in `responses={}`
-   Dict fields always render as `additionalPropX`
-   Examples are mandatory for readable docs

------------------------------------------------------------------------

## Input Schemas (DTOs)

Used for request validation.

``` python
class RegisterRequest(BaseModel):
    email: EmailStr
    password: str = Field(min_length=8)
```

Used automatically by FastAPI.

------------------------------------------------------------------------

## Output Schemas (Success Responses)

``` python
class UserResponse(BaseModel):
    id: UUID4
    email: str
```

``` python
@router.post(
    "/register",
    response_model=UserResponse,
)
```

------------------------------------------------------------------------

## Error Schemas (Documentation Only)

These often exist **only for OpenAPI**.

``` python
class ValidationErrorResponse(BaseModel):
    error: str
    fields: dict[str, str]

    model_config = {
        "json_schema_extra": {
            "example": {
                "error": "validation_error",
                "fields": {
                    "password": "password must be at least 8 characters long"
                }
            }
        }
    }
```

------------------------------------------------------------------------

## Declaring Error Responses

### Minimal (sometimes buggy)

``` python
responses={
    422: {"model": ValidationErrorResponse}
}
```

### Correct & Explicit (recommended)

``` python
responses={
    422: {
        "description": "Validation error",
        "content": {
            "application/json": {
                "schema": ValidationErrorResponse.model_json_schema(),
                "example": {
                    "error": "validation_error",
                    "fields": {
                        "password": "password must be at least 8 characters long"
                    }
                }
            }
        }
    }
}
```

------------------------------------------------------------------------

## Why Swagger Shows `additionalProp1`

``` python
fields: Dict[str, str]
```

OpenAPI cannot infer keys → generic placeholders.

**Fix:** always provide an example.

------------------------------------------------------------------------

## Global Exception Handlers

Runtime only.

``` python
app.add_exception_handler(
    RequestValidationError,
    validation_exception_handler,
)
```

⚠️ Does NOT affect OpenAPI.

------------------------------------------------------------------------

## DRY OpenAPI Responses

``` python
VALIDATION_ERROR_RESPONSE = {
    422: {
        "description": "Validation error",
        "content": {
            "application/json": {
                "schema": ValidationErrorResponse.model_json_schema()
            }
        }
    }
}
```

``` python
@router.post(
    "/register",
    responses=VALIDATION_ERROR_RESPONSE,
)
```

------------------------------------------------------------------------

## Router Rules (Non-Negotiable)

❌ No try/except\
❌ No error formatting\
❌ No domain logic

✅ Call service\
✅ Declare responses\
✅ Return result

------------------------------------------------------------------------

## When to Add New Schemas

  Situation             Add schema
  --------------------- ------------
  Validation error      Yes
  Domain error          Yes
  Public API response   Yes
  Internal exception    No
  Debug-only error      No

------------------------------------------------------------------------

## TL;DR

-   OpenAPI is dumb but honest
-   Explicit \> implicit
-   Schemas ARE the contract
-   Examples save lives
-   Everyone hates this once

Congrats. You now understand Swagger.
