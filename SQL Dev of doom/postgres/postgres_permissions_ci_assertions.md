# PostgreSQL Permission Assertions for CI

This cheat sheet contains **SQL snippets you can run in CI** to assert that your
PostgreSQL permissions are **correct, locked down, and non-regressing**.

These are designed to **FAIL FAST** if someone accidentally grants too much.

---

## 1. Assert Role Exists

```sql
SELECT 1
FROM pg_roles
WHERE rolname = 'app_user';
```

CI expectation: **at least one row**

---

## 2. Assert Role Does NOT Have Superuser

```sql
SELECT rolsuper
FROM pg_roles
WHERE rolname = 'app_user';
```

CI expectation: `false`

---

## 3. Assert No Database CREATE Privilege

```sql
SELECT has_database_privilege('app_user', current_database(), 'CREATE');
```

CI expectation: `false`

---

## 4. Assert Schema Usage Only (No CREATE)

```sql
SELECT
  has_schema_privilege('app_user', 'public', 'USAGE') AS usage,
  has_schema_privilege('app_user', 'public', 'CREATE') AS create;
```

CI expectation:
- usage = true
- create = false

---

## 5. Assert Table Access Is Read-Only

```sql
SELECT
  has_table_privilege('app_user', 'users', 'SELECT') AS can_select,
  has_table_privilege('app_user', 'users', 'INSERT') AS can_insert,
  has_table_privilege('app_user', 'users', 'UPDATE') AS can_update,
  has_table_privilege('app_user', 'users', 'DELETE') AS can_delete;
```

CI expectation:
- select = true
- insert/update/delete = false

---

## 6. Assert No Sequence Abuse (Critical)

```sql
SELECT
  has_sequence_privilege('app_user', 'users_id_seq', 'USAGE'),
  has_sequence_privilege('app_user', 'users_id_seq', 'UPDATE');
```

CI expectation:
- usage = false unless needed
- update = false ALWAYS

---

## 7. Assert Function Execution Allowed

```sql
SELECT has_function_privilege(
  'app_user',
  'create_user(text,text)',
  'EXECUTE'
);
```

CI expectation: `true`

---

## 8. Assert PUBLIC Has No Rights (Very Important)

```sql
SELECT
  has_schema_privilege('public', 'public', 'CREATE'),
  has_schema_privilege('public', 'public', 'USAGE');
```

CI expectation:
- create = false
- usage = false (or explicitly controlled)

---

## 9. Assert Default Privileges Are Locked

```sql
SELECT *
FROM pg_default_acl
WHERE defaclrole = (
  SELECT oid FROM pg_roles WHERE rolname = 'db_owner'
);
```

CI expectation:
- no unexpected default grants to app_user or PUBLIC

---

## 10. Assert RLS Is Enabled

```sql
SELECT relrowsecurity
FROM pg_class
WHERE relname = 'users';
```

CI expectation: `true`

---

## 11. Assert RLS Policy Exists

```sql
SELECT policyname
FROM pg_policies
WHERE tablename = 'users';
```

CI expectation:
- at least one policy

---

## 12. Assert App Cannot Bypass RLS

```sql
SELECT rolbypassrls
FROM pg_roles
WHERE rolname = 'app_user';
```

CI expectation: `false`

---

## 13. Hard FAIL If Any Forbidden Privilege Exists

```sql
SELECT *
FROM information_schema.role_table_grants
WHERE grantee = 'app_user'
  AND privilege_type IN ('TRUNCATE', 'REFERENCES');
```

CI expectation: **0 rows**

---

## 14. CI Pattern (psql)

```bash
psql -v ON_ERROR_STOP=1 -f permissions_assertions.sql
```

Fail the pipeline if **any assertion returns unexpected results**.

---

## Philosophy

If your CI does NOT assert permissions:
- you don't have security
- you have hope

And hope is not a strategy.

🦍 Locked DB = Easy Backend = Easy Frontend
