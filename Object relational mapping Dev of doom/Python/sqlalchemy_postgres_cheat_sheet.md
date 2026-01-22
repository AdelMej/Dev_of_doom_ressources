# SQLAlchemy 2.0 + PostgreSQL Cheat Sheet

This cheat sheet summarizes **modern SQLAlchemy (2.0 style)** using
`Mapped` / `mapped_column`, async usage, and PostgreSQL best practices.

------------------------------------------------------------------------

## Declarative Base

``` python
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
    pass
```

------------------------------------------------------------------------

## Basic Model

``` python
from sqlalchemy.orm import Mapped, mapped_column
from sqlalchemy import String

class Example(Base):
    __tablename__ = "examples"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(100), nullable=False)
```

------------------------------------------------------------------------

## UUID Primary Key

``` python
import uuid
from sqlalchemy import UUID

id: Mapped[uuid.UUID] = mapped_column(
    UUID(as_uuid=True),
    primary_key=True
)
```

------------------------------------------------------------------------

## Server-owned timestamps

``` python
from sqlalchemy import DateTime, text
from datetime import datetime

created_at: Mapped[datetime] = mapped_column(
    DateTime(timezone=True),
    server_default=text("now()"),
    init=False
)
```

------------------------------------------------------------------------

## Foreign Keys

``` python
from sqlalchemy import ForeignKey

user_id: Mapped[uuid.UUID] = mapped_column(
    ForeignKey("app.users.id"),
    nullable=False
)
```

------------------------------------------------------------------------

## Relationships

### Many-to-One

``` python
from sqlalchemy.orm import relationship

user: Mapped["User"] = relationship(
    "User",
    back_populates="credit_ledger_entries",
    lazy="joined"
)
```

### One-to-Many

``` python
credit_ledger_entries: Mapped[list["CreditLedger"]] = relationship(
    "CreditLedger",
    back_populates="user",
    lazy="selectin",
    cascade="none",
    passive_deletes=True
)
```

------------------------------------------------------------------------

## TYPE_CHECKING Pattern

``` python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from .users import User
```

------------------------------------------------------------------------

## Postgres Enums

``` python
from sqlalchemy.dialects.postgresql import ENUM

status: Mapped[SessionStatus] = mapped_column(
    ENUM(
        SessionStatus,
        name="session_status",
        schema="app",
        create_type=False
    ),
    nullable=False
)
```

------------------------------------------------------------------------

## Append-only Ledger Pattern

-   No UPDATE
-   No DELETE
-   Balance computed by triggers

``` python
balance_after_cents: Mapped[int] = mapped_column(
    Integer,
    nullable=False,
    init=False
)
```

------------------------------------------------------------------------

## Roles + RLS Model

``` text
app_user    → RLS enforced
app_system  → RLS enforced
app_admin   → BYPASS RLS (cron / migrations)
```

------------------------------------------------------------------------

## Async Engine

``` python
from sqlalchemy.ext.asyncio import create_async_engine

engine = create_async_engine(DATABASE_URL)
```

------------------------------------------------------------------------

## Async Session

``` python
from sqlalchemy.ext.asyncio import async_sessionmaker

SessionLocal = async_sessionmaker(engine, expire_on_commit=False)
```

------------------------------------------------------------------------

## Principles

-   DB owns truth
-   ORM reflects schema
-   Triggers \> app logic
-   Cron \> runtime deletes