# 16 — Alembic Migrations (Async SQLAlchemy) Cheat Sheet

This cheat sheet explains **how to use Alembic correctly** with async SQLAlchemy, without breaking your architecture or your sanity.

---

## 1. What Alembic actually is

Alembic is:
- a **schema migration tool**
- version control for your database
- NOT an ORM
- NOT runtime logic

Think:
> Git, but for database structure.

---

## 2. One important truth (read first)

**Alembic runs synchronously.**  
Even in async projects.

This is normal.
This is correct.
Do not fight it.

---

## 3. Initial setup

Install:
```bash
pip install alembic
```

Initialize:
```bash
alembic init alembic
```

Result:
```text
alembic/
  env.py
  versions/
alembic.ini
```

---

## 4. Point Alembic to your models

In `alembic/env.py`:

```python
from app.db.base import Base  # your declarative Base
target_metadata = Base.metadata
```

This is how Alembic discovers schema changes.

---

## 5. Async engine integration (CRITICAL)

Even with async SQLAlchemy, Alembic uses a **sync engine**.

In `env.py`:

```python
from sqlalchemy import engine_from_config
from sqlalchemy import pool

def run_migrations_online():
    connectable = engine_from_config(
        config.get_section(config.config_ini_section),
        prefix="sqlalchemy.",
        poolclass=pool.NullPool,
    )

    with connectable.connect() as connection:
        context.configure(
            connection=connection,
            target_metadata=target_metadata,
        )

        with context.begin_transaction():
            context.run_migrations()
```

Do NOT use `create_async_engine()` here.

---

## 6. Database URL management

Use environment variables:

```ini
sqlalchemy.url = postgresql://user:pass@localhost/db
```

Or dynamically in `env.py`:

```python
config.set_main_option("sqlalchemy.url", DATABASE_URL)
```

---

## 7. Creating migrations

Autogenerate (most common):

```bash
alembic revision --autogenerate -m "add users table"
```

Always **review generated files**.

Alembic is helpful, not psychic.

---

## 8. Applying migrations

```bash
alembic upgrade head
```

Rollback:
```bash
alembic downgrade -1
```

---

## 9. Migration file anatomy

```python
def upgrade():
    op.create_table(...)

def downgrade():
    op.drop_table(...)
```

Upgrade must be reversible.

If not → document why.

---

## 10. Common footguns (DO NOT DO)

❌ Editing old migrations  
❌ Deleting applied migrations  
❌ Running Alembic at app startup  
❌ Mixing schema logic into services  

---

## 11. SQLite vs Postgres migrations

SQLite limitations:
- limited ALTER TABLE
- no DROP COLUMN
- table rebuilds

Postgres:
- full ALTER support
- safer evolution

Always test migrations on Postgres before prod.

---

## 12. Migration strategy (recommended)

```text
local dev     → SQLite
CI            → Postgres (apply migrations)
production    → Postgres (apply migrations)
```

Same migration files.

---

## 13. When NOT to use autogenerate

Avoid autogen for:
- complex refactors
- data migrations
- column type changes with transforms

Write those manually.

---

## 14. Data migrations (important)

Schema change ≠ data change.

Example:
```python
def upgrade():
    op.add_column(...)
    op.execute("UPDATE ...")
```

Data migrations are real migrations.

---

## 15. Mental model (lock this in)

- ORM models = intent
- Alembic = history
- Database = truth
- Migrations = irreversible record

---

## 16. Final truth

> If your migrations scare you,
> your production deploys should.

Alembic done right makes deployments boring.
