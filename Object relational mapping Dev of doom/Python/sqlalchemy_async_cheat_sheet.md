# SQLAlchemy Async Cheat Sheet (Clean Architecture Friendly)

## 1. Engine
```python
from sqlalchemy.ext.asyncio import create_async_engine

engine = create_async_engine(
    DATABASE_URL,
    echo=False,
    future=True,
)
```

## 2. Session Factory
```python
from sqlalchemy.ext.asyncio import async_sessionmaker, AsyncSession

SessionFactory = async_sessionmaker(
    engine,
    expire_on_commit=False,
)
```

Type hint:
```python
async_sessionmaker[AsyncSession]
```

## 3. Session / Unit of Work
```python
class SqlUoW:
    def __init__(self, session_factory: async_sessionmaker[AsyncSession]):
        self._session_factory = session_factory

    async def __aenter__(self):
        self.session = self._session_factory()
        return self

    async def __aexit__(self, exc_type, exc, tb):
        if exc_type:
            await self.session.rollback()
        else:
            await self.session.commit()
        await self.session.close()
```

## 4. Querying
```python
from sqlalchemy import select
```

### One
```python
res = await session.execute(
    select(User).where(User.email == email)
)
user = res.scalar_one()
```

### One or none
```python
user = res.scalar_one_or_none()
```

### Many
```python
res = await session.execute(select(User))
users = res.scalars().all()
```

## 5. Insert
```python
session.add(user)
session.add_all(users)
```

## 6. Update
```python
user.disabled_at = utcnow()
```

Bulk:
```python
from sqlalchemy import update

await session.execute(
    update(User)
    .where(User.id == user_id)
    .values(disabled_at=utcnow())
)
```

## 7. Delete
```python
await session.delete(user)
```

Bulk:
```python
from sqlalchemy import delete

await session.execute(
    delete(RefreshToken)
    .where(RefreshToken.expires_at < now)
)
```

## 8. Relationships
```python
user.roles
```

Eager:
```python
from sqlalchemy.orm import selectinload

select(User).options(selectinload(User.roles))
```

## 9. Association Proxy
```python
user.roles
role.users
```

## 10. Result Helpers
| Method | Meaning |
|------|--------|
| scalar_one | exactly one |
| scalar_one_or_none | zero or one |
| scalars().all | many |

## 11. Architecture Rules
- Engine: app startup
- SessionFactory: DI
- Session: UoW
- Repos translate ORM → Domain
- Domain never sees ORM

## Mental Model
```
Engine → SessionFactory → Session → Repository → Domain
```
