# PostgreSQL Views Cheat Sheet

## What is a View?
A **view** is a stored SQL query that behaves like a table.
It does **not store data** (unless materialized).

```sql
CREATE VIEW app.active_users AS
SELECT id, email
FROM app.users
WHERE disabled_at IS NULL;
```

---

## Why Use Views?
- Encapsulate complex queries
- Enforce **read-only access**
- Hide sensitive columns
- Stabilize API / read models
- Reduce application-side logic

---

## Regular Views vs Materialized Views

### Regular View
- Always up to date
- Executes query every time
- No storage cost

```sql
CREATE VIEW app.user_counts AS
SELECT role_id, COUNT(*) FROM app.user_roles GROUP BY role_id;
```

### Materialized View
- Stores data physically
- Must be refreshed manually
- Faster for heavy queries

```sql
CREATE MATERIALIZED VIEW app.session_stats AS
SELECT date_trunc('day', start_at), COUNT(*)
FROM app.sessions
GROUP BY 1;
```

Refresh:
```sql
REFRESH MATERIALIZED VIEW app.session_stats;
```

---

## Security Patterns

### Read-only access
```sql
GRANT SELECT ON app.active_users TO app_user;
```

### Hide columns
```sql
CREATE VIEW app.public_users AS
SELECT id, first_name, last_name
FROM app.users;
```

### Views + Locked Tables
- Tables: no SELECT for app_user
- Views: SELECT allowed

This forces all reads through controlled queries.

---

## Views vs API Contracts
Treat views as **stable read models**:
- Backend logic changes
- View schema stays stable
- Frontend unaffected

---

## Performance Notes
- Views do NOT cache results
- Index underlying tables, not the view
- Materialized views need indexes explicitly

```sql
CREATE INDEX ON app.session_stats (date_trunc);
```

---

## Updating Through Views
Only possible if:
- Single table
- No joins
- No aggregates

Otherwise: ❌ not allowed.

---

## Common Gotchas
- `SELECT *` in views is dangerous
- Dropping a column breaks dependent views
- Materialized views can go stale
- Views do not bypass permissions

---

## Drop / Replace
```sql
DROP VIEW app.active_users;
CREATE OR REPLACE VIEW app.active_users AS ...
```

---

## When to Use Views
✅ Read models  
✅ Security boundaries  
✅ Admin dashboards  
✅ Audit projections  

❌ High-write paths  
❌ Complex mutation logic  

---

## Mental Model
> Tables = truth  
> Views = perspective  

🗿
