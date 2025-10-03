# 🛠️ SQL Functions Cheat Sheet
## 🔹 String Functions
```SQL
-- Length of string
LENGTH('Hello');              -- 5
CHAR_LENGTH('Hello');         -- 5

-- Concatenate
CONCAT('Hello', ' ', 'SQL');  -- Hello SQL

-- Substring
SUBSTR('Database', 1, 4);     -- Data
SUBSTRING('Database', 5, 3);  -- bas

-- Upper / Lower case
UPPER('sql');   -- SQL
LOWER('SQL');   -- sql

-- Trim spaces
TRIM('   hi  ');      -- hi
LTRIM('   hi');       -- hi
RTRIM('hi   ');       -- hi

-- Replace
REPLACE('abcabc', 'a', 'x');  -- xbcxbc

-- Position of substring
POSITION('a' IN 'data');      -- 2
```

## 🔹 Numeric Functions
```SQL
-- Rounding
ROUND(123.456, 2);   -- 123.46
CEIL(4.2);           -- 5
FLOOR(4.9);          -- 4
TRUNC(123.456, 1);   -- 123.4 (Postgres/Oracle)

-- Absolute value
ABS(-42);            -- 42

-- Power & roots
POWER(2, 3);         -- 8
SQRT(16);            -- 4

-- Modulo / remainder
MOD(10, 3);          -- 1

🔹 Date & Time Functions
-- Current date/time
CURRENT_DATE;        -- 2025-10-03
CURRENT_TIME;
NOW();               -- full timestamp

-- Extract parts
EXTRACT(YEAR FROM NOW());   -- 2025
EXTRACT(MONTH FROM NOW());  -- 10
EXTRACT(DAY FROM NOW());    -- 3

-- Add / subtract intervals
DATEADD(DAY, 7, '2025-10-03');   -- SQL Server
NOW() + INTERVAL '7 days';       -- Postgres

-- Difference
DATEDIFF(DAY, '2025-10-01', '2025-10-03');  -- SQL Server
AGE(NOW(), '2020-01-01');                  -- Postgres
```

## 🔹 Aggregate Functions
```SQL
COUNT(*)               -- count rows
COUNT(DISTINCT col)    -- unique values
SUM(col)               -- sum
AVG(col)               -- average
MIN(col)               -- smallest
MAX(col)               -- largest
```

## 🔹 Conditional Functions
```SQL
-- Case expression
CASE 
  WHEN score >= 50 THEN 'Pass'
  ELSE 'Fail'
END

-- Null handling
COALESCE(col, 'default');    -- returns first non-null
NULLIF(a, b);                -- null if a = b, else a

🔹 Conversion Functions
CAST('123' AS INT);          -- convert string to number
CAST(123.45 AS VARCHAR);     -- number to string

-- Postgres shorthand
'2025-10-03'::DATE
```

## 🔹 Window Functions (Analytics)
```SQL
-- Row numbers
ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC);

-- Rank with ties
RANK() OVER (ORDER BY score DESC);

-- Running totals / moving averages
SUM(sales) OVER (ORDER BY date);
AVG(score) OVER (PARTITION BY class);
```

## 🔹 JSON Functions (Postgres)
```SQL
-- Get JSON field
SELECT data->'name' FROM users;
SELECT data->>'name' FROM users;  -- text value

-- Build JSON
SELECT JSON_BUILD_OBJECT('id', id, 'name', name);
```
