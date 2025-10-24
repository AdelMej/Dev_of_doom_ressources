## 🧬 **God-Tier SQL — CTEs & Recursive Queries Cheat Sheet**

---

### ⚙️ **1. What’s a CTE (Common Table Expression)?**

> A CTE is a **temporary, named result set** defined within a query.

Syntax:

```sql
WITH cte_name AS (
  SELECT ...
)
SELECT ...
FROM cte_name;
```

✅ Think of it like defining a variable or temporary view inside a single query.  
✅ Improves readability and allows multi-step logic.

---

### 💡 **2. Basic CTE Example**

```sql
WITH high_value_orders AS (
  SELECT * FROM orders WHERE total > 1000
)
SELECT customer_id, COUNT(*) AS num_high_value_orders
FROM high_value_orders
GROUP BY customer_id;
```

✅ Step 1: Select large orders.  
✅ Step 2: Aggregate by customer.

No subquery nesting — clean and readable.

---

### 🧱 **3. Multiple CTEs**

```sql
WITH
  active_users AS (
    SELECT id, username FROM users WHERE active = TRUE
  ),
  user_orders AS (
    SELECT user_id, SUM(total) AS total_spent FROM orders GROUP BY user_id
  )
SELECT a.username, uo.total_spent
FROM active_users a
JOIN user_orders uo ON a.id = uo.user_id;
```

✅ Chain multiple logic blocks together.  
✅ Makes complex analytics queries _elegant_.

---

### ⚙️ **4. CTE vs Subquery**

|Feature|CTE|Subquery|
|---|---|---|
|Readability|✅ Clean & reusable|❌ Can get messy|
|Performance|⚖️ Usually same as subquery|⚖️ Same|
|Reusability|✅ Reused multiple times|❌ Defined once|
|Recursion|✅ Supported|❌ Not supported|

---

### 🔁 **5. Recursive CTEs — The Real Magic**

> Recursive CTEs allow SQL to **loop** through hierarchical or sequential data.

Syntax pattern:

```sql
WITH RECURSIVE cte_name AS (
  -- Anchor member (base query)
  SELECT ... 

  UNION ALL

  -- Recursive member (refers to itself)
  SELECT ...
  FROM cte_name
  WHERE ...
)
SELECT * FROM cte_name;
```

---

### 🌳 **6. Example: Employee Hierarchy**

Imagine a table:

```sql
employees(id, name, manager_id)
```

Get the full management chain:

```sql
WITH RECURSIVE hierarchy AS (
  -- Base case: top managers
  SELECT id, name, manager_id, 1 AS level
  FROM employees
  WHERE manager_id IS NULL

  UNION ALL

  -- Recursive step: employees managed by previous level
  SELECT e.id, e.name, e.manager_id, h.level + 1
  FROM employees e
  JOIN hierarchy h ON e.manager_id = h.id
)
SELECT * FROM hierarchy ORDER BY level;
```

✅ Builds an **organizational tree** from top → bottom.  
💡 Can go infinitely deep until no more matches found.

---

### 🪜 **7. Example: Folder or Category Tree**

```sql
WITH RECURSIVE folder_tree AS (
  SELECT id, name, parent_id, 1 AS depth
  FROM folders
  WHERE parent_id IS NULL

  UNION ALL

  SELECT f.id, f.name, f.parent_id, ft.depth + 1
  FROM folders f
  JOIN folder_tree ft ON f.parent_id = ft.id
)
SELECT * FROM folder_tree;
```

✅ Perfect for nested structures (menus, folders, categories).

---

### 📅 **8. Example: Generate a Sequence of Dates**

```sql
WITH RECURSIVE dates AS (
  SELECT DATE '2025-01-01' AS day
  UNION ALL
  SELECT day + INTERVAL '1 day'
  FROM dates
  WHERE day < '2025-01-10'
)
SELECT * FROM dates;
```

✅ Creates a date range without needing a table.  
💡 Handy for filling missing time periods in reports.

---

### ⚙️ **9. Stopping Recursion**

Always include a **termination condition** in the recursive step —  
otherwise, it’ll loop infinitely.

✅ Example:

```sql
WHERE depth < 5
```

or

```sql
WHERE parent_id IS NOT NULL
```

---

### 🔢 **10. Detecting Cycles (Avoid Infinite Loops)**

Some databases (like PostgreSQL) support:

```sql
WITH RECURSIVE cte(...) AS (
  ...
) SEARCH DEPTH FIRST BY id SET ordercol
CYCLE id SET is_cycle TO TRUE DEFAULT FALSE
```

✅ Prevents infinite loops in cyclic data structures.

---

### 🧠 **11. Recursive CTE Use Cases**

|Use Case|Description|
|---|---|
|Hierarchies|Employees, categories, dependencies|
|Graph traversal|Find paths between nodes|
|Sequence generation|Dates, numbers|
|Path reconstruction|Build file paths, org charts|
|Summing nested data|Roll up costs, totals|

---

### 💥 **12. CTE Chaining Trick**

You can use **non-recursive + recursive** CTEs together.

```sql
WITH
  base_sales AS (
    SELECT id, amount FROM sales WHERE date >= '2025-01-01'
  ),
  cumulative AS (
    SELECT id, amount,
           SUM(amount) OVER (ORDER BY id) AS running_total
    FROM base_sales
  )
SELECT * FROM cumulative;
```

✅ Combines filtering + window functions inside modular logic.

---

### ⚡ **13. Performance Tips**

|Tip|Why|
|---|---|
|Index recursive join columns (`id`, `parent_id`)|Prevents slow growth|
|Use `UNION ALL` (not `UNION`)|Avoids unnecessary deduplication|
|Limit recursion depth|Prevent runaway loops|
|Materialize large CTEs|Avoids recalculating|
|Debug step-by-step|Test base and recursive queries separately|

---

### 🏁 **14. Final Wisdom**

> “CTEs make queries **modular**,  
> recursion makes them **intelligent**.” 🧬

They’re how you:

- Build trees, graphs, and sequences
- Simplify giant multi-join subqueries
- Make SQL logic _read like pseudocode_