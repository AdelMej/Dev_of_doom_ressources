# PostgreSQL Row Level Security (RLS) — Cheat Sheet

Row Level Security (RLS) allows PostgreSQL to **enforce access rules per-row**, directly inside the database.
It is evaluated **after SQL permissions** and **before data is returned**.

---

## 1. Enable RLS on a Table

```sql
ALTER TABLE my_table ENABLE ROW LEVEL SECURITY;
```

Optional (force RLS even for table owner):
```sql
ALTER TABLE my_table FORCE ROW LEVEL SECURITY;
```

---

## 2. Basic Policy Structure

```sql
CREATE POLICY policy_name
ON my_table
FOR SELECT | INSERT | UPDATE | DELETE
TO role_name
USING (condition)        -- for SELECT, UPDATE, DELETE
WITH CHECK (condition);  -- for INSERT, UPDATE
```

- `USING` → which rows are **visible**
- `WITH CHECK` → which rows are **allowed to be written**

---

## 3. Minimal Example (User-Owned Rows)

```sql
ALTER TABLE documents ENABLE ROW LEVEL SECURITY;

CREATE POLICY documents_owner_select
ON documents
FOR SELECT
USING (owner_id = current_setting('app.current_user_id')::uuid);
```

Application sets context:
```sql
SET app.current_user_id = 'uuid-here';
```

---

## 4. INSERT / UPDATE Example

```sql
CREATE POLICY documents_owner_write
ON documents
FOR INSERT, UPDATE
WITH CHECK (owner_id = current_setting('app.current_user_id')::uuid);
```

---

## 5. DELETE Example

```sql
CREATE POLICY documents_owner_delete
ON documents
FOR DELETE
USING (owner_id = current_setting('app.current_user_id')::uuid);
```

---

## 6. Admin / Bypass Policy

```sql
CREATE POLICY admin_full_access
ON documents
FOR ALL
TO admin
USING (true)
WITH CHECK (true);
```

Or bypass entirely:
```sql
ALTER ROLE admin BYPASSRLS;
```

---

## 7. Public vs Authenticated Access

Public read-only:
```sql
CREATE POLICY public_read
ON articles
FOR SELECT
TO public
USING (is_published = true);
```

Authenticated write:
```sql
CREATE POLICY user_write
ON articles
FOR INSERT, UPDATE
TO authenticated
WITH CHECK (author_id = current_setting('app.current_user_id')::uuid);
```

---

## 8. Multiple Policies (OR Logic)

Policies are combined using **OR**:
```sql
-- Row is accessible if ANY policy allows it
```

---

## 9. Using JWT / App Context (Best Practice)

Set once per request:
```sql
SET LOCAL app.current_user_id = 'uuid';
SET LOCAL app.role = 'user';
```

Use in policies:
```sql
USING (owner_id = current_setting('app.current_user_id')::uuid)
```

---

## 10. Testing RLS

Check policies:
```sql
\dp my_table
```

Simulate role:
```sql
SET ROLE user;
SELECT * FROM my_table;
RESET ROLE;
```

Disable temporarily (debug only):
```sql
ALTER TABLE my_table DISABLE ROW LEVEL SECURITY;
```

---

## 11. Common Pitfalls

❌ Forgetting `ENABLE ROW LEVEL SECURITY`  
❌ Table owner bypassing RLS unintentionally  
❌ Not setting `SET LOCAL` (context leaks between requests)  
❌ Assuming RLS replaces application auth (it complements it)

---

## 12. When to Use RLS

✅ Multi-tenant systems  
✅ Strong security invariants  
✅ Defense-in-depth  
❌ High-throughput analytics without per-user filtering

---

## 13. Mental Model

- SQL permissions = **can you touch the table**
- RLS = **which rows you can see**
- App authz = **who is allowed to try**

RLS is a **database invariant**, not business logic.

---

## 14. Recommended Pattern (Clean)

- App:
  - Authenticates user
  - Sets session variables
- DB:
  - Enforces row visibility
- Domain:
  - Still validates business rules

**Impossible states stay impossible.**
