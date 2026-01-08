# PostgreSQL Cheat Sheet 🐘

## Connecting
```bash
psql -U username -d database
psql -h localhost -U username -d database
```

## Meta-commands (psql)
```sql
\l        -- list databases
\c db     -- connect to database
\dt       -- list tables
\dn       -- list schemas
\du       -- list roles
\dp       -- list privileges
\q        -- quit
```

## Roles / Users
```sql
CREATE ROLE app_user WITH LOGIN PASSWORD 'secret';
CREATE ROLE admin_user WITH LOGIN SUPERUSER;

ALTER ROLE app_user CREATEDB;
ALTER ROLE app_user CREATEROLE;

DROP ROLE app_user;
```

### Fix: role cannot be dropped
```sql
REVOKE ALL ON SCHEMA public FROM app_user;
ALTER DEFAULT PRIVILEGES REVOKE ALL ON TABLES FROM app_user;
DROP OWNED BY app_user;
DROP ROLE app_user;
```

## Databases
```sql
CREATE DATABASE app_db OWNER app_user;
DROP DATABASE app_db;
```

## Schemas
```sql
CREATE SCHEMA audit AUTHORIZATION app_user;
GRANT USAGE ON SCHEMA public TO app_user;
```

## Tables
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  created_at TIMESTAMP DEFAULT now()
);

DROP TABLE users;
```

## Privileges
```sql
GRANT SELECT, INSERT ON ALL TABLES IN SCHEMA public TO app_user;
REVOKE ALL ON ALL TABLES IN SCHEMA public FROM app_user;

ALTER DEFAULT PRIVILEGES
GRANT SELECT, INSERT ON TABLES TO app_user;
```

## Transactions
```sql
BEGIN;
COMMIT;
ROLLBACK;
```

## Indexes
```sql
CREATE INDEX idx_users_email ON users(email);
DROP INDEX idx_users_email;
```

## Backup & Restore
```bash
pg_dump app_db > backup.sql
psql app_db < backup.sql
```

## Service (Arch Linux)
```bash
sudo systemctl status postgresql
sudo systemctl restart postgresql
```

## Common Errors
- role cannot be dropped → revoke privileges & default privileges
- permission denied → check GRANT on schema/table
- trust auth → check pg_hba.conf

---
Built for sanity, not suffering.
