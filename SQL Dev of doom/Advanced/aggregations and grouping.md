## ⚙️ **Advanced SQL — Aggregations & Grouping Cheat Sheet**

---

### 📊 **1. Aggregate Functions Overview**

Aggregate functions summarize multiple rows into a single result.

|Function|Description|Example|
|---|---|---|
|`COUNT()`|Number of rows|`COUNT(*)`|
|`SUM()`|Total of numeric values|`SUM(price)`|
|`AVG()`|Average of numeric values|`AVG(age)`|
|`MIN()`|Minimum value|`MIN(salary)`|
|`MAX()`|Maximum value|`MAX(salary)`|

---

### 🔢 **2. COUNT() Variants**

```sql
SELECT COUNT(*) FROM users;
SELECT COUNT(email) FROM users WHERE email IS NOT NULL;
SELECT COUNT(DISTINCT country) FROM users;
```

✅ `COUNT(*)` counts all rows  
✅ `COUNT(column)` ignores `NULL`s  
✅ `DISTINCT` counts unique entries

---

### ➕ **3. SUM(), AVG(), MIN(), MAX()**

```sql
SELECT
  SUM(total) AS total_revenue,
  AVG(total) AS avg_order_value,
  MIN(total) AS smallest_order,
  MAX(total) AS largest_order
FROM orders;
```

✅ Works only on numeric columns (except `MIN`/`MAX`, which also work on dates and text).

---

### 🧱 **4. GROUP BY — Group Rows**

> Aggregates are meaningless without grouping!

```sql
SELECT country, COUNT(*) AS users_count
FROM users
GROUP BY country;
```

✅ Groups all rows by `country` and counts each group.

---

### 🧩 **5. GROUP BY Multiple Columns**

```sql
SELECT country, gender, COUNT(*) AS total
FROM users
GROUP BY country, gender;
```

✅ Each unique (country, gender) pair is one group.

---

### 🧠 **6. GROUP BY with Expressions**

```sql
SELECT EXTRACT(YEAR FROM created_at) AS year, COUNT(*) AS signups
FROM users
GROUP BY year;
```

✅ You can group by _expressions_, not just columns.

---

### 🧮 **7. HAVING — Filter Aggregates**

> `WHERE` filters **before** grouping, `HAVING` filters **after**.

```sql
SELECT country, COUNT(*) AS users_count
FROM users
GROUP BY country
HAVING COUNT(*) > 100;
```

✅ Use `HAVING` when you need to filter aggregate results.

---

### 🔄 **8. GROUP BY + ORDER BY**

```sql
SELECT country, COUNT(*) AS users_count
FROM users
GROUP BY country
ORDER BY users_count DESC;
```

✅ Sorts by the aggregate result, not the column itself.

---

### 🧮 **9. Aggregation in Joins**

```sql
SELECT d.name, COUNT(e.id) AS employee_count
FROM departments d
LEFT JOIN employees e ON e.department_id = d.id
GROUP BY d.name;
```

✅ Common in relational queries (like “departments with N employees”).

---

### 🧩 **10. DISTINCT Inside Aggregates**

```sql
SELECT COUNT(DISTINCT user_id) FROM orders;
```

✅ Counts unique users who placed at least one order.

---

### ⚙️ **11. Conditional Aggregation**

> Use `CASE` inside aggregates to count selectively.

```sql
SELECT
  SUM(CASE WHEN gender = 'M' THEN 1 ELSE 0 END) AS males,
  SUM(CASE WHEN gender = 'F' THEN 1 ELSE 0 END) AS females
FROM users;
```

✅ Perfect for dashboards and reporting.

---

### ⚡ **12. Windowed Aggregates (Without GROUP BY)**

> Compute aggregates _per row_, without collapsing the dataset.

```sql
SELECT
  username,
  country,
  COUNT(*) OVER (PARTITION BY country) AS users_in_country
FROM users;
```

✅ Adds a **running total** per partition, while keeping all rows visible.

---

### 🧮 **13. ROLLUP — Subtotals**

> Automatically creates **subtotals and grand totals**.

```sql
SELECT country, city, SUM(sales) AS total_sales
FROM stores
GROUP BY ROLLUP (country, city);
```

✅ Produces results like:

|Country|City|Total|
|---|---|---|
|France|Paris|5000|
|France|NULL|5000|
|NULL|NULL|12000|

---

### 🪜 **14. CUBE — All Combinations**

> Like ROLLUP, but gives _every possible combination_ of groupings.

```sql
SELECT region, product, SUM(sales)
FROM sales
GROUP BY CUBE (region, product);
```

✅ Creates totals per region, per product, and overall totals.

---

### 🔁 **15. GROUPING SETS — Custom Aggregations**

```sql
SELECT region, product, SUM(sales)
FROM sales
GROUP BY GROUPING SETS (
  (region, product),
  (region),
  ()
);
```

✅ Flexible control over which subtotals appear.

---

### 🧠 **16. Window Aggregates vs Group Aggregates**

|Use Case|Function|
|---|---|
|Collapse rows into groups|`GROUP BY`|
|Keep rows, add aggregate info|`OVER (PARTITION BY ...)`|

Example:

```sql
SELECT
  user_id,
  total,
  SUM(total) OVER (PARTITION BY user_id) AS user_total
FROM orders;
```

---

### 🚀 **17. Performance Tips**

|Tip|Why|
|---|---|
|Use indexes on GROUP BY columns|Faster grouping|
|Avoid DISTINCT in huge tables|Expensive|
|Use `HAVING` only after filtering with `WHERE`|Reduces processed rows|
|Combine `ROLLUP` or `CUBE` carefully|Can explode result size|
|Aggregate in CTEs for clarity|Keeps main query clean|

---

### 🏁 **18. Final Wisdom**

> “`GROUP BY` gives you the _big picture_,  
> `HAVING` filters the noise,  
> and `ROLLUP` tells the full story.” 📊