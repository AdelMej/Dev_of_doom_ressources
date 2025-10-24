## 💀 **God-Tier SQL — Query Optimization Cheat Sheet**

---

### ⚙️ **1. Always Start With EXPLAIN**

> Before optimizing, **see what the database is doing**.

```sql
-- PostgreSQL
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'john@example.com';

-- MySQL
EXPLAIN SELECT * FROM users WHERE email = 'john@example.com';
```

**Key things to look for:**

|Term|Meaning|
|---|---|
|`Seq Scan`|Sequential scan — no index used ❌|
|`Index Scan`|Using an index ✅|
|`Index Only Scan`|Fully served by index (best) 🥇|
|`Filter`|Conditions applied after fetch (less efficient)|
|`Nested Loop`|Good for small joins|
|`Hash Join`|Good for large joins|

✅ Goal: **Index Only Scans** or at least **Index Scans** on big tables.

---

### 🧠 **2. Limit the Data Early**

> The fewer rows processed, the faster everything is.

```sql
-- Instead of this:
SELECT * FROM users WHERE active = TRUE ORDER BY created_at DESC;

-- Do this:
SELECT id, username, email
FROM users
WHERE active = TRUE
ORDER BY created_at DESC
LIMIT 50;
```

**Rules:**

- Only select columns you need.
- Always use `LIMIT` in dev/testing.
- Avoid `SELECT *` in production code.

---

### 🧱 **3. Index for the Right Queries**

> Indexes help only when they match your query pattern.

✅ **Effective:**

```sql
CREATE INDEX idx_orders_customer_date
ON orders (customer_id, order_date);
SELECT * FROM orders WHERE customer_id = 5 ORDER BY order_date DESC;
```

❌ **Ineffective:**

```sql
SELECT * FROM orders WHERE order_date = '2025-10-21';
-- Only the second column — index not used efficiently.
```

**Golden Rule:**

> The index must start with the columns used first in your WHERE clause.

---

### 🔗 **4. Optimize Joins**

> Bad joins can cripple performance.

`-- ✅ Good SELECT o.id, c.name FROM orders o JOIN customers c ON o.customer_id = c.id WHERE c.active = TRUE;  -- ❌ Bad SELECT * FROM orders, customers WHERE orders.customer_id = customers.id;`

**Tips:**

- Use **INNER JOIN** instead of old-style joins.
    
- Join on indexed columns.
    
- Filter **before joining** if possible.
    

---

### 📊 **5. Use Proper Data Types**

> Mismatched or oversized types ruin index efficiency.

✅ Use:

- `INT` instead of `BIGINT` when possible.
- `TIMESTAMP` instead of `TEXT` for dates    
- `BOOLEAN` instead of `'Y'/'N'`.

⚠️ Avoid:

```sql
WHERE CAST(user_id AS TEXT) = '42'  -- breaks index usage!
```

---

### 🧩 **6. Cache Common Queries (Materialized Views)**

> Don’t recompute heavy aggregations every time.

```sql
CREATE MATERIALIZED VIEW fast_stats AS
SELECT region, COUNT(*) AS users, AVG(age) AS avg_age
FROM users
GROUP BY region;

REFRESH MATERIALIZED VIEW fast_stats;
```

**Use case:** dashboards, reports, analytics.

---

### 🚀 **7. Partition Big Tables**

> Split huge tables into smaller, faster chunks.

**PostgreSQL Example:**

```sql
CREATE TABLE logs (
  id SERIAL PRIMARY KEY,
  created_at TIMESTAMP NOT NULL
) PARTITION BY RANGE (created_at);

CREATE TABLE logs_2025_10 PARTITION OF logs
FOR VALUES FROM ('2025-10-01') TO ('2025-11-01');
```

✅ Only queries touching that month’s data are scanned.  
**Huge** for log or analytics tables.

---

### 🧮 **8. Avoid Expensive Operations**

|Operation|Alternative|
|---|---|
|`LIKE '%text%'`|Use full-text index|
|`DISTINCT`|Ensure proper indexing or use `GROUP BY`|
|`OR` chains|Use `IN` instead|
|`NOT IN`|Use `NOT EXISTS`|
|`ISNULL(col) = FALSE`|Use `col IS NOT NULL`|
|`ORDER BY RAND()`|Avoid — very expensive|

---

### 💾 **9. Optimize ORDER BY and GROUP BY**

```sql
-- ✅ Create index on sorted columns
CREATE INDEX idx_sales_region_date ON sales (region, sale_date);

-- ✅ Use same order in SELECT
SELECT region, sale_date, SUM(amount)
FROM sales
GROUP BY region, sale_date
ORDER BY region, sale_date;
```

Indexes can eliminate sorting entirely.

---

### 🧠 **10. Use Query Hints (Carefully)**

**PostgreSQL:** use planner settings like:

```sql
SET enable_seqscan = OFF;
```

**MySQL:**

```sql
SELECT /*+ INDEX(users idx_users_email) */ * FROM users WHERE email='a@b.com';
```

Use hints only when you know the optimizer is wrong — not as a default.

---

### 🔥 **11. Monitor and Analyze**

**PostgreSQL:**

```sql
EXPLAIN (ANALYZE, BUFFERS) SELECT ...;
SELECT * FROM pg_stat_statements ORDER BY total_time DESC LIMIT 10;
```

**MySQL:**

```sql
SHOW PROFILE FOR QUERY 1;
SHOW STATUS LIKE 'Handler_read%';
```

These show cache hits, buffer usage, and query timings.

---

### 🧰 **12. Golden Optimization Rules**

|Rule|Why|
|---|---|
|Use `EXPLAIN` before guessing|Know what’s slow|
|Index only what you query|Balance speed vs cost|
|Avoid functions on indexed columns in WHERE|They break index usage|
|Limit data early|Less data = faster execution|
|Partition large data|Keeps queries local|
|Cache aggregates|Recompute only when needed|
|Use `ANALYZE` to update stats|Keeps optimizer smart|

---

### 🏁 **Final Wisdom**

> 🧙‍♂️ “A slow query isn’t fixed by magic; it’s fixed by **knowing what it’s really doing**.”

Check the plan, shrink the dataset, and **align your indexes with your access patterns.**