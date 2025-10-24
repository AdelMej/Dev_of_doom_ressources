## 🧩 **Advanced SQL — Views Cheat Sheet**

---

### 📘 **1. What Is a View?**

A **view** is a **virtual table** that stores a query instead of data.  
When you query the view, the database runs the underlying query in real time.

✅ **Use views to:**

- Simplify complex joins
- Reuse logic across multiple queries
- Restrict access to sensitive columns
- Create cleaner APIs for your database

---

### 🏗️ **2. Creating a View**

```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

**Example:**

```sql
CREATE VIEW active_users AS
SELECT id, username, email
FROM users
WHERE status = 'active';
```
Now you can query it like a regular table:

```sql
SELECT * FROM active_users;
```

---

### 🔍 **3. Viewing or Describing a View**

```sql
-- PostgreSQL
\d+ view_name;

-- MySQL
SHOW CREATE VIEW view_name;
DESCRIBE view_name;
```

---

### ✏️ **4. Updating a View**

Some views are **updatable**, meaning you can use `INSERT`, `UPDATE`, or `DELETE` directly on them — if:

- The view is based on a single table
- It doesn’t use `GROUP BY`, `DISTINCT`, `JOIN`, or aggregates

```sql
UPDATE active_users
SET status = 'inactive'
WHERE id = 1;
```

---

### 🧱 **5. Creating Read-Only Views**

```sql
CREATE VIEW employee_public AS
SELECT id, name, department
FROM employees
WITH CHECK OPTION;
```

`WITH CHECK OPTION` ensures that updates through the view still respect the `WHERE` condition.  
If a user tries to update a row that would violate the filter, it’s rejected.

---

### 🧩 **6. Parameter-Like Views (Using Functions)**

SQL views don’t accept parameters directly — but you can simulate them using **functions** (PostgreSQL example):

```sql
CREATE OR REPLACE FUNCTION get_orders_for_customer(cust_id INT)
RETURNS TABLE(order_id INT, total NUMERIC) AS $$
BEGIN
  RETURN QUERY
  SELECT id, total FROM orders WHERE customer_id = cust_id;
END;
$$ LANGUAGE plpgsql;
```
Then call it like:

```sql
SELECT * FROM get_orders_for_customer(42);
```

---

### ⚙️ **7. Updating a View Definition**

```sql
CREATE OR REPLACE VIEW view_name AS
SELECT ...;
```

---

### ❌ **8. Dropping a View**

```sql
DROP VIEW view_name;
DROP VIEW IF EXISTS view_name;
```

---

### 🧠 **9. Materialized Views (Performance Boost)**

> A **materialized view** stores query results physically — great for heavy analytics.

**PostgreSQL example:**

```sql
CREATE MATERIALIZED VIEW sales_summary AS
SELECT
  region,
  SUM(amount) AS total_sales,
  COUNT(*) AS orders_count
FROM sales
GROUP BY region;
```

**Refresh when data changes:**

```sql
REFRESH MATERIALIZED VIEW sales_summary;
```

**Query it:**

```sql
SELECT * FROM sales_summary;
```

✅ **Benefits:** Fast reads, precomputed data  
⚠️ **Downside:** Needs manual refresh (can become outdated)

---

### 💡 **10. Best Practices**

|Tip|Why|
|---|---|
|Use views for reusable logic|Keeps queries clean|
|Use materialized views for reports|Huge performance boost|
|Name convention: `v_` or `mv_` prefix|Example: `v_users`, `mv_sales_summary`|
|Don’t nest too many views|Hard to maintain and debug|
|Always index base tables|Improves view performance|