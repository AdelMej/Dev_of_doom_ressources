# PostgreSQL Data Types – Practical Reference

This document lists **PostgreSQL data types**, grouped by category, with **clear explanations** and **notes about auto-increment behavior** where relevant.

---

## 1. Numeric Types

### Small integers
- `SMALLINT` (int2)  
  2 bytes, range: -32,768 to 32,767

- `INTEGER` (int4)  
  4 bytes, range: -2,147,483,648 to 2,147,483,647

- `BIGINT` (int8)  
  8 bytes, very large range

### Auto-increment (legacy)
- `SMALLSERIAL`
- `SERIAL`
- `BIGSERIAL`

⚠️ These **are NOT real types**.  
They automatically create:
- an integer column
- a sequence
- a `DEFAULT nextval(...)`

### Auto-increment (recommended)
- `SMALLINT GENERATED AS IDENTITY`
- `INTEGER GENERATED AS IDENTITY`
- `BIGINT GENERATED AS IDENTITY`

✅ SQL standard  
✅ Explicit  
✅ Safer than `SERIAL`

### Floating point
- `REAL` (float4) – 6 decimal digits
- `DOUBLE PRECISION` (float8) – 15 decimal digits

### Exact numeric
- `NUMERIC(p, s)` / `DECIMAL(p, s)`  
  Exact precision, use for money if needed

---

## 2. Character Types

- `CHAR(n)`  
  Fixed length, padded (rarely used)

- `VARCHAR(n)`  
  Variable length with limit

- `TEXT`  
  Unlimited length (preferred unless constrained)

✅ No performance difference between `VARCHAR` and `TEXT` in Postgres

---

## 3. Boolean

- `BOOLEAN`  
  Values: `TRUE`, `FALSE`, `NULL`

---

## 4. Date & Time

- `DATE`
- `TIME`
- `TIMESTAMP`
- `TIMESTAMPTZ` ✅ **recommended**

`TIMESTAMPTZ` stores UTC internally and converts on output.

Common default:
```sql
created_at timestamptz NOT NULL DEFAULT now()
```

---

## 5. UUID

- `UUID`

Use with:
```sql
DEFAULT gen_random_uuid()
```

Good for public identifiers and distributed systems.

---

## 6. JSON

- `JSON` – stored as text
- `JSONB` ✅ binary, indexed, preferred

Use `JSONB` unless you have a strong reason not to.

---

## 7. Binary Data

- `BYTEA`  
  Raw binary data (files, hashes, etc.)

---

## 8. Enumerated Types

- `ENUM`

Example:
```sql
CREATE TYPE role_type AS ENUM ('user', 'admin');
```

⚠️ Hard to change later. Use sparingly.

---

## 9. Arrays

- `INTEGER[]`
- `TEXT[]`
- `UUID[]`

Example:
```sql
tags TEXT[]
```

Useful but can complicate queries.

---

## 10. Network Types

- `INET` – IP address
- `CIDR` – network range
- `MACADDR`

Great for logging / auditing.

---

## 11. Full Text Search

- `TSVECTOR`
- `TSQUERY`

Used for search indexes.

---

## 12. Ranges

- `INT4RANGE`
- `TSRANGE`
- `TSTZRANGE`

Perfect for time intervals (e.g. sessions, bookings).

---

## 13. Object Identifiers

- `OID`
- `REGCLASS`
- `REGTYPE`

Mostly internal / advanced use.

---

## 14. Special Types

- `INTERVAL`
- `XML`
- `MONEY` ⚠️ discouraged
- `POINT`, `LINE`, `CIRCLE` (geometry)

---

## Summary: Auto-Increment Cheat Sheet

| Type | Auto-Increment | Recommended |
|----|----|----|
| SERIAL | Yes | ❌ |
| BIGSERIAL | Yes | ❌ |
| IDENTITY | Yes | ✅ |
| UUID | No (random) | ✅ |
| BIGINT | No | ❌ |

---

## Best Practices (TL;DR)

- Prefer `TIMESTAMPTZ`
- Prefer `TEXT` over `VARCHAR` unless constrained
- Prefer `IDENTITY` over `SERIAL`
- Prefer `JSONB` over `JSON`
- Prefer `UUID` for public IDs
- Prefer `BIGINT IDENTITY` for audit logs

---

Happy querying 🚀
