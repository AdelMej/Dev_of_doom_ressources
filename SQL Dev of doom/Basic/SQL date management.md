## 🗓️ **SQL Date & Time Management Cheat Sheet**

### ⏰ **1. Common Date/Time Data Types**

|Type|Description|
|---|---|
|`DATE`|Stores only date (`YYYY-MM-DD`)|
|`TIME`|Stores only time (`HH:MM:SS`)|
|`DATETIME`|Date and time (`YYYY-MM-DD HH:MM:SS`)|
|`TIMESTAMP`|Date/time with timezone or auto-tracking|
|`INTERVAL` (PostgreSQL)|Duration (e.g., `INTERVAL '5 days'`)|

---

### 📥 **2. Inserting Dates and Times**

```sql
INSERT INTO events (name, event_date)
VALUES ('Conference', '2025-11-15');

-- With timestamp
INSERT INTO logs (message, created_at)
VALUES ('System started', CURRENT_TIMESTAMP);
```

**Common functions for current time:**

|Function|Description|
|---|---|
|`CURRENT_DATE`|Current date|
|`CURRENT_TIME`|Current time|
|`CURRENT_TIMESTAMP` or `NOW()`|Current date and time|
|`LOCALTIMESTAMP`|Current date/time (no timezone)|

---

### 🧮 **3. Extracting Parts of Dates**

```sql
-- PostgreSQL / MySQL 8+
SELECT
  EXTRACT(YEAR FROM order_date) AS year,
  EXTRACT(MONTH FROM order_date) AS month,
  EXTRACT(DAY FROM order_date) AS day
FROM orders;

-- Alternative MySQL syntax
SELECT
  YEAR(order_date) AS year,
  MONTH(order_date) AS month,
  DAY(order_date) AS day
FROM orders;
```

---

### 🧠 **4. Formatting Dates**

```sql
-- PostgreSQL
SELECT TO_CHAR(order_date, 'YYYY-MM-DD HH24:MI:SS') AS formatted_date FROM orders;

-- MySQL
SELECT DATE_FORMAT(order_date, '%Y-%m-%d %H:%i:%s') AS formatted_date FROM orders;
```

**Common MySQL format patterns:**

|Pattern|Output|
|---|---|
|`%Y`|4-digit year|
|`%m`|2-digit month|
|`%d`|2-digit day|
|`%H`|24-hour|
|`%i`|Minutes|
|`%s`|Seconds|

---

### ⏳ **5. Date Arithmetic**

```sql
-- Add or subtract days
SELECT order_date + INTERVAL '7 days' FROM orders;          -- PostgreSQL
SELECT DATE_ADD(order_date, INTERVAL 7 DAY) FROM orders;    -- MySQL
SELECT DATE_SUB(order_date, INTERVAL 1 MONTH) FROM orders;  -- MySQL

-- Calculate difference between two dates
SELECT AGE(NOW(), birth_date) FROM users;                   -- PostgreSQL
SELECT DATEDIFF(NOW(), birth_date) FROM users;              -- MySQL
```

---

### 📊 **6. Filtering with Dates**

```sql
-- Exact match
SELECT * FROM events WHERE event_date = '2025-11-15';

-- Range
SELECT * FROM events
WHERE event_date BETWEEN '2025-10-01' AND '2025-12-31';

-- Recent N days
SELECT * FROM logs
WHERE created_at >= CURRENT_DATE - INTERVAL '7 days';        -- PostgreSQL
SELECT * FROM logs
WHERE created_at >= NOW() - INTERVAL 7 DAY;                  -- MySQL
```

---

### 🧩 **7. Useful Examples**

```sql
-- Users registered this year
SELECT * FROM users
WHERE EXTRACT(YEAR FROM registration_date) = EXTRACT(YEAR FROM CURRENT_DATE);

-- Events happening next month
SELECT * FROM events
WHERE event_date >= DATE_TRUNC('month', CURRENT_DATE + INTERVAL '1 month')
  AND event_date <  DATE_TRUNC('month', CURRENT_DATE + INTERVAL '2 month');

-- Convert string to date
SELECT TO_DATE('2025-12-25', 'YYYY-MM-DD');                  -- PostgreSQL
SELECT STR_TO_DATE('25/12/2025', '%d/%m/%Y');                -- MySQL
```