# PostgreSQL `pg_advisory_xact_lock` Cheat Sheet

A quick, practical reference for using **transaction-level advisory locks** in PostgreSQL.

---

## What it is

`pg_advisory_xact_lock` acquires a **transaction-scoped advisory lock**.

- Automatically released at **transaction end** (COMMIT / ROLLBACK)
- No manual unlock required
- Perfect for **preventing race conditions**
- Enforced **inside PostgreSQL**, not application-side

---

## Why use it

Use when you need to enforce **cross-row or cross-table invariants**, such as:

- Max participants per session
- Prevent double-spend / double-booking
- Serialize writes per logical entity
- Replace fragile `COUNT(*)` + trigger logic

---

## Basic usage

```sql
SELECT pg_advisory_xact_lock(42);
```

- Blocks until the lock on key `42` is acquired
- Lock is held **until transaction ends**

---

## Inside a trigger (common pattern)

```sql
PERFORM pg_advisory_xact_lock(NEW.session_id);
```

Guarantees:
- Only one transaction at a time can modify data for that `session_id`
- Prevents concurrent inserts from violating invariants

---

## Typical trigger example

```sql
CREATE OR REPLACE FUNCTION enforce_limit()
RETURNS trigger AS $$
DECLARE
    cnt integer;
BEGIN
    PERFORM pg_advisory_xact_lock(NEW.session_id);

    SELECT COUNT(*)
    INTO cnt
    FROM session_participation
    WHERE session_id = NEW.session_id;

    IF cnt >= 6 THEN
        RAISE EXCEPTION 'Session is full';
    END IF;

    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

---

## Lock key types

### Single bigint key
```sql
pg_advisory_xact_lock(bigint)
```

### Two int keys (namespacing)
```sql
pg_advisory_xact_lock(int, int)
```

Example:
```sql
-- namespace 1 = sessions
SELECT pg_advisory_xact_lock(1, session_id);
```

---

## Transaction-level vs session-level

| Function | Scope | Manual unlock |
|-------|------|---------------|
| `pg_advisory_xact_lock` | Transaction | ❌ No |
| `pg_advisory_lock` | Session | ✅ Yes |

**Rule of thumb:**  
👉 Always prefer `pg_advisory_xact_lock` inside triggers.

---

## Blocking behavior

- Blocks until lock is available
- Respects transaction isolation
- No deadlock if you:
  - Always lock in the same order
  - Use one logical key per invariant

---

## Non-blocking variant

```sql
SELECT pg_try_advisory_xact_lock(42);
```

Returns:
- `true` → lock acquired
- `false` → lock not acquired

---

## Best practices

- Use **domain identifiers** as lock keys
- Keep critical sections **short**
- One invariant = one lock
- Document why the lock exists

---

## When NOT to use it

- Simple row-level constraints
- Pure uniqueness checks
- High-contention hot keys (use carefully)

---

## Mental model

> “This lock represents a business rule, not a row.”

---

## TL;DR

- Use `pg_advisory_xact_lock` to **serialize business logic**
- Perfect for triggers and invariants
- Safer than `COUNT(*)` under concurrency
- Auto-unlocks at transaction end

---

Happy locking 🗝️
