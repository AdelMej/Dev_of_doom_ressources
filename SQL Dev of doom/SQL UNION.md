# 📝 SQL UNION Cheat Sheet

### 1. **Basic UNION**

Combine results from two queries, removing duplicates:

```sql
SELECT column1, column2 FROM table1 
UNION 
SELECT column1, column2 FROM table2;
```
- `UNION` = **distinct only** (no duplicates).

---

### 2. **UNION ALL**

Keeps duplicates (faster since no deduplication):

```SQL
SELECT column1 FROM table1 
UNION ALL 
SELECT column1 FROM table2;`
```

---

### 3. **Column Rules**

- Both queries must have the **same number of columns**.
- Columns must have **compatible data types**.
- Column names in the final result are taken from the **first SELECT**.

---

### 4. **ORDER BY with UNION**

Put `ORDER BY` only **at the very end**:

```SQL
SELECT id FROM Employees 
UNION 
SELECT id FROM EmployeeUNI 
ORDER BY id;
```

---

### 5. **Using DISTINCT / ALL Inside**

- `UNION` already removes duplicates, so `DISTINCT` inside is redundant.
- `UNION ALL` ignores uniqueness, but you can still filter manually:

 ```SQL
 SELECT DISTINCT id FROM Employees 
 UNION ALL 
 SELECT DISTINCT id FROM EmployeeUNI;
 ```

---

### 6. **Mixing Constants**

You can add literals/columns to align queries:

```SQL
SELECT id, 'Employees' AS source FROM Employees 
UNION 
SELECT id, 'EmployeeUNI' AS source FROM EmployeeUNI;
```

👉 This way you know **which table the row came from**.

---

### 7. **Common Uses**

- Combine logs from multiple tables
- Merge old & new tables during migration
- Collect unique values from different sources

---

⚡Quick mental rule:

- Need **unique results**? → `UNION`
- Need **all results** (with duplicates)? → `UNION ALL`