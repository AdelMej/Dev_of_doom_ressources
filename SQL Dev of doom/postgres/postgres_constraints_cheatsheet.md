# PostgreSQL Constraints – Cheat Sheet

## 1. PRIMARY KEY
Uniquely identifies a row.

```sql
CREATE TABLE users (
    id UUID PRIMARY KEY
);
```

- Implies NOT NULL + UNIQUE
- One per table
- Backed by an index automatically

---

## 2. FOREIGN KEY
Enforces referential integrity.

```sql
CREATE TABLE orders (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id) ON DELETE CASCADE
);
```

### Options
- ON DELETE: CASCADE | RESTRICT | SET NULL | SET DEFAULT
- ON UPDATE: same options

---

## 3. UNIQUE
Ensures column(s) are unique.

```sql
ALTER TABLE users ADD CONSTRAINT uq_users_email UNIQUE (email);
```

- Allows multiple NULLs (important!)
- Use NOT NULL + UNIQUE for strict uniqueness

---

## 4. NOT NULL
Prevents NULL values.

```sql
ALTER TABLE users ALTER COLUMN email SET NOT NULL;
```

- Fast
- Always enforce invariants early

---

## 5. CHECK
Validates expressions.

```sql
ALTER TABLE users
ADD CONSTRAINT chk_age CHECK (age >= 18);
```

### Advanced
```sql
CHECK (status IN ('active', 'disabled'))
CHECK (email ~* '^[^@]+@[^@]+\.[^@]+$')
CHECK (start_date <= end_date)
```

---

## 6. DEFERRABLE CONSTRAINTS
Checked at COMMIT time instead of immediately.

```sql
ALTER TABLE orders
ADD CONSTRAINT fk_orders_user
FOREIGN KEY (user_id)
REFERENCES users(id)
DEFERRABLE INITIALLY DEFERRED;
```

Useful for:
- Circular references
- Multi-step transactions

---

## 7. Naming Conventions
Use explicit names.

- pk_<table>
- fk_<table>_<ref>
- uq_<table>_<column>
- chk_<table>_<rule>

---

## 8. Best Practices
- Database owns invariants
- ORM validation is secondary
- Prefer CHECK over app logic
- Use constraints to kill invalid states early

---

## 9. Common Footguns
- UNIQUE + NULL surprises
- Missing DEFERRABLE when needed
- Overusing CASCADE deletes
- Relying only on ORM validation

---

## TL;DR
If the data must never be wrong:
**constraint > trigger > app code**
