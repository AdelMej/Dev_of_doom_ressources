# 🗃️ SQL Cheat Sheet

## 🔹 Basics
```SQL
-- Select all columns 
SELECT * FROM table_name;  
-- Select specific columns 
SELECT column1, column2 FROM table_name;  
-- Rename column (alias) 
SELECT column1 AS new_name FROM table_name;
```

---

## 🔹 Filtering Data

```SQL
-- Conditions 
SELECT * FROM table_name WHERE column = 'value'; 
SELECT * FROM table_name WHERE column > 10;  

-- Multiple conditions 
SELECT * FROM table_name WHERE column1 = 'value' AND column2 < 100; 
SELECT * FROM table_name WHERE column1 = 'value' OR column2 IS NULL;  

-- Pattern matching 
SELECT * FROM table_name WHERE column LIKE 'A%';   -- starts with A 
SELECT * FROM table_name WHERE column LIKE '%A';   -- ends with A 
SELECT * FROM table_name WHERE column LIKE '%A%';  -- contains A  

-- In list 
SELECT * FROM table_name WHERE column IN ('a', 'b', 'c');
```


---

## 🔹 Sorting & Limiting

```SQL
SELECT * FROM table_name ORDER BY column ASC;   -- ascending 
SELECT * FROM table_name ORDER BY column DESC;  -- descending  

-- Limit results 
SELECT * FROM table_name LIMIT 10;
```

---

## 🔹 Aggregates

```SQL
SELECT COUNT(*) FROM table_name;             -- count rows 
SELECT AVG(column) FROM table_name;          -- average 
SELECT SUM(column) FROM table_name;          -- sum 
SELECT MIN(column), MAX(column) FROM table;  -- min/max  

-- Group results 
SELECT column, COUNT(*)  
FROM table_name 
GROUP BY column;  

-- Filter grouped results 
SELECT column, COUNT(*)  
FROM table_name 
GROUP BY column 
HAVING COUNT(*) > 1;
```

---

## 🔹 Joins

```SQL
-- Inner Join (only matching rows) 
SELECT a.col, b.col 
FROM tableA a 
INNER JOIN tableB b ON a.id = b.id;  

-- Left Join (all from left, matching from right) 
SELECT a.col, b.col 
FROM tableA a 
LEFT JOIN tableB b ON a.id = b.id;  

-- Right Join (all from right, matching from left) 
SELECT a.col, b.col 
FROM tableA a 
RIGHT JOIN tableB b ON a.id = b.id;  

-- Full Join (everything, match if possible) 
SELECT a.col, b.col 
FROM tableA a 
FULL OUTER JOIN tableB b ON a.id = b.id;
```


---

## 🔹 Modifying Data

```SQL
-- Insert 
INSERT INTO table_name (col1, col2) VALUES ('val1', 'val2');  

-- Update 
UPDATE table_name SET column = 'new_value' WHERE id = 1;  

-- Delete 
DELETE FROM table_name WHERE id = 1;
```

---

## 🔹 Table Management

```SQL
-- Create table
CREATE TABLE users (
	id INT PRIMARY KEY,
	name VARCHAR(100),
	age INT
);  

-- Alter table 
ALTER TABLE users ADD email VARCHAR(100);  

-- Drop table 
DROP TABLE users;
```