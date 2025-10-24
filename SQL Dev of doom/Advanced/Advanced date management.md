## 🧮 **Advanced SQL Date & Time Functions Cheat Sheet**

### 🎂 **1. Age / Difference Calculations**

```sql
-- PostgreSQL: exact difference (returns years, months, days)
SELECT AGE(NOW(), birth_date) AS age FROM users;

-- MySQL: difference in days
SELECT DATEDIFF(NOW(), birth_date) AS days_old FROM users;

-- MySQL: convert days to years (approx.)
SELECT FLOOR(DATEDIFF(NOW(), birth_date) / 365) AS age_years FROM users;

-- Difference in hours, minutes, seconds
SELECT TIMESTAMPDIFF(HOUR, start_time, end_time) AS hours_diff FROM sessions;
SELECT TIMESTAMPDIFF(MINUTE, start_time, end_time) AS minutes_diff FROM sessions;
```

---

### 🌍 **2. Time Zone Handling**

```sql
-- PostgreSQL: convert to another timezone
SELECT CURRENT_TIMESTAMP AT TIME ZONE 'UTC';
SELECT CURRENT_TIMESTAMP AT TIME ZONE 'Europe/Paris';

-- MySQL: convert timezone
SELECT CONVERT_TZ(NOW(), 'UTC', 'Europe/Paris');

-- Show server time zone
SHOW TIMEZONE;        -- PostgreSQL
SELECT @@global.time_zone, @@session.time_zone; -- MySQL
```

---

### 🕐 **3. Truncating Dates (Round Down)**

```sql
-- PostgreSQL
SELECT DATE_TRUNC('year', CURRENT_DATE);   -- 2025-01-01
SELECT DATE_TRUNC('month', CURRENT_DATE);  -- 2025-10-01
SELECT DATE_TRUNC('day', CURRENT_TIMESTAMP); -- 2025-10-21 00:00:00

-- MySQL (no DATE_TRUNC, so use DATE_FORMAT)
SELECT DATE_FORMAT(NOW(), '%Y-01-01') AS year_start;
SELECT DATE_FORMAT(NOW(), '%Y-%m-01') AS month_start;
```

---

### ⏳ **4. Working with Intervals**

```sql
-- PostgreSQL: add/subtract durations
SELECT NOW() + INTERVAL '3 days';
SELECT NOW() - INTERVAL '2 hours';

-- MySQL: similar syntax
SELECT NOW() + INTERVAL 3 DAY;
SELECT NOW() - INTERVAL 2 HOUR;

-- Store an interval column
CREATE TABLE tasks (
  id SERIAL PRIMARY KEY,
  duration INTERVAL
);
INSERT INTO tasks (duration) VALUES (INTERVAL '1 hour');
```

---

### 🧩 **5. Date Comparison Examples**

```sql
-- Events happening today
SELECT * FROM events
WHERE DATE(event_date) = CURRENT_DATE;

-- Events in the next 7 days
SELECT * FROM events
WHERE event_date BETWEEN CURRENT_DATE AND CURRENT_DATE + INTERVAL '7 days';

-- Records from the past 24 hours
SELECT * FROM logs
WHERE created_at >= NOW() - INTERVAL '1 day';
```

---

### 🧠 **6. Combining Date Logic**

```sql
-- Last login older than 30 days
SELECT * FROM users
WHERE last_login < NOW() - INTERVAL '30 days';

-- Users registered this quarter
SELECT * FROM users
WHERE DATE_TRUNC('quarter', registration_date) =
      DATE_TRUNC('quarter', CURRENT_DATE);

-- Next month’s birthdays
SELECT * FROM users
WHERE EXTRACT(MONTH FROM birth_date) = EXTRACT(MONTH FROM CURRENT_DATE + INTERVAL '1 month');
```

---

### 🕰️ **7. Useful Conversion Helpers**

```sql
-- String to date
SELECT TO_DATE('2025-12-31', 'YYYY-MM-DD');          -- PostgreSQL
SELECT STR_TO_DATE('31/12/2025', '%d/%m/%Y');        -- MySQL

-- Date to string
SELECT TO_CHAR(NOW(), 'DD Mon YYYY, HH24:MI');       -- PostgreSQL
SELECT DATE_FORMAT(NOW(), '%d %b %Y, %H:%i');        -- MySQL
```