# PL/pgSQL Cheat Sheet

A concise, practical reference for writing PostgreSQL `plpgsql` functions and triggers.

---

## Function Basics

```sql
CREATE OR REPLACE FUNCTION schema.fn_name(args)
RETURNS return_type
LANGUAGE plpgsql
AS $$
BEGIN
    -- logic
    RETURN value;
END;
$$;
```

### Void function

```sql
RETURNS void
RETURN;
```

---

## Variables

```sql
DECLARE
    v_id UUID;
    v_count INTEGER := 0;
    v_now TIMESTAMPTZ := now();
```

---

## Control Flow

### IF / ELSIF / ELSE

```sql
IF condition THEN
    ...
ELSIF other_condition THEN
    ...
ELSE
    ...
END IF;
```

### CASE

```sql
CASE
    WHEN cond1 THEN ...
    WHEN cond2 THEN ...
    ELSE ...
END CASE;
```

---

## Loops

### LOOP

```sql
LOOP
    EXIT WHEN condition;
END LOOP;
```

### WHILE

```sql
WHILE condition LOOP
    ...
END LOOP;
```

### FOR

```sql
FOR i IN 1..10 LOOP
    ...
END LOOP;
```

### FOR record

```sql
FOR r IN SELECT * FROM table LOOP
    ...
END LOOP;
```

---

## SELECT INTO

```sql
SELECT col1, col2
INTO v1, v2
FROM table
WHERE id = some_id;
```

Check result:

```sql
IF NOT FOUND THEN
    RAISE EXCEPTION 'No row found';
END IF;
```

---

## Exceptions

```sql
BEGIN
    ...
EXCEPTION
    WHEN unique_violation THEN
        ...
    WHEN others THEN
        RAISE;
END;
```

Custom error:

```sql
RAISE EXCEPTION 'message';
RAISE EXCEPTION 'message %', value;
```

---

## Triggers

### Trigger function template

```sql
CREATE FUNCTION schema.tg_name()
RETURNS trigger
LANGUAGE plpgsql
AS $$
BEGIN
    IF TG_OP = 'INSERT' THEN
        ...
    ELSIF TG_OP = 'UPDATE' THEN
        ...
    END IF;

    RETURN NEW; -- or OLD for DELETE
END;
$$;
```

### Attach trigger

```sql
CREATE TRIGGER trg_name
BEFORE INSERT OR UPDATE
ON schema.table
FOR EACH ROW
EXECUTE FUNCTION schema.tg_name();
```

---

## Trigger Context Variables

| Variable | Meaning |
|--------|--------|
| `TG_OP` | INSERT / UPDATE / DELETE |
| `NEW` | New row |
| `OLD` | Old row |
| `TG_TABLE_NAME` | Table name |
| `TG_WHEN` | BEFORE / AFTER |

---

## Immutability Pattern

```sql
IF TG_OP = 'UPDATE' AND OLD.col IS NOT NULL THEN
    RAISE EXCEPTION 'Row is immutable';
END IF;
```

---

## Security & RLS Helpers

Current role:

```sql
current_user
```

JWT claims (Supabase-style):

```sql
current_setting('request.jwt.claim.sub', true)
```

---

## Best Practices

- Always use `BEFORE` triggers for validation
- Enforce business rules in triggers, not app code
- Keep triggers small and deterministic
- Prefer constraints where possible
- Never trust frontend timestamps

---

## Common Gotchas

- Forgetting `RETURN NEW`
- Using `SELECT` instead of `SELECT INTO`
- Mutating immutable rows
- Trigger recursion

---

## Debugging

```sql
RAISE NOTICE 'value: %', var;
```

---

## When to Use What

| Need | Use |
|----|----|
| Simple invariant | CHECK constraint |
| Cross-table rule | Trigger |
| Authorization | RLS |
| Business timing | Trigger |
| Derived data | Generated column |

---

Happy cooking 🔥
