# PostgreSQL Index Cheat Sheet

## What is an Index?
An index is a data structure that improves read performance by allowing PostgreSQL to find rows faster without scanning the whole table.

Indexes trade:
- ✅ Faster reads
- ❌ Slower writes (INSERT/UPDATE/DELETE)
- ❌ Extra disk usage

---

## When to Use Indexes
Use indexes when:
- Columns are used in `WHERE`, `JOIN`, `ORDER BY`
- Tables are large
- Queries are frequent

Do **NOT** index:
- Small tables
- Columns with very low cardinality (e.g. boolean, status flags)
- Columns rarely queried

---

## Index Types

### 1. B-tree (default)
Best for:
- Equality (`=`)
- Range (`< > <= >=`)
- Sorting

```sql
CREATE INDEX idx_users_email ON app.users(email);
```

---

### 2. Unique Index
Enforces uniqueness + fast lookup.

```sql
CREATE UNIQUE INDEX uq_users_email ON app.users(email);
```

---

### 3. Composite Index
Index on multiple columns (order matters).

```sql
CREATE INDEX idx_payment_user_status
ON app.payment_intents(user_id, status);
```

Rule:
- Left-most column must be used in query.

---

### 4. Partial Index
Index only part of the table.

```sql
CREATE INDEX idx_active_users
ON app.users(id)
WHERE disabled_at IS NULL;
```

🔥 Extremely powerful for soft-delete patterns.

---

### 5. Expression Index
Index a computed value.

```sql
CREATE INDEX idx_lower_email
ON app.users (lower(email));
```

Must match query expression exactly.

---

### 6. GIN (Generalized Inverted Index)
Best for:
- `JSONB`
- Arrays
- Full-text search

```sql
CREATE INDEX idx_metadata_gin
ON audit.events
USING GIN (metadata);
```

---

### 7. GiST
Best for:
- Geospatial
- Ranges
- Similarity searches

```sql
CREATE INDEX idx_time_range
ON app.sessions
USING GiST (tsrange(start_at, end_at));
```

---

### 8. Hash Index
Equality-only lookups.
Rarely better than B-tree.

```sql
CREATE INDEX idx_hash_example
ON app.tokens
USING HASH (hash);
```

---

## Indexes and Constraints
- `PRIMARY KEY` → creates unique B-tree index
- `UNIQUE` → creates unique index automatically
- `FOREIGN KEY` → ❌ NO automatic index (add manually!)

```sql
CREATE INDEX idx_fk_user_id ON app.sessions(user_id);
```

---

## Indexing Foreign Keys (IMPORTANT)
Always index FK columns used in joins:

```sql
CREATE INDEX idx_user_roles_user_id
ON app.user_roles(user_id);
```

---

## Covering Index (INCLUDE)
Index supports query without touching table.

```sql
CREATE INDEX idx_sessions_lookup
ON app.sessions(coach_id)
INCLUDE (start_at, end_at);
```

🔥 Massive performance win.

---

## Concurrent Index Creation
Avoid table locks in production.

```sql
CREATE INDEX CONCURRENTLY idx_name ON table(column);
```

⚠ Cannot run inside transaction.

---

## Inspect Indexes

```sql
\di
```

```sql
SELECT * FROM pg_indexes WHERE tablename = 'users';
```

---

## Query Planner Debug

```sql
EXPLAIN ANALYZE SELECT ...
```

Look for:
- Seq Scan ❌
- Index Scan ✅
- Bitmap Index Scan ✅

---

## Drop Index

```sql
DROP INDEX idx_name;
```

---

## Golden Rules 🗿
- Index for queries, not tables
- Measure before and after
- Fewer good indexes > many bad ones
- Partial + composite indexes = giga-chad moves
- Indexes are **part of your schema invariants**

---

## Mental Model
Indexes are **read-optimization contracts**:
> "This query path must never be slow."
