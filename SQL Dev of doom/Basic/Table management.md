	## 🧱 **SQL Table Management Cheat Sheet**

### 🧩 **1. Create Table**

```sql
CREATE TABLE table_name (
  id INT PRIMARY KEY,                -- unique identifier
  name VARCHAR(100) NOT NULL,        -- text with max length
  age INT CHECK (age >= 0),          -- must be non-negative
  email VARCHAR(255) UNIQUE,         -- no duplicates allowed
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Common Data Types:**

|Type|Description|
|---|---|
|`INT`|Whole number|
|`REAL` / `FLOAT`|Decimal number|
|`VARCHAR(n)`|Text up to n characters|
|`TEXT`|Long text|
|`DATE`|Date only|
|`TIMESTAMP`|Date and time|
|`BOOLEAN`|True/False|
|`SERIAL`|Auto-increment integer (PostgreSQL)|

---

### ✏️ **2. Alter Table (Modify Existing Table)**

```sql
-- Add a new column
ALTER TABLE table_name ADD COLUMN column_name data_type;

-- Add with default value
ALTER TABLE table_name ADD COLUMN status VARCHAR(20) DEFAULT 'active';

-- Modify a column (depends on SQL dialect)
ALTER TABLE table_name ALTER COLUMN column_name TYPE new_data_type;   -- PostgreSQL
ALTER TABLE table_name MODIFY column_name new_data_type;              -- MySQL

-- Rename a column
ALTER TABLE table_name RENAME COLUMN old_name TO new_name;

-- Rename the entire table
ALTER TABLE old_table_name RENAME TO new_table_name;

-- Drop (delete) a column
ALTER TABLE table_name DROP COLUMN column_name;
```

---

### 🔐 **3. Add / Remove Constraints**

```sql
-- Add a primary key
ALTER TABLE table_name ADD CONSTRAINT pk_name PRIMARY KEY (id);

-- Add a foreign key
ALTER TABLE orders
ADD CONSTRAINT fk_customer
FOREIGN KEY (customer_id) REFERENCES customers(id);

-- Drop a constraint
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```
---

### ❌ **4. Drop or Truncate Table**

```sql
-- Delete all data (keeps structure)
TRUNCATE TABLE table_name;

-- Delete the table completely
DROP TABLE table_name;

-- Drop only if it exists
DROP TABLE IF EXISTS table_name;
```

---

### 🧠 **5. Temporary & IF NOT EXISTS**

```sql
-- Create a temporary table (deleted after session)
CREATE TEMPORARY TABLE temp_data (
  id INT,
  value TEXT
);

-- Avoid error if table already exists
CREATE TABLE IF NOT EXISTS table_name (...);
```