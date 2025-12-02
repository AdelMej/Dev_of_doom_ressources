# PostgreSQL Partitioning Cheat Sheet

## 1. Time-Based Partitioning (GOAT)

Best for: - Logs, metrics, dashboards\
- Append‑only data\
- High‑volume tables

Benefits: - Fast OFFSET and cursor pagination\
- Small indexes per partition\
- Instant deletion via `DROP TABLE`\
- Automatic partition pruning

**Example:**

``` sql
CREATE TABLE events (
    id BIGSERIAL,
    created_at TIMESTAMPTZ NOT NULL,
    payload JSONB
) PARTITION BY RANGE (created_at);
```

## 2. Hash Partitioning

Best for: - User‑specific data (messages, profiles, preferences)

Benefits: - Load balancing\
- Prevents index bloat\
- Ultra‑fast per‑user pagination

**Example:**

``` sql
CREATE TABLE messages (
    uid BIGINT NOT NULL,
    mid BIGSERIAL,
    content TEXT,
    sent_at TIMESTAMPTZ NOT NULL
) PARTITION BY HASH (uid);
```

## 3. Range Partitioning on ID

Best for: - Append‑only sequences (orders, event IDs)

Benefits: - Uniform partition sizes\
- Fast ID‑based cursor pagination

**Example:**

``` sql
PARTITION BY RANGE (id);
```

## 4. Combining Strategies (Giga‑Brain Mode)

-   Time + Hash\
-   Time + Range

Used in: - Billion‑row log systems\
- Real‑time analytics\
- Cloud‑scale dashboards

## 5. Indexing Strategy (Important)

Create per‑partition indexes:

``` sql
CREATE INDEX idx_events_created_at ON events_2025_12_02 (created_at);
CREATE INDEX idx_events_id ON events_2025_12_02 (id);
```

## 6. Auto‑Management

### Create partitions automatically:

Use `pg_cron`:

``` sql
SELECT cron.schedule(
  'create_partitions',
  '0 0 * * *',
  $$ CALL create_tomorrow_partition(); $$
);
```

### Drop old data instantly:

``` sql
DROP TABLE events_2025_10_01;
```

## TL;DR

-   Time partitioning = best for logs + metrics\
-   Hash partitioning = best for user‑centric data\
-   Range on ID = best for append‑only systems\
-   Partitioning fixes offset performance\
-   Deletes become instant\
-   Indexes stay tiny\
-   Pagination becomes god‑tier
