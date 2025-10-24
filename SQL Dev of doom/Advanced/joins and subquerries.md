## ⚙️ **Advanced SQL — Joins & Subqueries Cheat Sheet**

---

### 🔗 **1. What Is a Join?**

> A **join** combines rows from two or more tables based on a related column.

Common use:

```sql
SELECT orders.id, users.username
FROM orders
JOIN users ON orders.user_id = users.id;
```

---

### 🧱 **2. INNER JOIN**

> Returns rows **only when there’s a match** in both tables.

```sql
SELECT u.username, o.total
FROM users u
INNER JOIN orders o ON u.id = o.user_id;
```

✅ Use for: connected data (like customers and their orders).  
❌ Doesn’t return users without orders.

---

### 🌕 **3. LEFT JOIN (a.k.a. LEFT OUTER JOIN)**

> Returns **all rows from the left table**, and matching ones from the right.

```sql
SELECT u.username, o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

- Unmatched `orders` → `NULL` values.  
    ✅ Use to find “users without orders.”

```sql
SELECT u.username
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE o.id IS NULL;
```

---

### 🌑 **4. RIGHT JOIN**

> Returns all rows from the right table, matched ones from the left.

```sql
SELECT o.id, u.username
FROM orders o
RIGHT JOIN users u ON u.id = o.user_id;
```

📌 Same logic as `LEFT JOIN`, just reversed — rarely used in practice.

---

### 🌕🌑 **5. FULL OUTER JOIN**

> Returns all rows from both tables, matched or not.

```sql
SELECT u.username, o.total
FROM users u
FULL OUTER JOIN orders o ON u.id = o.user_id;
```

✅ Missing matches get `NULL`s on their side.  
⚠️ Not supported in MySQL (use `UNION` workaround).

---

### 🧩 **6. CROSS JOIN**

> Combines _every row from A with every row from B_ (Cartesian product).

```sql
SELECT u.username, p.name
FROM users u
CROSS JOIN products p;
```

💥 `10 users × 5 products = 50 rows`  
✅ Useful for generating all possible combinations.

---

### 🧠 **7. SELF JOIN**

> A table joins **itself** — compare rows within one table.

```sql
SELECT e1.name AS employee, e2.name AS manager
FROM employees e1
JOIN employees e2 ON e1.manager_id = e2.id;
```

✅ Common for hierarchical or recursive data (like org charts).

---

### ⚙️ **8. NATURAL JOIN**

> Joins automatically on columns with the same name.

```sql
SELECT * FROM users NATURAL JOIN orders;
```

⚠️ Avoid in production — implicit behavior can cause issues.  
✅ Good for quick tests or small queries.

---

### 💡 **9. USING Clause**

> Shorthand when both columns share the same name.

```sql
SELECT u.username, o.total
FROM users u
JOIN orders o USING (user_id);
```

Cleaner than:

```sql
ON u.user_id = o.user_id
```

---

## 🔍 **Subqueries**

---

### 🧱 **1. What’s a Subquery?**

> A **query inside another query**, used to compute or filter data dynamically.

```sql
SELECT username
FROM users
WHERE id IN (SELECT user_id FROM orders WHERE total > 100);
```

---

### 📊 **2. Subquery in SELECT**

> Use a subquery to compute derived values per row.

```sql
SELECT
  u.username,
  (SELECT COUNT(*) FROM orders o WHERE o.user_id = u.id) AS order_count
FROM users u;
```

✅ Handy for dashboards, metrics, and aggregates.

---

### ⚙️ **3. Subquery in FROM (Derived Table)**

```sql
SELECT region, AVG(avg_age)
FROM (
  SELECT region, AVG(age) AS avg_age
  FROM users
  GROUP BY region
) AS sub
GROUP BY region;
```

✅ Use to pre-aggregate or simplify complex queries.

---

### 🧩 **4. Subquery in WHERE**

> Dynamically filter data based on another query’s result.

```sql
SELECT *
FROM products
WHERE price > (
  SELECT AVG(price)
  FROM products
);
```

✅ Compares each product against the overall average.

---

### 🔁 **5. EXISTS and NOT EXISTS**

> Check if _a matching row exists_ in another table.

```sql
SELECT u.username
FROM users u
WHERE EXISTS (
  SELECT 1 FROM orders o WHERE o.user_id = u.id
);
```

✅ Faster and more reliable than `IN()` for large datasets.

**Opposite:**

```sql
WHERE NOT EXISTS (
  SELECT 1 FROM orders o WHERE o.user_id = u.id
);
```

---

### ⚡ **6. IN vs EXISTS — Quick Rules**

|Condition|Best Choice|
|---|---|
|Small subquery result|`IN`|
|Large subquery result|`EXISTS`|
|Nullable columns|`EXISTS` (avoids NULL pitfalls)|
|Complex joins|`EXISTS` (optimizer-friendly)|

---

### 🔍 **7. Correlated Subquery**

> Depends on the outer query — executes once per row.

```sql
SELECT u.username
FROM users u
WHERE u.balance > (
  SELECT AVG(balance)
  FROM users u2
  WHERE u2.region = u.region
);
```

✅ Great for region-based or grouped comparisons.  
⚠️ Can be slow — rewrite as `JOIN` if possible.

---

### 💀 **8. Subquery vs Join — Performance Tips**

|Goal|Use|
|---|---|
|Combine columns|`JOIN`|
|Filter rows|`EXISTS` or `IN`|
|Aggregate comparison|Subquery|
|Avoid duplicates|Subquery|
|Large dataset join|Indexed `JOIN` (faster)|

---

### 🧠 **9. Common Table Expressions (CTEs)**

> Define a temporary, readable query block (like a named subquery).

```sql
WITH user_orders AS (
  SELECT user_id, COUNT(*) AS order_count
  FROM orders
  GROUP BY user_id
)
SELECT u.username, user_orders.order_count
FROM users u
JOIN user_orders ON u.id = user_orders.user_id;
```

✅ Improves readability and reusability.  
💡 Supported in PostgreSQL, MySQL 8+, SQLite, SQL Server.

---

### 🏁 **10. Final Wisdom**

> “JOINs are relationships — subqueries are thoughts.  
> Use joins to **connect**, and subqueries to **reason**.” 🧠