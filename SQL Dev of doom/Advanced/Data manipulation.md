## ⚙️ **Advanced SQL — Data Manipulation Cheat Sheet**

---

### 🧱 **1. INSERT — Add Data to a Table**

**Basic form:**

```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

**Example:**

```sql
INSERT INTO users (username, email, age)
VALUES ('john_doe', 'john@example.com', 25);
```

---

### ➕ **2. Multiple Inserts**

```sql
INSERT INTO users (username, email, age)
VALUES 
  ('alice', 'alice@mail.com', 24),
  ('bob', 'bob@mail.com', 29);
```

✅ Faster than multiple single inserts.

---

### ⚙️ **3. Insert with Default Values**

```sql
INSERT INTO logs DEFAULT VALUES;
```

→ Inserts a row with all default or `NULL` values.

---

### 🔁 **4. Insert from Another Table**

```sql
INSERT INTO archive_users (id, username, email)
SELECT id, username, email
FROM users
WHERE active = FALSE;
```

✅ Used for archiving, backups, and data migrations.

---

### 🚫 **5. Prevent Duplicate Inserts**

**PostgreSQL:**

```sql
INSERT INTO users (email, username)
VALUES ('john@example.com', 'johnny')
ON CONFLICT (email) DO NOTHING;
```

**MySQL:**

```sql
INSERT IGNORE INTO users (email, username)
VALUES ('john@example.com', 'johnny');
```

---

### 🔄 **6. UPSERT — Insert or Update**

**PostgreSQL:**

```sql
INSERT INTO users (email, username)
VALUES ('john@example.com', 'johnny')
ON CONFLICT (email)
DO UPDATE SET username = EXCLUDED.username;
```
**MySQL:**

```sql
INSERT INTO users (email, username)
VALUES ('john@example.com', 'johnny')
ON DUPLICATE KEY UPDATE username = VALUES(username);
```

✅ Essential for syncing data safely.

---

### ✏️ **7. UPDATE — Modify Existing Data**

**Basic form:**

```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

**Example:**

```sql
UPDATE users
SET age = 26
WHERE username = 'john_doe';
```

⚠️ **Always use `WHERE`** — or you’ll update _every row_.

---

### 🧠 **8. Update Using Another Table**

```sql
UPDATE users
SET department_id = d.id
FROM departments d
WHERE users.department_name = d.name;
```

✅ Cross-table updates are super handy for data normalization.

---

### 🧩 **9. Conditional Updates**

```sql
UPDATE orders
SET discount = 
  CASE 
    WHEN total > 100 THEN 0.1
    WHEN total > 50 THEN 0.05
    ELSE 0
  END;
```

✅ Great for tiered logic or rules.

---

### ❌ **10. DELETE — Remove Data**

**Basic form:**

```sql
DELETE FROM table_name
WHERE condition;
```

**Example:**

```sql
DELETE FROM users WHERE active = FALSE;
```

⚠️ No `WHERE` = wipes the entire table.

---

### 🧮 **11. Delete Using Another Table**

```sql
DELETE FROM users
USING blacklist
WHERE users.email = blacklist.email;
```

✅ Efficient for purging data in bulk.

---

### 🧰 **12. Safe Deletion Techniques**

|Technique|Purpose|
|---|---|
|`BEGIN; ... ROLLBACK;`|Test delete safely|
|Use `LIMIT`|Delete gradually|
|`DELETE RETURNING *;`|View deleted data (PostgreSQL)|
|Use `soft delete` flag|Keep history for audit|

**Example (soft delete):**

```sql
ALTER TABLE users ADD COLUMN deleted_at TIMESTAMP;

UPDATE users SET deleted_at = NOW() WHERE id = 1;
```

---

### 🔥 **13. RETURNING Clause (PostgreSQL)**

> Immediately see what changed.

```sql
UPDATE users
SET age = age + 1
WHERE active = TRUE
RETURNING id, username, age;
```

✅ Saves round-trips and simplifies app logic.

---

### 🧙‍♂️ **14. Performance Tips**

|Tip|Reason|
|---|---|
|Batch inserts (`INSERT ... VALUES (...), (...)`)|Fewer commits, faster|
|Avoid updating indexed columns often|Indexes slow down writes|
|Prefer UPSERTs for sync operations|Safer than manual checks|
|Disable triggers/constraints temporarily for bulk ops|Huge speed boost (use carefully!)|
|Use transactions for grouped changes|Keeps integrity intact|

---

### 🏁 **15. Final Wisdom**

> “INSERT adds, UPDATE changes, DELETE removes —  
> but **transactions define their story together**.” ⚙️