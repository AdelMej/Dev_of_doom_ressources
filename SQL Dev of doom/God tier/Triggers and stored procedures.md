# 🧩 God-Tier SQL — Triggers & Stored Procedures Cheat Sheet

This is where your database **reacts intelligently** and **executes logic automatically**, just like backend code — but inside SQL itself.

---

## ⚙️ **1. What Are Triggers and Stored Procedures?**

|Feature|Purpose|
|---|---|
|**Stored Procedure**|A _named block of SQL code_ you can call anytime (like a function).|
|**Trigger**|Code that runs _automatically_ when a table is modified (`INSERT`, `UPDATE`, `DELETE`).|

✅ **Stored Procedures** = manual execution.  
✅ **Triggers** = automatic execution.

---

## 🧠 **2. Stored Procedures — Reusable Logic**

### 🧱 Create a Stored Procedure

**MySQL / PostgreSQL (plpgsql syntax):**

```sql
CREATE OR REPLACE PROCEDURE add_order(p_user_id INT, p_total NUMERIC)
LANGUAGE plpgsql
AS $$
BEGIN
  INSERT INTO orders (user_id, total, created_at)
  VALUES (p_user_id, p_total, NOW());
END;
$$;
```

✅ Wraps logic into one callable block.

---

### ▶️ **Call a Stored Procedure**

```sql
CALL add_order(12, 199.99);
```

✅ Executes like a function — cleaner than repeating SQL code.

---

### 🧩 **With Input and Output Parameters**

```sql
CREATE OR REPLACE PROCEDURE get_user_stats(
  IN p_user_id INT,
  OUT total_orders INT,
  OUT total_spent NUMERIC
)
LANGUAGE plpgsql
AS $$
BEGIN
  SELECT COUNT(*), COALESCE(SUM(total), 0)
  INTO total_orders, total_spent
  FROM orders
  WHERE user_id = p_user_id;
END;
$$;
```

Call it:

```sql
CALL get_user_stats(5, total_orders, total_spent);
```

---

### 🔁 **Control Flow Inside Procedures**

```sql
CREATE OR REPLACE PROCEDURE reward_user(p_user_id INT)
LANGUAGE plpgsql
AS $$
DECLARE
  order_count INT;
BEGIN
  SELECT COUNT(*) INTO order_count FROM orders WHERE user_id = p_user_id;

  IF order_count >= 10 THEN
    UPDATE users SET reward_points = reward_points + 100 WHERE id = p_user_id;
  END IF;
END;
$$;
```

✅ Supports variables, loops, IFs, and transactions — like mini-programs inside SQL.

---

### 💾 **Stored Functions vs Procedures**

|Feature|Function|Procedure|
|---|---|---|
|Returns a value|✅ Yes|❌ No|
|Called in queries|✅ Yes|❌ No|
|Used for logic only|⚖️ Sometimes|✅ Always|
|Transaction control|❌|✅ Yes (BEGIN/COMMIT)|

---

## ⚡ **3. Triggers — Automation on Data Change**

### 🧩 **Trigger Basics**

> A trigger automatically executes when a table changes.

Syntax:

```sql
CREATE TRIGGER trigger_name
AFTER INSERT OR UPDATE OR DELETE
ON table_name
FOR EACH ROW
EXECUTE FUNCTION function_name();
```

✅ Works for **INSERT**, **UPDATE**, and **DELETE** events.

---

### ⚙️ **Example: Log All Inserted Users**

First, make a function:

```sql
CREATE OR REPLACE FUNCTION log_user_insert()
RETURNS TRIGGER AS $$
BEGIN
  INSERT INTO user_logs(user_id, action, created_at)
  VALUES (NEW.id, 'INSERT', NOW());
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

Then attach it as a trigger:

```sql
CREATE TRIGGER user_insert_trigger
AFTER INSERT ON users
FOR EACH ROW
EXECUTE FUNCTION log_user_insert();
```

✅ Automatically logs every new user.

---

### 🔁 **BEFORE vs AFTER Triggers**

|Type|When It Runs|Use Case|
|---|---|---|
|**BEFORE**|Before row is modified|Validate or modify data|
|**AFTER**|After row is modified|Audit, sync, or notify|

Example (validation before insert):

```sql
CREATE OR REPLACE FUNCTION check_positive_total()
RETURNS TRIGGER AS $$
BEGIN
  IF NEW.total < 0 THEN
    RAISE EXCEPTION 'Order total cannot be negative';
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER validate_order
BEFORE INSERT ON orders
FOR EACH ROW
EXECUTE FUNCTION check_positive_total();
```

---

### 🔄 **Example: Auto-Update Timestamp**

```sql
CREATE OR REPLACE FUNCTION update_modified_at()
RETURNS TRIGGER AS $$
BEGIN
  NEW.updated_at := NOW();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER touch_updated_at
BEFORE UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION update_modified_at();
```
✅ Keeps `updated_at` always in sync automatically.

---

### 🧩 **Accessing OLD and NEW Values**

|Keyword|Description|
|---|---|
|`NEW`|The new row values (`INSERT`, `UPDATE`)|
|`OLD`|The old row values (`UPDATE`, `DELETE`)|

Example:

```sql
INSERT INTO change_log(user_id, old_email, new_email)
VALUES (OLD.id, OLD.email, NEW.email);
```
---

### ⚡ **4. Triggers with Multiple Events**

```sql
CREATE TRIGGER order_events
AFTER INSERT OR UPDATE OR DELETE ON orders
FOR EACH ROW
EXECUTE FUNCTION log_order_activity();
```

✅ One trigger, multiple actions.

---

### 🧠 **5. Disabling / Dropping Triggers**

```sql
ALTER TABLE users DISABLE TRIGGER user_insert_trigger;
DROP TRIGGER user_insert_trigger ON users;
```

---

### 🚀 **6. Performance & Safety Tips**

|Tip|Why|
|---|---|
|Keep triggers lightweight|They fire on every row|
|Avoid complex queries inside triggers|Can lock the table|
|Use AFTER triggers for logging|Prevents blocking the main transaction|
|Test trigger behavior carefully|Debugging can be tricky|
|Use stored procedures for heavy logic|Triggers should delegate work|

---

### 🧩 **7. Real-World Use Cases**

|Use Case|Solution|
|---|---|
|Audit trail|AFTER INSERT trigger → log table|
|Data validation|BEFORE INSERT trigger|
|Auto-maintenance|BEFORE UPDATE trigger → update timestamps|
|Synchronization|AFTER UPDATE trigger → propagate changes|
|Soft deletes|BEFORE DELETE trigger → set `deleted_at` timestamp|

---

### 🏁 **8. Final Wisdom**

> “Procedures make your database _think_;  
> Triggers make it _react_.” ⚙️

Together, they turn SQL into an **event-driven system** —  
capable of enforcing rules, automating workflows, and reducing backend overhead.