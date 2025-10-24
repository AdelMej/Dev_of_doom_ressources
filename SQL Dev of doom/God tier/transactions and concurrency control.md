## 💀 **God-Tier SQL — Transactions & Concurrency Control Cheat Sheet**

---

### ⚙️ **1. What’s a Transaction?**

A **transaction** is a group of SQL operations executed as a single logical unit.  
They must follow the **ACID** principles:

|Property|Meaning|
|---|---|
|**A**tomicity|All statements succeed or none do.|
|**C**onsistency|Data moves from one valid state to another.|
|**I**solation|Transactions don’t interfere with each other.|
|**D**urability|Once committed, data is permanent (even after crash).|

---

### 🔄 **2. Transaction Basics**

```sql
BEGIN;                     -- start transaction
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;                    -- save changes
```

If something goes wrong:

```sql
ROLLBACK;                  -- undo everything since BEGIN
```

✅ Use transactions for:

- Money transfers
- Inventory updates
- Complex multi-table operations

---

### 🧱 **3. Implicit vs Explicit Transactions**

- **Implicit:** Each statement auto-commits by default.
    
	```sql
	INSERT INTO users VALUES (...);  -- auto-committed
	```
    
- **Explicit:** Manual control for critical operations.
    
```sql
BEGIN;
... multiple statements ...
COMMIT;
```
    

---

### 🧩 **4. Savepoints (Partial Rollback)**

> Roll back to a specific point inside a transaction.

```sql
BEGIN;
INSERT INTO logs VALUES ('step1');
SAVEPOINT sp1;

INSERT INTO logs VALUES ('step2');
ROLLBACK TO sp1;  -- undo 'step2', keep 'step1'
COMMIT;
```

✅ Great for partial recovery during complex operations.

---

### 🧠 **5. Isolation Levels**

> Controls how concurrent transactions see each other’s data.

|Level|Dirty Read|Non-Repeatable Read|Phantom Read|Notes|
|---|---|---|---|---|
|**READ UNCOMMITTED**|✅|✅|✅|Fast but unsafe|
|**READ COMMITTED**|❌|✅|✅|Default in PostgreSQL|
|**REPEATABLE READ**|❌|❌|✅|Prevents inconsistent reads|
|**SERIALIZABLE**|❌|❌|❌|Full isolation, slower|

**Example:**

```sql
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN;
-- operations
COMMIT;
```
---

### 🔄 **6. Concurrency Problems**

|Problem|Description|Example|
|---|---|---|
|**Dirty Read**|Read uncommitted data|A reads data B hasn’t committed yet|
|**Non-Repeatable Read**|Data changes between reads|A reads same row twice, sees different values|
|**Phantom Read**|New rows appear/disappear|A’s range query changes after B inserts rows|
|**Lost Update**|Concurrent updates overwrite each other|Two users update same row simultaneously|

---

### 🛡️ **7. Concurrency Control Techniques**

#### 🔹 Optimistic Locking

> Assume no conflict; detect on update.

```sql
ALTER TABLE users ADD COLUMN version INT DEFAULT 1;

UPDATE users
SET name='John', version=version+1
WHERE id=5 AND version=1;
```

If 0 rows updated → another transaction modified it.

---

#### 🔹 Pessimistic Locking

> Lock rows to prevent conflicts.

```sql
BEGIN;
SELECT * FROM accounts WHERE id=1 FOR UPDATE;
-- locks this row until COMMIT or ROLLBACK
UPDATE accounts SET balance = balance - 100 WHERE id=1;
COMMIT;
```

✅ Safe for financial operations.  
⚠️ Can cause deadlocks if used carelessly.

---

### ⚔️ **8. Deadlocks**

> Two transactions wait on each other forever.

**Example:**

- TX1 locks row A, waits for B
- TX2 locks row B, waits for A

💀 **Avoid by:**

- Always lock rows in the same order.
- Keep transactions short.
- Use `NOWAIT` or `SKIP LOCKED`:

```sql
SELECT * FROM tasks
WHERE status = 'pending'
FOR UPDATE SKIP LOCKED;
```

→ Skips locked rows instead of waiting.

---

### 🔍 **9. Monitoring Transactions**

**PostgreSQL:**

```sql
SELECT * FROM pg_stat_activity WHERE state = 'active';
SELECT * FROM pg_locks;
```

**MySQL:**

```sql
SHOW ENGINE INNODB STATUS;
SHOW PROCESSLIST;
```

---

### 🧮 **10. Performance Tips**

|Tip|Why|
|---|---|
|Keep transactions short|Reduce lock contention|
|Lock smallest possible scope|Avoid full-table locks|
|Use `READ COMMITTED` for speed|Most balanced isolation|
|Avoid long idle transactions|They block vacuum/cleanup|
|Use batching for inserts|Fewer commits = faster|

---

### 💡 **11. Transaction Patterns**

#### ✅ Safe Money Transfer

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

#### 🚫 Unsafe Version (No Transaction)

```sql
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
-- Crash here → inconsistent data!
```

---

### 🧙‍♂️ **12. Final Wisdom**

> “Transactions are not about speed — they’re about **safety under chaos**.”

Use the **lowest isolation level that keeps data correct**, and always know **what you’re locking**.