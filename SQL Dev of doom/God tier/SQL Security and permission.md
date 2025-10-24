# 🔒 God-Tier SQL — Security & Permissions Cheat Sheet

This one is essential if you ever plan to manage multi-user databases, web apps, or servers.  
It’s about _who_ can do _what_ — and _how much_.

---

## ⚙️ **1. SQL Security Model Overview**

SQL security revolves around **Users**, **Roles**, and **Privileges**:

|Concept|Description|
|---|---|
|**User**|An account that connects to the database.|
|**Role**|A collection of permissions (can be assigned to users).|
|**Privilege**|A specific right (like `SELECT`, `INSERT`, `UPDATE`).|

✅ You control everything via `GRANT` and `REVOKE`.  
✅ You can separate database management from application access.

---

## 👤 **2. Creating and Managing Users**

### 🧱 Create a User

```sql
CREATE USER app_user WITH PASSWORD 'strongpassword';
```

### 🔒 Force password change at next login (PostgreSQL)

```sql
ALTER ROLE app_user PASSWORD 'temporary' VALID UNTIL '2025-11-01';
```

### 🧹 Drop a User

```sql
DROP USER app_user;
```

---

## 🧩 **3. Roles — Grouping Privileges**

### Create a Role

```sql
CREATE ROLE readonly;
```

### Grant Privileges to Role

```sql
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly;
```

### Assign Role to User

```sql
GRANT readonly TO app_user;
```
✅ Cleaner management: change one role → all users update automatically.

---

## 🧠 **4. Granting Privileges**

### 📘 Grant Basic Table Privileges

```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON employees TO app_user;
```

### 🔐 Grant Schema or Database Privileges

```sql
GRANT CONNECT ON DATABASE company_db TO app_user;
GRANT USAGE ON SCHEMA public TO app_user;
```

### 🧱 Grant Object Creation Privilege

```sql
GRANT CREATE ON SCHEMA public TO dev_user;
```

---

## ❌ **5. Revoking Privileges**

```sql
REVOKE INSERT, DELETE ON employees FROM app_user;
REVOKE ALL PRIVILEGES ON DATABASE company_db FROM app_user;
```

✅ Use this to strip access or enforce least-privilege policies.

---

## 🧠 **6. Understanding Privilege Types**

|Type|Example|Description|
|---|---|---|
|**Data privileges**|`SELECT`, `INSERT`, `UPDATE`, `DELETE`|Direct access to data|
|**Schema privileges**|`CREATE`, `USAGE`|Create/use tables, views, etc.|
|**Database privileges**|`CONNECT`, `TEMP`|Access databases or temp objects|
|**Admin privileges**|`SUPERUSER`, `CREATEDB`, `CREATEROLE`|System-level powers|

---

## 🧱 **7. Managing Default Privileges**

> Make sure new tables automatically get the right permissions.

```sql
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT SELECT ON TABLES TO readonly;
```

✅ Future tables inherit these permissions automatically.

---

## 🧩 **8. Viewing Permissions**

### PostgreSQL Example:

```sql
\du  -- list users and roles
\z   -- list privileges
```
Or via query:

```sql
SELECT grantee, privilege_type, table_name
FROM information_schema.role_table_grants;
```
---

## 🔐 **9. Role Inheritance**

Roles can **inherit** privileges from other roles.

```sql
CREATE ROLE dev_team INHERIT;
GRANT readonly TO dev_team;
GRANT dev_team TO alice;
```

✅ Alice now inherits everything from `readonly` through `dev_team`.

---

## 🧠 **10. Security Best Practices**

|Best Practice|Why|
|---|---|
|**Use roles, not individual grants**|Easier to manage|
|**Enforce least privilege**|Limit potential damage|
|**Avoid superuser for applications**|Prevent full database control|
|**Use separate users for each service**|Track activity easily|
|**Rotate passwords regularly**|Prevent stale credentials|
|**Use SSL connections**|Encrypt data in transit|
|**Audit logs**|Monitor suspicious activity|

---

## 🧩 **11. Example Setup for a Web App**

### 🏗️ Step 1: Create Roles

```sql
CREATE ROLE web_readonly;
CREATE ROLE web_admin;
```

### 🧱 Step 2: Assign Privileges

```sql
GRANT SELECT ON ALL TABLES IN SCHEMA public TO web_readonly;
GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO web_admin;
```

### 👤 Step 3: Create Users

```sql
CREATE USER api_user WITH PASSWORD 'securepass';
CREATE USER frontend_user WITH PASSWORD 'viewonly';
```

### 🔗 Step 4: Assign Roles

```sql
GRANT web_admin TO api_user;
GRANT web_readonly TO frontend_user;
```

✅ `api_user` can modify data.  
✅ `frontend_user` can only view it.

---

## ⚡ **12. Row-Level Security (RLS)** — _Fine-Grained Access Control_

> Restrict access to specific rows in a table based on the user.

```sql
ALTER TABLE orders ENABLE ROW LEVEL SECURITY;

CREATE POLICY user_orders
ON orders
FOR SELECT
USING (user_id = current_user::INT);
```

✅ Each user only sees their own orders.  
💡 Powerful for multi-tenant apps.

---

## 🧠 **13. Security Auditing**

You can log queries or changes:

```sql
ALTER DATABASE company_db SET log_statement = 'all';
```

Or use triggers to write to audit tables:

```sql
AFTER UPDATE OR DELETE ON sensitive_table
EXECUTE FUNCTION log_changes();
```
---

## 🏁 **14. Final Wisdom**

> “Optimization makes SQL fast.  
> Security makes SQL safe.” 🔒

With this, your database isn’t just efficient — it’s **bulletproof**.  
You can now:

- Control access per user, table, or row
- Audit sensitive actions    
- Build production-ready, secure systems