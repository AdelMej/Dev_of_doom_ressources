# 🧠 **SQLite Complete Cheat Sheet**

---

## 🧩 BASICS

### 📦 Create a database

```bash
sqlite3 mydb.db
```
👉 Creates a file named `mydb.db` in the current directory.

---

### 📋 List commands

Inside the SQLite shell:

```sql
.help
```

---

### 📄 Show all tables

```sql
.tables
```

---

### 🧱 Show a table schema

```sql
.schema users
```
---

### 🔍 See database info

```sql
.databases
```

---

### ❌ Exit

```sql
.exit
```

---

## ⚙️ DATABASE OPERATIONS (SQL)

### 🏗️ Create a table

```sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  username TEXT UNIQUE NOT NULL,
  password TEXT NOT NULL,
  role TEXT DEFAULT 'user'
);
```

---

### ➕ Insert data

```sql
INSERT INTO users (username, password, role)
VALUES ('Adel', 'supersecurehash', 'admin');
```

---

### 🔍 Query data

```sql
SELECT * FROM users;
SELECT username, role FROM users;
SELECT * FROM users WHERE username='Adel';
```

---

### ✏️ Update data

```sql
UPDATE users SET role='sudoer' WHERE username='Adel';
```

---

### 🗑️ Delete data

```sql
DELETE FROM users WHERE username='test';
```

---

### 🧹 Drop table

```sql
DROP TABLE users;
```

---

### 💾 Export / Import

**Export database (to SQL file):**

```sql
sqlite3 mydb.db .dump > backup.sql
```

**Import SQL file:**

```sql
sqlite3 new.db < backup.sql
```

---

## 🧰 TERMINAL / BASH TIPS

### 🧾 Run SQL directly from shell

```sql
sqlite3 mydb.db "SELECT * FROM users;"
```

---

### 📜 Run SQL script file

```sql
sqlite3 mydb.db < script.sql
```

---

### 🪄 Output formatting

```bash
sqlite3 mydb.db
.mode column
.headers on
SELECT * FROM users;
```

Output example:

```sql
id   username   password           role
--   --------   ----------------   ----
1    Adel       supersecurehash    admin
```

---

### 🧩 Add user from Bash (non-interactive)

```bash
sqlite3 mydb.db "INSERT INTO users (username, password, role) VALUES ('demo', '1234', 'user');"
```

---

### 🧾 Readable query output from Bash

```bash
sqlite3 -header -column mydb.db "SELECT * FROM users;"
```

---

### 🔐 Secure your DB permissions

```bash
chmod 600 mydb.db
```

(only owner can read/write)

---

## 🐍 PYTHON QUICK REFERENCE

### ✅ Connect & Create Cursor

```python
import sqlite3
conn = sqlite3.connect("mydb.db")
cur = conn.cursor()
```

---

### 🏗️ Create Table (if not exists)

```python
cur.execute("""
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT UNIQUE NOT NULL,
    password TEXT NOT NULL,
    role TEXT DEFAULT 'user'
)
""")
conn.commit()
```

---

### ➕ Insert

```python
cur.execute("INSERT INTO users (username, password, role) VALUES (?, ?, ?)",
            ("Adel", "hash", "admin"))
conn.commit()
```

---

### 🔍 Select

```python
cur.execute("SELECT username, role FROM users")
print(cur.fetchall())
```
---

### ✏️ Update / Delete

```python
cur.execute("UPDATE users SET role=? WHERE username=?", ("sudoer", "Adel"))
cur.execute("DELETE FROM users WHERE username=?", ("test",))
conn.commit()
```

---

### ❌ Close

```python
conn.close()
```

---

## 🧠 EXTRA

### 📜 View all table names (Python)

```python
cur.execute("SELECT name FROM sqlite_master WHERE type='table';")
print(cur.fetchall())
```

---

### 🔍 View table schema (Python)

```python
cur.execute("PRAGMA table_info(users);")
print(cur.fetchall())
```

---

### 🧩 Change file location

SQLite DBs are just files — you can move or rename them:

```bash
mv mydb.db /etc/dashboard/auth.db
```

---

### 🧮 Count users

```sql
SELECT COUNT(*) FROM users;
```

---

### 🧰 Vacuum (clean + defrag)

```sql
VACUUM;
```

---

### 🧠 Fun fact

SQLite supports:

- ✅ Transactions (`BEGIN; COMMIT; ROLLBACK;`)
- ✅ Views, triggers, indexes
- ✅ JSON fields (since v3.9+)