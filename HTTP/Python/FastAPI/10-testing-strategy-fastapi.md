# FastAPI Testing Strategy Cheat Sheet

## Purpose

This document defines a **practical, layered testing strategy** for
FastAPI applications.

Goals: - fast feedback - high confidence - minimal mocking - tests that
survive refactors

Golden rule: \> **Test behavior, not implementation.**

------------------------------------------------------------------------

## Test Layers Overview

    Unit Tests        → services, utilities
    Integration Tests → repositories, database
    API Tests         → full HTTP stack

Each layer has a **clear responsibility**.

------------------------------------------------------------------------

## What to Test at Each Layer

### Unit Tests

Test: - services - domain logic - edge cases

Rules: - no FastAPI - no database - no HTTP - mock repositories if
needed

------------------------------------------------------------------------

### Integration Tests

Test: - repositories - ORM mappings - database constraints

Rules: - real database - no HTTP - no FastAPI routing

------------------------------------------------------------------------

### API Tests

Test: - endpoints - request/response contracts - auth guards - error
mapping

Rules: - full stack - real dependencies (or test doubles) - hit the app
via HTTP

------------------------------------------------------------------------

## Tooling

Recommended stack: - `pytest` - `httpx` - `pytest-asyncio` (if async) -
SQLite / Testcontainers

------------------------------------------------------------------------

## Unit Test Example (Service)

``` python
def test_register_user_success():
    repo = FakeUserRepository()
    service = AuthService(repo)

    user = service.register(
        RegisterRequest(
            email="a@b.com",
            password="Strong123",
        )
    )

    assert user.email == "a@b.com"
```

------------------------------------------------------------------------

## Integration Test Example (Repository)

``` python
def test_user_persistence(db_session):
    repo = UserRepository(db_session)

    user = repo.create(
        email="a@b.com",
        password_hash="hash",
    )

    assert repo.exists_by_email("a@b.com")
```

------------------------------------------------------------------------

## API Test Example

``` python
from httpx import AsyncClient

async def test_register_endpoint(app):
    async with AsyncClient(app=app, base_url="http://test") as client:
        response = await client.post(
            "/auth/register",
            json={
                "email": "a@b.com",
                "password": "Strong123",
            },
        )

    assert response.status_code == 201
```

------------------------------------------------------------------------

## Test Database Strategy

### Recommended

-   SQLite in-memory
-   fresh schema per test session

``` env
DATABASE_URL=sqlite:///:memory:
```

------------------------------------------------------------------------

### Alternatives

-   Dockerized Postgres (Testcontainers)
-   Dedicated test database

Never use: - production database - shared dev database

------------------------------------------------------------------------

## Dependency Overrides (FastAPI)

FastAPI allows overriding dependencies in tests:

``` python
app.dependency_overrides[get_db] = get_test_db
```

Use this to: - inject test DB - inject fake services - bypass auth when
needed

------------------------------------------------------------------------

## Authentication in Tests

Options: - override `get_current_user` - generate test tokens - disable
auth selectively

Never hardcode real secrets.

------------------------------------------------------------------------

## What NOT to Test

🚫 FastAPI internals\
🚫 Pydantic validation (already tested)\
🚫 SQLAlchemy internals\
🚫 Framework behavior

Trust your tools.

------------------------------------------------------------------------

## Test Naming Conventions

-   `test_<behavior>.py`
-   one behavior per test
-   readable test names

Bad:

``` python
test_register_1()
```

Good:

``` python
test_register_fails_when_email_exists()
```

------------------------------------------------------------------------

## Test Speed Rules

-   unit tests \< 100ms
-   integration tests \< 1s
-   API tests \< a few seconds

If tests are slow: - reduce scope - reduce setup - remove unnecessary
mocks

------------------------------------------------------------------------

## CI Considerations

-   run tests on every commit
-   fail fast
-   no flaky tests allowed

Tests must be: - deterministic - isolated - repeatable

------------------------------------------------------------------------

## Anti-Patterns

🚫 Mocking everything\
🚫 Testing private methods\
🚫 One giant test\
🚫 Tests depending on order

------------------------------------------------------------------------

## Mental Model

    Unit tests → correctness
    Integration tests → persistence safety
    API tests → contract safety

------------------------------------------------------------------------

## TL;DR

-   Layered testing strategy
-   Services are easiest to test
-   DB tests use real DB
-   API tests validate contracts
-   Fast tests \> perfect tests

> If tests slow you down, they are wrong.
