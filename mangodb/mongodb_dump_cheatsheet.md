
# MongoDB Dump & Restore Cheat Sheet

## 🎯 1. Dump Entire Database Server
Dumps **all databases** into a directory:
```bash
mongodump --out dump/
```

## 🎯 2. Dump a Single Database
```bash
mongodump --db mydb --out dump/
```

## 🎯 3. Dump a Single Collection
```bash
mongodump --db mydb --collection users --out dump/
```

## 🔐 4. Dump With Authentication
```bash
mongodump \
  --host localhost \
  --port 27017 \
  --username admin \
  --password admin123 \
  --authenticationDatabase admin \
  --db mydb \
  --out dump/
```

# RESTORE

## 🔁 5. Restore Entire Dump Directory
```bash
mongorestore dump/
```

## 🔁 6. Restore a Single Database
```bash
mongorestore --db mydb dump/mydb/
```

## 🔁 7. Restore a Single Collection
```bash
mongorestore \
  --db mydb \
  --collection users \
  dump/mydb/users.bson
```

## 🔐 8. Restore With Authentication
```bash
mongorestore \
  --host localhost \
  --port 27017 \
  --username admin \
  --password admin123 \
  --authenticationDatabase admin \
  dump/
```

# ⚙️ Useful Flags

### –gzip
```bash
mongodump --gzip --out dump/
mongorestore --gzip dump/
```

### –archive
```bash
mongodump --archive=db.archive
mongorestore --archive=db.archive
```

### –archive + gzip
```bash
mongodump --archive=db.archive.gz --gzip
mongorestore --archive=db.archive.gz --gzip
```

# 🐳 Podman/Container Version
```bash
podman exec mangodb-mongo-1 mongodump --db mydb --out /dump
podman cp mangodb-mongo-1:/dump ./dump
```
