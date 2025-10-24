# 🧠 **Python SQLAlchemy Complete Cheat Sheet (2025 Edition)**

---

## ⚙️ Installation

```bash
pip install sqlalchemy
# Optional: for MySQL, PostgreSQL, SQLite drivers
pip install mysqlclient psycopg2-binary aiosqlite
```

---

## 🧩 Basic Import & Engine Setup

```python
from sqlalchemy import create_engine

# SQLite (local file)
engine = create_engine("sqlite:///example.db", echo=True)

# MySQL
# engine = create_engine("mysql+mysqldb://user:password@localhost/mydatabase")

# PostgreSQL
# engine = create_engine("postgresql+psycopg2://user:password@localhost/mydatabase")
```

> 💡 `echo=True` logs all SQL commands (useful for debugging).

---

## 🧱 SQLAlchemy Core (SQL Expression Language)

### Metadata, Tables, and Columns

```python
from sqlalchemy import MetaData, Table, Column, Integer, String

metadata = MetaData()

users = Table(
    "users", metadata,
    Column("id", Integer, primary_key=True),
    Column("name", String(50)),
    Column("age", Integer),
)
```

### Create Tables

```python
metadata.create_all(engine)
```
### Drop Tables

```python
metadata.drop_all(engine)
```

---

### Insert Rows

```python
with engine.connect() as conn:
    conn.execute(users.insert().values(name="Alice", age=25))
    conn.commit()
```

### Bulk Insert

```python
with engine.begin() as conn:
    conn.execute(users.insert(), [
        {"name": "Bob", "age": 30},
        {"name": "Charlie", "age": 22},
    ])
```

---

### Select Data

```python
from sqlalchemy import select

stmt = select(users)
with engine.connect() as conn:
    results = conn.execute(stmt)
    for row in results:
        print(row)
```
### Select With Conditions

```python
stmt = select(users).where(users.c.age > 25)
```

### Select Specific Columns

```python
stmt = select(users.c.name, users.c.age)
```

### Order, Limit, Offset

```python
stmt = select(users).order_by(users.c.age.desc()).limit(5).offset(2)
```

---

### Update Rows

```python
stmt = users.update().where(users.c.name == "Alice").values(age=26)
with engine.begin() as conn:
    conn.execute(stmt)
```

### Delete Rows

```python
stmt = users.delete().where(users.c.name == "Bob")
with engine.begin() as conn:
    conn.execute(stmt)
```

---

### Transactions

```python
with engine.begin() as conn:
    conn.execute(users.insert().values(name="Zoe", age=33))
    conn.execute(users.update().where(users.c.name == "Alice").values(age=27))
```

---

## 🧰 SQLAlchemy ORM (Object Relational Mapper)

---

### Base and Models

```python
from sqlalchemy.orm import declarative_base
from sqlalchemy import Column, Integer, String

Base = declarative_base()

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)

    def __repr__(self):
        return f"<User(name={self.name}, age={self.age})>"
```

---

### Create Tables from ORM

```python
Base.metadata.create_all(engine)
```

---

### Session Setup

```python
from sqlalchemy.orm import sessionmaker

Session = sessionmaker(bind=engine)
session = Session()
```

---

### Insert Rows (ORM Way)

```python
user = User(name="Alice", age=25)
session.add(user)
session.commit()
```

### Insert Multiple

```python
user = User(name="Alice", age=25)
session.add(user)
session.commit()
```

---

### Query All

```python
users = session.query(User).all()
for u in users:
    print(u.id, u.name, u.age)
```

### Filter Data

```python
session.query(User).filter(User.age > 25).all()
session.query(User).filter_by(name="Alice").first()
```

### Advanced Filters

```python
from sqlalchemy import and_, or_

session.query(User).filter(
    or_(User.age < 20, User.age > 50)
).all()
```
### Order and Limit

```python
session.query(User).order_by(User.age.desc()).limit(3).all()
```

---

### Update Record

```python
alice = session.query(User).filter_by(name="Alice").first()
alice.age = 28
session.commit()
```

### Delete Record

```python
session.delete(alice)
session.commit()
```

---

## 🔗 Relationships

### One-to-Many Example

```python
from sqlalchemy import ForeignKey
from sqlalchemy.orm import relationship

class Post(Base):
    __tablename__ = "posts"

    id = Column(Integer, primary_key=True)
    title = Column(String(100))
    user_id = Column(Integer, ForeignKey("users.id"))

    user = relationship("User", back_populates="posts")

User.posts = relationship("Post", back_populates="user")
```

### Insert Related Data

```python
alice = User(name="Alice", age=25)
post1 = Post(title="Hello World", user=alice)
session.add_all([alice, post1])
session.commit()
```
### Query with Join

```python
session.query(User, Post).join(Post).filter(User.name == "Alice").all()
```

---

## 🧩 Many-to-Many Example

```python
from sqlalchemy import Table

association = Table(
    "association", Base.metadata,
    Column("user_id", Integer, ForeignKey("users.id")),
    Column("group_id", Integer, ForeignKey("groups.id"))
)

class Group(Base):
    __tablename__ = "groups"
    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    users = relationship("User", secondary=association, back_populates="groups")

User.groups = relationship("Group", secondary=association, back_populates="users")
```

---

## ⚡ Executing Raw SQL

```python
with engine.connect() as conn:
    result = conn.execute(text("SELECT * FROM users WHERE age > :age"), {"age": 25})
    for row in result:
        print(row)
```

---

## 🔒 Transactions with ORM

```python
from sqlalchemy.exc import SQLAlchemyError

try:
    session.add(User(name="Eve", age=35))
    session.commit()
except SQLAlchemyError as e:
    session.rollback()
    print("Error:", e)
finally:
    session.close()
```

---

## 🧠 Async SQLAlchemy (Optional Modern Pattern)

```python
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker

engine = create_async_engine("sqlite+aiosqlite:///async.db", echo=True)
AsyncSessionLocal = sessionmaker(engine, class_=AsyncSession, expire_on_commit=False)
```

---

## 🧾 Quick Reference Table

|Task|ORM Example|Core Example|
|---|---|---|
|Create Table|`Base.metadata.create_all(engine)`|`metadata.create_all(engine)`|
|Insert|`session.add(User(...))`|`conn.execute(users.insert().values(...))`|
|Select|`session.query(User)`|`select(users)`|
|Filter|`.filter(User.age > 25)`|`.where(users.c.age > 25)`|
|Update|`obj.age = 30; session.commit()`|`users.update().values(age=30)`|
|Delete|`session.delete(obj)`|`users.delete().where(...)`|
|Commit|`session.commit()`|`conn.commit()`|
|Rollback|`session.rollback()`|`conn.rollback()`|

---

## 🧯 Best Practices

✅ Use **ORM** for readability, **Core** for performance-critical operations.  
✅ Always close sessions (`session.close()`).  
✅ Use **parameterized queries** to prevent SQL injection.  
✅ Add `__repr__` methods for debugging models.  
✅ Use `scoped_session` in multi-threaded environments (like web servers).  
✅ For production, disable `echo=True`.  
✅ Keep models in a separate `models.py` file for clarity.