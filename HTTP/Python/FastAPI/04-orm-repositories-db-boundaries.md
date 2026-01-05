# FastAPI ORM Models, Repositories & Database Boundaries Cheat Sheet

## Purpose

This document defines **clear database boundaries** in a FastAPI
application.

Goals: - isolate persistence logic - avoid ORM leakage - keep services
database-agnostic - allow easy DB switching (SQLite → Postgres → MySQL)

------------------------------------------------------------------------

## Layer Overview

    Service
       ↓
    Repository
       ↓
    ORM Models
       ↓
    Database

Rules: - Services never talk to the ORM directly - Repositories never
contain business logic - ORM models never contain queries

------------------------------------------------------------------------

## ORM Models (`models.py`)

### Responsibilities

-   define table structure
-   define relationships
-   map database fields

### ORM Model Example (SQLAlchemy)

``` python
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column
from sqlalchemy import String
import uuid

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "users"

    id: Mapped[uuid.UUID] = mapped_column(primary_key=True)
    email: Mapped[str] = mapped_column(String, unique=True, index=True)
    password_hash: Mapped[str]
```

### Rules

-   NO queries
-   NO business logic
-   NO FastAPI imports
-   NO Pydantic inheritance

------------------------------------------------------------------------

## Repository Layer (`repository.py`)

### Responsibilities

-   CRUD operations
-   queries
-   ORM interaction
-   persistence-only logic

### Repository Example

``` python
from sqlalchemy.orm import Session
from app.auth.models import User

class UserRepository:
    def __init__(self, session: Session):
        self.session = session
	
    def exists_by_email(self, email: str) -> bool:
        return (
            self.session
            .query(User)
            .filter(User.email == email)
            .first()
            is not None
        )

    def create(self, email: str, password_hash: str) -> User:
        user = User(email=email, password_hash=password_hash)
        self.session.add(user)
        self.session.commit()
        self.session.refresh(user)
        return user
```

### Rules

-   NO FastAPI imports
-   NO HTTP logic
-   NO business rules
-   NO exception-to-HTTP mapping

------------------------------------------------------------------------

## Database Session Management

### Dependency Injection (FastAPI)

``` python
from sqlalchemy.orm import sessionmaker
from sqlalchemy import create_engine

engine = create_engine(settings.DATABASE_URL)
SessionLocal = sessionmaker(bind=engine)

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

Repositories receive the session --- **not services**.

------------------------------------------------------------------------

## Repository Wiring

``` python
from fastapi import Depends
from app.database import get_db
from app.auth.repository import UserRepository

def get_user_repository(
    db = Depends(get_db),
):
    return UserRepository(db)
```

------------------------------------------------------------------------

## Database Configuration by Type

### SQLite (dev / tests)

``` env
DATABASE_URL=sqlite:///./app.db
```

Notes: - single file - no concurrency - ideal for local dev

------------------------------------------------------------------------

### PostgreSQL (recommended prod)

``` env
DATABASE_URL=postgresql+psycopg://user:pass@host:5432/dbname
```

Notes: - strong concurrency - transactions - JSON, indexes, constraints

------------------------------------------------------------------------

### MySQL / MariaDB

``` env
DATABASE_URL=mysql+pymysql://user:pass@host:3306/dbname
```

Notes: - popular - weaker defaults than Postgres - careful with
isolation levels

------------------------------------------------------------------------

### Async SQLAlchemy (optional)

``` python
from sqlalchemy.ext.asyncio import create_async_engine

engine = create_async_engine(settings.DATABASE_URL)
```

Rules: - async repos only - async services - async routers - no mixing
sync/async

------------------------------------------------------------------------

## Configuration Management

### `settings.py` Example

``` python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    DATABASE_URL: str

    class Config:
        env_file = ".env"

settings = Settings()
```

Never hardcode DB URLs.

------------------------------------------------------------------------

## Migration Strategy

Use **Alembic**: - versioned schema changes - safe upgrades - rollback
support

Rules: - never auto-create tables in prod - migrations are mandatory

------------------------------------------------------------------------

## Testing Strategy

-   use SQLite in-memory for tests
-   mock repositories for service tests
-   integration tests hit real DB

``` env
DATABASE_URL=sqlite:///:memory:
```

------------------------------------------------------------------------

## Anti-Patterns

🚫 Services importing ORM models\
🚫 Controllers querying DB\
🚫 Repositories containing business rules\
🚫 ORM models with logic

------------------------------------------------------------------------

## Mental Model

    Service = WHAT happens
    Repository = HOW data is stored
    ORM = HOW tables look

------------------------------------------------------------------------

## TL;DR

-   ORM models = structure only
-   Repositories = persistence only
-   Services = logic only
-   DB config via environment
-   Easy DB swapping

> Databases change. Clean boundaries survive.
