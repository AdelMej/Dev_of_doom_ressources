## 🔍 **SQL Query Cheat Sheet**

### 📘 **1. Basic Selection**

```sql
-- Select specific columns
SELECT column1, column2 FROM table_name;

-- Select all columns
SELECT * FROM table_name;

-- Rename columns (alias)
SELECT column_name AS alias_name FROM table_name;

-- Remove duplicate values
SELECT DISTINCT column_name FROM table_name;
```

---

### 🧮 **2. Filtering Data (WHERE Clause)**

```sql
-- Filter rows based on a condition
SELECT * FROM employees
WHERE department = 'IT';

-- Comparison operators
=   <>   >   <   >=   <=

-- Logical operators
AND   OR   NOT

-- Combine conditions
SELECT * FROM employees
WHERE department = 'IT' AND salary > 50000;
```

---

### 🔎 **3. Pattern Matching**

```sql
-- Partial text matching with LIKE
SELECT * FROM users WHERE name LIKE 'A%';   -- starts with A
SELECT * FROM users WHERE name LIKE '%son'; -- ends with 'son'
SELECT * FROM users WHERE name LIKE '%ann%';-- contains 'ann'

-- Case-insensitive (PostgreSQL)
SELECT * FROM users WHERE name ILIKE '%ann%';
```

---

### 📊 **4. Range & List Filters**

```sql
-- Range
SELECT * FROM products WHERE price BETWEEN 100 AND 500;

-- List
SELECT * FROM products WHERE category IN ('Electronics', 'Books');

-- Null checks
SELECT * FROM customers WHERE phone IS NULL;
SELECT * FROM customers WHERE phone IS NOT NULL;
```

---

### 🧱 **5. Sorting & Limiting**

```sql
-- Sort ascending / descending
SELECT * FROM products ORDER BY price ASC;
SELECT * FROM products ORDER BY price DESC;

-- Sort by multiple columns
SELECT * FROM products ORDER BY category, price DESC;

-- Limit number of rows
SELECT * FROM users LIMIT 10;

-- Skip and limit (pagination)
SELECT * FROM users OFFSET 20 LIMIT 10;
```

---

### 📈 **6. Aggregation & Grouping**

```sql
-- Aggregate functions
SELECT COUNT(*), AVG(price), MAX(price), MIN(price), SUM(price)
FROM products;

-- Group results
SELECT category, COUNT(*) FROM products
GROUP BY category;

-- Filter grouped data
SELECT category, AVG(price)
FROM products
GROUP BY category
HAVING AVG(price) > 50;
```

---

### 🔗 **7. Joins**

```sql
-- Inner join (only matching)
SELECT * FROM orders
INNER JOIN customers ON orders.customer_id = customers.id;

-- Left join (all from left + matches)
SELECT * FROM orders
LEFT JOIN customers ON orders.customer_id = customers.id;

-- Right join (all from right + matches)
SELECT * FROM orders
RIGHT JOIN customers ON orders.customer_id = customers.id;

-- Full join (all records, matched or not)
SELECT * FROM orders
FULL OUTER JOIN customers ON orders.customer_id = customers.id;
```

---

### 🧩 **8. Subqueries**

```sql
-- Subquery in WHERE
SELECT name FROM employees
WHERE department_id = (
  SELECT id FROM departments WHERE name = 'Finance'
);

-- Subquery in FROM
SELECT AVG(avg_salary)
FROM (
  SELECT department_id, AVG(salary) AS avg_salary
  FROM employees
  GROUP BY department_id
) AS dept_avg;
```
---

### 🧠 **9. Conditional Logic**

```sql
SELECT name,
  CASE
    WHEN salary > 100000 THEN 'High'
    WHEN salary BETWEEN 50000 AND 100000 THEN 'Medium'
    ELSE 'Low'
  END AS salary_level
FROM employees;
```