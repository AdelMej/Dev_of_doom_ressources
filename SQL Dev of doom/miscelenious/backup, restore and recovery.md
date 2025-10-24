## 💀 **God-Tier SQL — Backup, Restore & Recovery Cheat Sheet**

---

### ⚙️ **1. Why Backups Matter**

> “Data that isn’t backed up is data you’ve already decided to lose.” 💣

Backups are your **last defense** against:

- Hardware failures
- Human errors
- Corruption
- Ransomware
- Botched migrations

---

### 🧱 **2. Types of Backups**

|Type|Description|Pros|Cons|
|---|---|---|---|
|**Full**|Entire database snapshot|Simple, complete|Large, slow|
|**Incremental**|Only changes since last backup|Efficient|Requires chain of backups|
|**Differential**|Changes since last full backup|Faster restores|Larger over time|
|**Logical**|Export SQL statements|Portable|Slower restore|
|**Physical**|Copy database files|Fast restore|System-specific|

---

### 🧠 **3. Logical Backup — `pg_dump` (PostgreSQL)**

```bash
# Backup single database
pg_dump mydb > mydb_backup.sql

# With compression
pg_dump -Fc mydb > mydb_backup.dump

# Restore
psql mydb < mydb_backup.sql
pg_restore -d mydb mydb_backup.dump
```


**Partial backups:**

```bash
pg_dump -t users mydb > users_table.sql  # single table
pg_dump -s mydb > schema_only.sql        # schema only
```

---

### 🧩 **4. Logical Backup — `mysqldump` (MySQL)**

```sql
# Backup
mysqldump -u root -p mydb > mydb_backup.sql

# With schema only
mysqldump -u root -p --no-data mydb > schema_only.sql

# Restore
mysql -u root -p mydb < mydb_backup.sql
```

✅ Add compression:

```bash
mysqldump mydb | gzip > mydb_backup.sql.gz
gunzip < mydb_backup.sql.gz | mysql mydb
```
---

### 🧮 **5. Physical Backups**

#### PostgreSQL (File-Level)

```bash
pg_basebackup -D /backup/mydb -F tar -z -P
```

#### MySQL

```bash
systemctl stop mysql
cp -r /var/lib/mysql /backup/
systemctl start mysql
```

⚠️ Must stop DB (or use `xtrabackup` for live backup).

---

### 🔁 **6. Point-in-Time Recovery (PITR)**

> Restore to _any exact moment_ before failure.

**PostgreSQL:**

1. Enable WAL archiving:
	```bash
	wal_level = replica
	archive_mode = on
	archive_command = 'cp %p /backup/wal/%f'
	```
    
2. Restore from backup + WAL logs:
    ```bash
    restore_command = 'cp /backup/wal/%f %p'
	recovery_target_time = '2025-10-21 18:30:00'
    ```
    

---

### 🧱 **7. Verify and Test Restores**

> A backup is useless if you never test restoring it.

**Checklist:**

- Restore to a test environment regularly.
- Compare table counts, checksums, and indexes.
- Automate validation with cron or CI scripts.

Example:

```sql
pg_restore -l backup.dump  # list contents
pg_restore --check -d testdb backup.dump
```

---

### ⚡ **8. Automating Backups**

#### PostgreSQL (cron example)

```sql
0 2 * * * pg_dump -Fc mydb > /backups/mydb_$(date +\%F).dump
find /backups -type f -mtime +7 -delete  # keep 7 days
```

#### MySQL (daily)

```sql
0 3 * * * mysqldump -u root -pMyPass mydb | gzip > /backups/mydb_$(date +\%F).sql.gz
```

---

### 🧰 **9. Backup Storage Best Practices**

|Rule|Reason|
|---|---|
|Store off-site copies|Protect against local disasters|
|Encrypt sensitive backups|GDPR & security compliance|
|Use versioned filenames|Avoid overwriting|
|Rotate backups|Manage disk space|
|Store checksums|Detect corruption|
|Use cloud storage (S3, Backblaze)|Durable and redundant|

---

### 🔒 **10. Backup Encryption**

#### PostgreSQL:

```sql
pg_dump mydb | gpg -c > mydb_backup.sql.gpg
gpg -d mydb_backup.sql.gpg | psql mydb
```

#### MySQL:

```sql
mysqldump mydb | openssl enc -aes-256-cbc -salt -out backup.sql.enc
openssl enc -d -aes-256-cbc -in backup.sql.enc | mysql mydb
```

---

### 🧠 **11. Disaster Recovery Plan**

> Know exactly what to do when things explode 💥

✅ **Critical Steps**

1. Identify the failure type (hardware, corruption, human).
2. Stop affected services.
3. Restore last good backup.
4. Apply logs (for PITR).
5. Validate consistency (`CHECKSUM`, `COUNT(*)`, foreign keys).
6. Re-enable access gradually.

---

### 🏁 **12. Golden Backup Rules**

|Rule|Description|
|---|---|
|3-2-1 Rule|3 copies, 2 types of media, 1 off-site|
|Automate Everything|No manual backups = no forgotten backups|
|Verify Often|Test restores monthly|
|Encrypt & Rotate|Security + disk space management|
|Keep WAL/Redo Logs|Enables point-in-time recovery|
|Use Transactional Backups|Avoid data inconsistency|

---

### 🧙‍♂️ **13. Final Wisdom**

> “A real DBA doesn’t fear system crashes — they **rebuild worlds** from a dump file.” 🌍

Always plan for failure.  
The gods of uptime favor the ones who test their recovery scripts.