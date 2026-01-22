# PostgreSQL SECURITY DEFINER – Cheat Sheet

This cheat sheet explains **SECURITY DEFINER functions** in PostgreSQL, why they exist, how to use them safely, and common pitfalls.

---

## What is SECURITY DEFINER?

By default, PostgreSQL functions run as the **caller**:

SECURITY INVOKER (default)

With `SECURITY DEFINER`, the function runs with the **privileges of its owner**.

This allows controlled privilege escalation **without granting broad table access**.

---

## Core Use Cases

- Keep GRANTS minimal (no direct SELECT on tables)
- Use functions inside RLS policies
- Centralize sensitive logic
- Avoid permission errors inside RLS
- Expose intent-based access instead of raw data access

---

## Basic Syntax

```sql
CREATE OR REPLACE FUNCTION app.can_view_session(p_session_id uuid)
RETURNS boolean
LANGUAGE sql
SECURITY DEFINER
AS $$
    SELECT EXISTS (
        SELECT 1
        FROM app.session_participation sp
        WHERE sp.session_id = p_session_id
          AND sp.user_id = current_setting('app.current_user_id')::uuid
    );
$$;
```

---

## Ownership Rules (CRITICAL)

The function executes with the **privileges of its owner**.

```sql
ALTER FUNCTION app.can_view_session(uuid)
OWNER TO app_admin;
```

Best practice:
- Owner = `app_admin`
- Callers = restricted roles (`app_user`, `app_system`)

---

## Granting Execute Only

Never grant table access if a function is meant to replace it.

```sql
GRANT EXECUTE ON FUNCTION app.can_view_session(uuid) TO app_user;
```

Caller can:
- Execute function
- NOT read underlying tables

---

## SECURITY DEFINER + RLS

SECURITY DEFINER functions **bypass table GRANTS**, but:
- RLS still applies unless table owner bypasses it
- Use helper functions to safely evaluate policies

Example RLS policy:

```sql
CREATE POLICY session_visibility
ON app.sessions
USING (app.can_view_session(id));
```

---

## Using Session Context (SET LOCAL)

Typical pattern:

```sql
SET LOCAL app.current_user_id = 'uuid';
```

Inside function:

```sql
current_setting('app.current_user_id', true)::uuid
```

Use `true` to avoid hard errors if unset.

---

## Safe Pattern: Boolean Gate Functions

Best practice:
- Return `boolean`
- No data leakage
- Used by RLS or CHECK constraints

Avoid:
- Returning rows
- Returning sensitive columns

---

## Search Path Hardening (IMPORTANT)

Always lock the search path:

```sql
ALTER FUNCTION app.can_view_session(uuid)
SET search_path = app, pg_temp;
```

---

## Recommended Template

```sql
CREATE OR REPLACE FUNCTION app.fn_name(args...)
RETURNS boolean
LANGUAGE sql
SECURITY DEFINER
SET search_path = app, pg_temp
AS $$
    -- logic
$$;

ALTER FUNCTION app.fn_name(args...)
OWNER TO app_admin;

GRANT EXECUTE ON FUNCTION app.fn_name(args...)
TO app_user;
```

---

## Common Pitfalls

- Forgetting to change owner
- Forgetting to lock search_path
- Returning data instead of booleans
- Using SECURITY DEFINER everywhere
- Granting table access anyway

---

## Mental Model

Think of SECURITY DEFINER functions as:

**Database syscalls**

Small, explicit, audited entry points.

---

## Final Advice

Prefer **boolean gate functions**  
Combine with RLS  
Keep functions tiny  
Audit them like public APIs
