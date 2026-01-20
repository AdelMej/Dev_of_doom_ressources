# PostgreSQL Triggers – Cheat Sheet

## What is a Trigger?
A trigger is a function automatically executed when a specified event occurs on a table.

Events:
- INSERT
- UPDATE
- DELETE
- TRUNCATE

Timing:
- BEFORE
- AFTER
- INSTEAD OF (views)

---

## Basic Syntax

### Create trigger function
```sql
CREATE OR REPLACE FUNCTION my_trigger_fn()
RETURNS trigger AS $$
BEGIN
  -- logic
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

### Create trigger
```sql
CREATE TRIGGER my_trigger
BEFORE INSERT ON my_table
FOR EACH ROW
EXECUTE FUNCTION my_trigger_fn();
```

---

## Common Trigger Patterns

### updated_at auto-update
```sql
CREATE OR REPLACE FUNCTION set_updated_at()
RETURNS trigger AS $$
BEGIN
  NEW.updated_at = now();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

```sql
CREATE TRIGGER trg_set_updated_at
BEFORE UPDATE ON app.users
FOR EACH ROW
EXECUTE FUNCTION set_updated_at();
```

---

### Prevent DELETE (soft-delete only)
```sql
CREATE OR REPLACE FUNCTION forbid_delete()
RETURNS trigger AS $$
BEGIN
  RAISE EXCEPTION 'DELETE is forbidden on this table';
END;
$$ LANGUAGE plpgsql;
```

```sql
CREATE TRIGGER trg_no_delete
BEFORE DELETE ON audit.events
FOR EACH ROW
EXECUTE FUNCTION forbid_delete();
```

---

### Make columns immutable
```sql
CREATE OR REPLACE FUNCTION forbid_column_update()
RETURNS trigger AS $$
BEGIN
  IF NEW.created_at <> OLD.created_at THEN
    RAISE EXCEPTION 'created_at is immutable';
  END IF;
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

---

### Enforce append-only table
```sql
CREATE TRIGGER trg_append_only
BEFORE UPDATE OR DELETE ON audit.events
FOR EACH ROW
EXECUTE FUNCTION forbid_delete();
```

---

## Trigger Variables
- NEW → new row (INSERT/UPDATE)
- OLD → old row (UPDATE/DELETE)
- TG_OP → operation (INSERT, UPDATE, DELETE)
- TG_TABLE_NAME → table name
- TG_SCHEMA_NAME → schema

---

## Best Practices
- Put trigger functions in a dedicated schema (e.g. app_triggers)
- Keep triggers **small and explicit**
- Prefer CHECK constraints when possible
- Use triggers for invariants that cannot be expressed otherwise
- Triggers = last line of defense

---

## When NOT to use triggers
- Authorization logic
- Business workflows
- Anything needing external I/O
- Complex branching logic

---

## Mental Model (Based)
> Constraints define what is allowed  
> Triggers define what is impossible  

