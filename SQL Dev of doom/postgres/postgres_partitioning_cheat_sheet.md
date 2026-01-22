# PostgreSQL Table Partitioning – Cheat Sheet

## What is Partitioning?
Partitioning splits a large logical table into multiple physical tables (partitions)
based on a partition key. PostgreSQL handles routing automatically.

Best for:
- Large tables
- Time-series data
- High write volume
- Retention / archival strategies

---

## Partitioning Types

### RANGE
Rows belong to partitions defined by ranges.

Example:
```sql
PARTITION BY RANGE (created_at)
```

Good for:
- Time-based data
- Logs, events, audits

---

### LIST
Rows belong to partitions defined by discrete values.

Example:
```sql
PARTITION BY LIST (status)
```

Good for:
- Enums
- Small, fixed categories

---

### HASH
Rows distributed by hash of a column.

Example:
```sql
PARTITION BY HASH (user_id)
```

Good for:
- Even distribution
- No natural ranges

---

## Creating a Partitioned Table

```sql
CREATE TABLE audit.events (
    id uuid NOT NULL,
    created_at timestamptz NOT NULL,
    data jsonb NOT NULL
) PARTITION BY RANGE (created_at);
```

---

## Creating Partitions

```sql
CREATE TABLE audit.events_2026_01
PARTITION OF audit.events
FOR VALUES FROM ('2026-01-01') TO ('2026-02-01');
```

---

## Default Partition

Catches rows that don’t match any partition.

```sql
CREATE TABLE audit.events_default
PARTITION OF audit.events DEFAULT;
```

---

## Constraints & Indexes

- Constraints on parent apply to partitions
- Indexes must be created on the parent table

```sql
CREATE INDEX ON audit.events (created_at);
```

---

## Inserts & Routing

- INSERT goes to parent table
- Postgres routes to correct partition
- If no matching partition → ERROR (unless DEFAULT exists)

---

## Query Performance

Partition pruning:
- Happens at plan time (static)
- Or execution time (dynamic)

GOOD:
```sql
WHERE created_at >= now() - interval '7 days'
```

BAD:
```sql
WHERE date(created_at) = current_date
```

---

## Triggers & RLS

- Triggers on parent fire for partitions
- RLS policies must exist on parent
- Advisory locks still work normally

---

## Maintenance

Detach partition:
```sql
ALTER TABLE audit.events DETACH PARTITION audit.events_2026_01;
```

Drop old data safely:
```sql
DROP TABLE audit.events_2024_01;
```

---

## Golden Rules

- Partition key should be immutable
- Always create future partitions
- DEFAULT partition avoids outages
- Partitioning = lifecycle control

---

## Mental Model

Parent table = router  
Partitions = real tables  
