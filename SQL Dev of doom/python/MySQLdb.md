# 🐍 Python + MySQLdb (mysqlclient) Complete Cheat Sheet

---

## ⚙️ Installation

```bash
pip install mysqlclient
```

---

## 🧩 Import & Connect

```python
import MySQLdb

db = MySQLdb.connect(
    host="localhost",
    user="root",
    passwd="password",
    db="mydatabase",
    charset="utf8mb4"  # Always good for Unicode
)

cursor = db.cursor()
```
---

## 🏗️ Database Management

### Create a Database

```sql
cursor.execute("CREATE DATABASE IF NOT EXISTS mydatabase")
```

### Select Database

```sql
cursor.execute("USE mydatabase")
```

### Show Databases

```sql
cursor.execute("SHOW DATABASES")
for dbname in cursor.fetchall():
    print(dbname)
```
---

## 🧱 Table Management

### Create a Table

```python
cursor.execute("""
CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
)
""")
```

### Drop Table

 ```python
 cursor.execute("DROP TABLE IF EXISTS users")
 ```

### Show Tables

```python
cursor.execute("SHOW TABLES")
print(cursor.fetchall())
```
### Describe Table

```python
cursor.execute("DESCRIBE users")
for row in cursor.fetchall():
    print(row)
```

---

## 💾 CRUD Operations

### Insert One

```python
cursor.execute("INSERT INTO users (name, email, age) VALUES (%s, %s, %s)", 
               ("Alice", "alice@example.com", 25))
db.commit()
```
### Insert Many

```python
data = [
    ("Bob", "bob@example.com", 30),
    ("Charlie", "charlie@example.com", 22)
]
cursor.executemany("INSERT INTO users (name, email, age) VALUES (%s, %s, %s)", data)
db.commit()
```
### Select All

```python
cursor.execute("SELECT * FROM users")
for row in cursor.fetchall():
    print(row)
```

### Select With Condition

```python
cursor.execute("SELECT name, age FROM users WHERE age > %s", (25,))
results = cursor.fetchall()
```

### Select with LIKE, ORDER, LIMIT

```python
cursor.execute("SELECT * FROM users WHERE name LIKE %s ORDER BY age DESC LIMIT 5", ("%a%",))
```

### Update Records

```python
cursor.execute("UPDATE users SET age = %s WHERE name = %s", (28, "Alice"))
db.commit()
```

### Delete Records

```python
cursor.execute("DELETE FROM users WHERE name = %s", ("Charlie",))
db.commit()
```

---

## 🧠 Fetch Methods

```python
cursor.fetchone()   # Fetch 1 row
cursor.fetchmany(5) # Fetch 5 rows
cursor.fetchall()   # Fetch all rows
```
---

## 🔒 Transactions

```python
try:
    cursor.execute("UPDATE users SET age = age + 1")
    db.commit()
except:
    db.rollback()
```

---

## 📦 Dictionary Cursor (Access Columns by Name)

```python
import MySQLdb.cursors

db = MySQLdb.connect(
    host="localhost",
    user="root",
    passwd="password",
    db="mydatabase",
    cursorclass=MySQLdb.cursors.DictCursor
)

cursor = db.cursor()
cursor.execute("SELECT * FROM users")
for row in cursor.fetchall():
    print(row["name"], row["age"])
```
---

## 🧰 Useful Utilities

### Get Last Inserted ID

```python
cursor.execute("INSERT INTO users (name, email, age) VALUES (%s, %s, %s)",
               ("Diana", "diana@example.com", 27))
db.commit()
print("Last ID:", cursor.lastrowid)
```

### Get Row Count

```python
cursor.execute("SELECT * FROM users")
print("Rows:", cursor.rowcount)
```

---

## ⚙️ Stored Procedures

### Create Procedure (SQL side)

```sql
DELIMITER //
CREATE PROCEDURE GetAllUsers()
BEGIN
  SELECT * FROM users;
END //
DELIMITER ;
```

### Call Procedure (Python)

```python
cursor.callproc("GetAllUsers")
for result in cursor.fetchall():
    print(result)
```

---

## 🧪 Error Handling

```python
import MySQLdb

try:
    db = MySQLdb.connect("localhost", "root", "password", "mydatabase")
    cursor = db.cursor()
    cursor.execute("SELECT * FROM unknown_table")
except MySQLdb.Error as e:
    print(f"Error {e.args[0]}: {e.args[1]}")
finally:
    db.close()
```
---

## ⚡ Context Manager Style (Auto-Close Cursor)

```python
with db.cursor() as cursor:
    cursor.execute("SELECT * FROM users WHERE age > %s", (25,))
    for row in cursor.fetchall():
        print(row)
```

---

## 🧩 Connection Pooling (via `DBUtils`)

```bash
pip install DBUtils
```

```python
from DBUtils.PooledDB import PooledDB
import MySQLdb

pool = PooledDB(MySQLdb, mincached=2, maxcached=5,
                host="localhost", user="root", passwd="password", db="mydatabase")

conn = pool.connection()
cursor = conn.cursor()
cursor.execute("SELECT NOW()")
print(cursor.fetchone())
cursor.close()
conn.close()
```

---

## 🧼 Closing Connections

```python
cursor.close()
db.close()
```

---

## 🧾 Quick Command Reference

|Action|Code Snippet|
|---|---|
|Connect to DB|`MySQLdb.connect()`|
|Create Cursor|`db.cursor()`|
|Execute SQL|`cursor.execute(sql, params)`|
|Fetch One|`cursor.fetchone()`|
|Fetch All|`cursor.fetchall()`|
|Commit Changes|`db.commit()`|
|Rollback|`db.rollback()`|
|Last Inserted ID|`cursor.lastrowid`|
|Row Count|`cursor.rowcount`|
|Dict Cursor|`cursorclass=MySQLdb.cursors.DictCursor`|
|Close Cursor/Connection|`cursor.close()`, `db.close()`|

---

## 🧠 Pro Tips

- ✅ Always use **parameterized queries** (`%s`) to prevent SQL injection.
- ⚡ Use **`DictCursor`** for cleaner, readable data.
- 🔁 Commit **only after** inserts, updates, or deletes.
- 🧩 Use **connection pooling** for web apps or multi-threaded environments.
- 🧯 Always close connections in `finally` or `with` blocks.
- 🧑‍💻 Test your SQL queries in MySQL CLI before using them in Python.