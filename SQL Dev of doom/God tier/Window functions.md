## 🧠 **God Tier SQL — Window Functions Cheat Sheet**

---

### ⚙️ **1. What Are Window Functions?**

> Window functions perform calculations **across related rows** —  
> _without collapsing them into a single row._

💡 Think of it like “GROUP BY that doesn’t group.”

**Syntax:**

```sql
function_name(column) OVER (
  PARTITION BY col1
  ORDER BY col2
)
```

✅ Keeps all rows visible, adds an “extra computed view.”

---

### 🧩 **2. COUNT(), SUM(), AVG(), MIN(), MAX() OVER()**

**Example:**

```sql
SELECT
  department,
  employee,
  salary,
  AVG(salary) OVER (PARTITION BY department) AS dept_avg_salary
FROM employees;
```

✅ Computes average salary **per department**, while keeping all rows.

---

### 🧱 **3. PARTITION BY — Divide into Groups**

```sql
SELECT
  country,
  username,
  COUNT(*) OVER (PARTITION BY country) AS users_in_country
FROM users;
```

✅ Like `GROUP BY`, but the table stays ungrouped.

---

### 🪜 **4. ORDER BY — Sort Within the Window**

```sql
SELECT
  user_id,
  order_date,
  SUM(total) OVER (PARTITION BY user_id ORDER BY order_date) AS running_total
FROM orders;
```

✅ Adds a **running total** per user, chronologically.  
📈 Perfect for balances, scores, or progress tracking.

---

### 🔢 **5. ROW_NUMBER() — Unique Row Index**

```sql
SELECT
  user_id,
  order_date,
  ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY order_date) AS order_rank
FROM orders;
```
✅ Assigns a unique sequential number **within each group**.  
💡 Ideal for getting “first” or “latest” record per user.

---

### 🏅 **6. RANK() vs DENSE_RANK()**

```sql
SELECT
  username,
  score,
  RANK() OVER (ORDER BY score DESC) AS rank,
  DENSE_RANK() OVER (ORDER BY score DESC) AS dense_rank
FROM leaderboard;
```

|Function|Behavior|
|---|---|
|`RANK()`|Skips ranks after ties (`1, 2, 2, 4`)|
|`DENSE_RANK()`|No gaps (`1, 2, 2, 3`)|

✅ Great for leaderboards, competitions, or ordered reports.

---

### 🎯 **7. NTILE(n) — Divide into Buckets**

```sql
SELECT
  username,
  salary,
  NTILE(4) OVER (ORDER BY salary DESC) AS quartile
FROM employees;
```

✅ Splits rows into `n` roughly equal groups.  
📊 Used for percentiles, quartiles, or load balancing.

---

### 🔁 **8. LAG() and LEAD() — Compare to Previous/Next Row**

**Example:**

```sql
SELECT
  user_id,
  order_date,
  total,
  LAG(total, 1) OVER (PARTITION BY user_id ORDER BY order_date) AS prev_total,
  LEAD(total, 1) OVER (PARTITION BY user_id ORDER BY order_date) AS next_total
FROM orders;
```

✅ Access previous/next row values _without self-joins_.  
Perfect for:

- Detecting trends
- Tracking changes
- Detecting gaps in sequences

---

### 📅 **9. FIRST_VALUE() and LAST_VALUE()**

```sql
SELECT
  user_id,
  order_date,
  total,
  FIRST_VALUE(total) OVER (PARTITION BY user_id ORDER BY order_date) AS first_order,
  LAST_VALUE(total)  OVER (PARTITION BY user_id ORDER BY order_date
                           ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) AS last_order
FROM orders;
```

✅ Captures **first/last** value within each partition.  
⚠️ Always specify window frame (see next section).

---

### 🔲 **10. WINDOW FRAMES**

> Define the _range of rows_ a window function can see.

**Default (for ordered functions):**

```sql
RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
```

**Custom examples:**

```sql
-- Rolling 3-row average
AVG(score) OVER (
  ORDER BY test_date
  ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
)

-- Cumulative total
SUM(total) OVER (
  ORDER BY date
  ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
)
```

✅ Use `ROWS` for physical row counts, `RANGE` for value-based frames.

---

### 🔍 **11. Combining Aggregates and Windows**

```sql
SELECT
  department,
  employee,
  salary,
  SUM(salary) OVER (PARTITION BY department) AS dept_total,
  ROUND(
    100.0 * salary / SUM(salary) OVER (PARTITION BY department),
    2
  ) AS percent_of_dept
FROM employees;
```

✅ Shows _each employee’s salary share_ in their department.

---

### 🧮 **12. Window Alias (Reuse Window Definition)**

```sql
SELECT
  name,
  score,
  RANK() OVER w AS rank,
  AVG(score) OVER w AS avg_score
FROM players
WINDOW w AS (PARTITION BY team ORDER BY score DESC);
```

✅ Cleaner, reusable window definitions.

---

### ⚙️ **13. Mix with CTEs for Readability**

```sql
WITH ranked_orders AS (
  SELECT
    *,
    ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY order_date DESC) AS rn
  FROM orders
)
SELECT * FROM ranked_orders WHERE rn = 1;
```

✅ Super clean “latest order per user” query.

---

### 🚀 **14. Performance Tips**

|Tip|Why|
|---|---|
|Use proper indexes on `PARTITION` + `ORDER BY` columns|Huge speed difference|
|Avoid unnecessary window recalculations|Use `CTE`s|
|Watch out for `RANGE` — it can be slower than `ROWS`|Range = value-based comparisons|
|Prefer `ROWS` for predictable frames|More deterministic|
|Keep partitions balanced|Big skew kills performance|

---

### 🧠 **15. When to Use Window Functions**

|Use Case|Function|
|---|---|
|Ranking items|`ROW_NUMBER`, `RANK`, `DENSE_RANK`|
|Previous/next value|`LAG`, `LEAD`|
|Running totals or averages|`SUM`, `AVG` with `ORDER BY`|
|Comparing to first/last|`FIRST_VALUE`, `LAST_VALUE`|
|Percentiles / quartiles|`NTILE(n)`|
|Share within a group|`SUM() OVER (...)` with ratios|

---

### 🏁 **16. Final Wisdom**

> “Aggregates summarize groups.  
> Window functions reveal patterns **within** them.” 🔍

They turn raw data into **time series, leaderboards, KPIs, rolling stats, and trends** — all _without subqueries_.  
They’re what make SQL _analytical-grade_.