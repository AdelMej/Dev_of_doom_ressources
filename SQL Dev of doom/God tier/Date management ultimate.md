### 🧭 **1. Time-Zone-Safe Storage**

> _Rule #1: Always store timestamps in UTC, display them in local time._

```sql
-- Store UTC by default
CREATE TABLE logs (
  id SERIAL PRIMARY KEY,
  message TEXT,
  created_at TIMESTAMPTZ DEFAULT CURRENT_TIMESTAMP  -- PostgreSQL
);

-- Convert safely to local time
SELECT created_at AT TIME ZONE 'UTC' AT TIME ZONE 'Europe/Paris'
FROM logs;

-- MySQL equivalent
ALTER TABLE logs MODIFY created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP;
SELECT CONVERT_TZ(created_at, 'UTC', 'Europe/Paris') FROM logs;
```

✅ **Best Practice:**  
Always store in UTC → convert on output.  
Never store mixed time zones in one table.

---

### 🧮 **2. Business Day Logic**

> Skip weekends, find next working day, and calculate business durations.

```sql
-- PostgreSQL: find next working day
SELECT
  CASE
    WHEN EXTRACT(DOW FROM CURRENT_DATE) IN (6, 0) -- Sat=6, Sun=0
    THEN CURRENT_DATE + INTERVAL '1 day' * (8 - EXTRACT(DOW FROM CURRENT_DATE))
    ELSE CURRENT_DATE
  END AS next_business_day;

-- Days between two dates excluding weekends
SELECT COUNT(*) AS workdays
FROM generate_series('2025-10-01'::date, '2025-10-21'::date, '1 day') AS g(day)
WHERE EXTRACT(DOW FROM g.day) NOT IN (0, 6);
```

MySQL doesn’t have `generate_series`, but you can create a helper “calendar” table and query from that.

---

### 🧾 **3. Fiscal & Quarter Logic**

> Perfect for reports, accounting, and data analytics.

```sql
-- PostgreSQL: current quarter and fiscal year
SELECT
  EXTRACT(QUARTER FROM CURRENT_DATE) AS quarter,
  CASE
    WHEN EXTRACT(MONTH FROM CURRENT_DATE) >= 4
    THEN EXTRACT(YEAR FROM CURRENT_DATE)
    ELSE EXTRACT(YEAR FROM CURRENT_DATE) - 1
  END AS fiscal_year;

-- First and last day of current quarter
SELECT
  DATE_TRUNC('quarter', CURRENT_DATE) AS quarter_start,
  (DATE_TRUNC('quarter', CURRENT_DATE) + INTERVAL '3 month' - INTERVAL '1 day') AS quarter_end;
```

---

### ⚙️ **4. Performance & Indexing**

> Time-based queries are fast _only if your timestamps are indexed._

```sql
-- Index your date column
CREATE INDEX idx_logs_created_at ON logs (created_at);

-- Optimize rolling queries
SELECT * FROM logs
WHERE created_at >= NOW() - INTERVAL '7 days';

-- Partitioning (PostgreSQL example)
CREATE TABLE logs_2025_10 PARTITION OF logs
FOR VALUES FROM ('2025-10-01') TO ('2025-11-01');
```

✅ **Tips:**

- Use **range partitions** by month or year for very large tables.
- Keep **recent data “hot”** in fast storage.
- Archive older data to cold storage using scripts or scheduled jobs.

---

### 🧩 **5. Time Bucketing (for Graphs & Stats)**

> Aggregate events into fixed time windows for dashboards.

```sql
-- PostgreSQL: hourly buckets
SELECT
  DATE_TRUNC('hour', created_at) AS hour_bucket,
  COUNT(*) AS event_count
FROM logs
GROUP BY hour_bucket
ORDER BY hour_bucket;

-- MySQL: use FLOOR on UNIX timestamps
SELECT
  FROM_UNIXTIME(FLOOR(UNIX_TIMESTAMP(created_at)/3600)*3600) AS hour_bucket,
  COUNT(*) AS event_count
FROM logs
GROUP BY hour_bucket
ORDER BY hour_bucket;
```

---

### 🕓 **6. Interval Math & Scheduling**

> Schedule jobs or predict next run dates precisely.

```sql
-- Next run date based on interval
SELECT last_run + INTERVAL '15 minutes' AS next_run FROM jobs;

-- Tasks overdue
SELECT * FROM jobs
WHERE last_run + INTERVAL '1 day' < NOW();
```

---

### 💡 **7. Practical Production Tips**

|Tip|Why|
|---|---|
|Use `TIMESTAMPTZ` (PostgreSQL) or `TIMESTAMP` with `UTC` (MySQL)|Prevents daylight-saving bugs|
|Never store local time directly|Local time shifts cause mismatched data|
|Always index timestamps|Huge performance boost|
|Partition big tables by month|Keeps queries fast|
|Use `DATE_TRUNC` for grouping|Perfect for charts, reports, and logs|