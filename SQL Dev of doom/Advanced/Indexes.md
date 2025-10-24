## ⚙️ **Advanced SQL — Indexes Cheat Sheet**

---

### 🧠 **1. What Is an Index?**

An **index** is like a _lookup table_ that helps the database find rows faster, just like an index in a book.

✅ **Indexes speed up:**

- `SELECT` queries (especially with `WHERE`, `JOIN`, and `ORDER BY`)
- Range queries and sorting

⚠️ **Indexes slow down:**

- `INSERT`, `UPDATE`, and `DELETE` (because the index must be updated too)

---

### 🧱 **2. Creating Indexes**

```sql
CREATE INDEX index_name ON table_name (column_name);
```

**Example:**

```sql
CREATE INDEX idx_users_email ON users (email);
```

Now this query is faster:

```sql
SELECT * FROM users WHERE email = 'john@example.com';
```

---

### 🔢 **3. Multi-Column Indexes**

```sql
CREATE INDEX idx_orders_customer_date
ON orders (customer_id, order_date);
```

✅ **Best when:** queries use both columns in filters or sorting:

```sql
SELECT * FROM orders
WHERE customer_id = 5
ORDER BY order_date DESC;
```

⚠️ **Order matters:**

- `(customer_id, order_date)` can optimize filters starting with `customer_id`
- But not the reverse (`WHERE order_date = ...` alone)

---

### 🔍 **4. Unique Indexes**

```sql
CREATE UNIQUE INDEX idx_users_username ON users (username);
```

- Prevents duplicate values (like a secondary unique key).
- MySQL’s `UNIQUE` constraint under the hood is just a unique index.

---

### 🧮 **5. Partial / Filtered Indexes**

> Only index rows that match a condition — very efficient for sparse data.

**PostgreSQL:**

```sql
CREATE INDEX idx_active_users
ON users (email)
WHERE active = TRUE;
```

Now only active users are indexed.

---

### 📊 **6. Expression / Functional Indexes**

> Index computed values or expressions.

**PostgreSQL:**

```sql
CREATE INDEX idx_lower_email
ON users (LOWER(email));
```

Now queries like:

```sql
SELECT * FROM users WHERE LOWER(email) = 'john@example.com';
```

will use the index efficiently.

---

### ⚡ **7. Full-Text Indexes**

For searching text fields (instead of using `LIKE '%word%'`).

**PostgreSQL:**

```sql
CREATE INDEX idx_posts_search
ON posts USING GIN (to_tsvector('english', content));

SELECT * FROM posts
WHERE to_tsvector('english', content) @@ plainto_tsquery('SQL tuning');
```

**MySQL:**

```sql
CREATE FULLTEXT INDEX ft_posts_content ON posts(content);

SELECT * FROM posts
WHERE MATCH(content) AGAINST('SQL tuning');
```

---

### 🧩 **8. Index Maintenance**

```sql
-- Remove an index
DROP INDEX index_name;                -- PostgreSQL
DROP INDEX table_name.index_name;     -- MySQL

-- Rename an index (PostgreSQL)
ALTER INDEX old_name RENAME TO new_name;

-- Rebuild or reindex (PostgreSQL)
REINDEX INDEX index_name;
```
---

### 🧰 **9. Checking Index Usage**

```sql
-- PostgreSQL: show query plan
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'john@example.com';

-- MySQL: show query plan
EXPLAIN SELECT * FROM users WHERE email = 'john@example.com';
```

Look for `"Index Scan"` or `"Using index"` — that means it’s working.

---

### 💡 **10. Best Practices**

|Tip|Why|
|---|---|
|Index columns used in `WHERE`, `JOIN`, `ORDER BY`, `GROUP BY`|Core performance gain|
|Don’t index everything|Wastes space and slows writes|
|Use composite indexes wisely|Order matters!|
|Drop unused indexes|They consume CPU and disk|
|Use `EXPLAIN` often|See what’s actually optimized|
|Prefer `BTREE` (default)|Best for most cases|
|Use `GIN` / `HASH` for special data|Full-text, JSONB, or exact matches|

---

### 🚀 **Pro Trick**

If your table has time-based data (like logs), combine a **timestamp** with another key in a composite index:

```sql
CREATE INDEX idx_logs_service_time ON logs (service_id, created_at DESC);
```

→ blazing-fast lookups for recent entries per service.