# FastAPI Clean / Hexagonal (Accidental) Architecture Cheat Sheet

> You did not choose hexagonal architecture.
> You just hated duplication and coupling.
> This document explains the final structure and *why* it exists.

---

## 🎯 Goals

- Avoid duplication (`UserModel` everywhere)
- Decouple business logic from frameworks
- Allow multiple persistence implementations
- Enable fast tests (no DB required)
- Keep teammates sane

---

## 📁 Final Project Structure

```
app/
├── main.py
├── shared/
│   ├── exceptions.py          # Global exception handlers
│   └── types.py               # Shared base types / errors
│
├── domain/
│   └── user.py                # Business rules & invariants
│
├── features/
│   └── auth/
│       ├── router.py          # HTTP layer (FastAPI)
│       ├── schemas.py         # DTOs (request / response)
│       ├── service.py         # Use cases
│       ├── repository.py      # Repository interface (PORT)
│       └── dependencies.py    # Feature-scoped DI
│
├── infrastructure/
│   └── persistence/
│       ├── in_memory/
│       │   └── user_repo.py   # In-memory adapter (tests / demo)
│       └── sqlalchemy/
│           ├── models/
│           │   └── user.py    # SQLAlchemy models
│           └── user_repo.py   # SQLAlchemy adapter
│
└── tests/
    └── auth/
```

---

## 🧠 Layer Responsibilities

### `domain/`
**What lives here**
- Core business rules
- Invariants that must *always* hold true

**What does NOT live here**
- SQLAlchemy
- FastAPI
- HTTP
- Framework imports

---

### `features/*/repository.py` (PORT)

Defines **what the app needs**, not **how it’s done**.

```python
class UserRepository(Protocol):
    async def save(self, user: User) -> User: ...
    async def get_by_email(self, email: str) -> User | None: ...
```

📌 Implementation lives in `infrastructure/`

---

### `infrastructure/`
**Adapters to the outside world**
- Databases
- ORMs
- External services

Multiple implementations can exist:
- In-memory (tests)
- SQLAlchemy (production)
- Anything else later

---

### `service.py`
- Orchestrates use cases
- Applies domain rules
- Talks to repositories via interfaces

📌 This is where business workflows live.

---

### `router.py`
- HTTP only
- Calls services
- Converts DTOs → domain → DTOs

📌 No business logic.

---

### `dependencies.py`
- Feature-scoped dependency injection
- Prevents a massive global DI god-file
- Easy swapping of implementations

---

## 🧪 Testing Benefits

- No DB needed
- In-memory repo plugs in instantly
- Tests are fast and deterministic

```python
repo = InMemoryUserRepository()
service = AuthService(repo)
```

---

## 🧠 What This Actually Is

This structure matches the **core idea of hexagonal architecture**:

> Business logic does not depend on infrastructure.

No buzzwords required.
No cargo cult.
Just clean code.

---

## 🏁 Final Notes

- Folder structure is a tool, not a religion
- Names don’t matter, boundaries do
- Architecture should reduce pain, not create it

You didn’t overengineer.
You evolved.
