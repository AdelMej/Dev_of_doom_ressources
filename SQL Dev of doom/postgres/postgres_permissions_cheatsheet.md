# PostgreSQL Permissions Cheat Sheet

## Core Concepts
PostgreSQL permissions are managed with **roles**. A role can act as:
- A **user** (LOGIN)
- A **group** (NOLOGIN)

Everything is explicit. No permission is granted by default except ownership.

---

## Roles

### Create Roles
```sql
CREATE ROLE app_user LOGIN PASSWORD 'secret';
CREATE ROLE app_readonly NOLOGIN;
```

### Alter Roles
```sql
ALTER ROLE app_user CREATEDB;
ALTER ROLE app_user NOINHERIT;
```

### Drop Roles
```sql
DROP ROLE app_user;
```

---

## Role Membership (RBAC)

```sql
GRANT app_readonly TO app_user;
REVOKE app_readonly FROM app_user;
```

- WITH INHERIT → permissions are automatic
- NOINHERIT → must `SET ROLE`

```sql
SET ROLE app_readonly;
RESET ROLE;
```

---

## Database Permissions

```sql
GRANT CONNECT ON DATABASE app_db TO app_user;
REVOKE CONNECT ON DATABASE app_db FROM app_user;
```

---

## Schema Permissions

```sql
GRANT USAGE ON SCHEMA public TO app_user;
GRANT CREATE ON SCHEMA public TO app_user;
```

- USAGE = access objects
- CREATE = create tables, views, etc.

---

## Table Permissions

```sql
GRANT SELECT, INSERT, UPDATE ON TABLE users TO app_user;
REVOKE DELETE ON TABLE users FROM app_user;
```

Common privileges:
- SELECT
- INSERT
- UPDATE
- DELETE
- TRUNCATE
- REFERENCES
- TRIGGER

---

## Column-Level Permissions

```sql
GRANT UPDATE(email) ON TABLE users TO app_user;
```

Yes, Postgres actually supports this 😎

---

## All Tables / Future Tables

### Existing tables
```sql
GRANT SELECT ON ALL TABLES IN SCHEMA public TO app_readonly;
```

### Future tables (IMPORTANT)
```sql
ALTER DEFAULT PRIVILEGES
IN SCHEMA public
GRANT SELECT ON TABLES TO app_readonly;
```

⚠️ Default privileges are **role-scoped**

---

## Sequences

```sql
GRANT USAGE, SELECT ON SEQUENCE users_id_seq TO app_user;
```

If you forget this → inserts will fail.

---

## Functions & Procedures

```sql
GRANT EXECUTE ON FUNCTION create_user(text) TO app_user;
```

---

## Views

Views use **invoker rights** by default.

```sql
GRANT SELECT ON user_view TO app_user;
```

For security:
```sql
CREATE VIEW secure_view
SECURITY DEFINER
AS SELECT ...
```

---

## Ownership

Owners bypass permission checks.

```sql
ALTER TABLE users OWNER TO app_admin;
```

Never let app roles own tables.

---

## REVOKE EVERYTHING (Lockdown)

```sql
REVOKE ALL ON DATABASE app_db FROM PUBLIC;
REVOKE ALL ON SCHEMA public FROM PUBLIC;
```

PUBLIC = everyone (including future roles)

---

## Inspect Permissions

```sql
\dp table_name
\du
\l
```

Or SQL:
```sql
SELECT * FROM information_schema.role_table_grants;
```

---

## Best Practices (Battle-Tested)

- One **owner role**
- One **migration role**
- One **app runtime role**
- No SUPERUSER for apps
- Combine with **Row Level Security**
- Lock PUBLIC immediately

---

## Mental Model

> Authentication = who you are  
> Authorization = what SQL lets you do  

Postgres enforces authz **inside the database**.  
If it’s denied here, it’s denied forever.

---

## Common Footguns

❌ Forgot sequence permissions  
❌ Forgot ALTER DEFAULT PRIVILEGES  
❌ App role owns tables  
❌ PUBLIC still has access  

---

## Recommended Pairings
- Permissions + RLS
- Permissions + Views
- Permissions + SECURITY DEFINER functions
