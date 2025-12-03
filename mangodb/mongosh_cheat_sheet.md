# MongoDB (mongosh) Cheat Sheet

## 🔹 Start mongosh in a container
podman exec -it <container_name> mongosh

## 🔹 Connect to a specific DB
use myDatabase

## 🔹 Show current database
db

## 🔹 List all databases
show dbs

## 🔹 List all collections in current DB
show collections

---

# 📁 Working With Collections

## 🔹 Create a collection
db.createCollection("users")

## 🔹 Drop a collection
db.users.drop()

---

# 📄 Insert Documents

## 🔹 Insert one
db.users.insertOne({ name: "John", age: 30 })

## 🔹 Insert many
db.users.insertMany([
  { name: "Alice", age: 25 },
  { name: "Bob", age: 40 }
])

---

# 🔍 Query Documents

## 🔹 Find all
db.users.find()

## 🔹 Find with filter
db.users.find({ age: 30 })

## 🔹 Pretty print
db.users.find().pretty()

## 🔹 Find one
db.users.findOne({ name: "John" })

---

# ✏️ Update Documents

## 🔹 Update one field
db.users.updateOne(
  { name: "John" },
  { $set: { age: 31 } }
)

## 🔹 Update many
db.users.updateMany(
  { age: { $gt: 30 } },
  { $set: { active: true } }
)

---

# 🗑️ Delete Documents

## 🔹 Delete one
db.users.deleteOne({ name: "Bob" })

## 🔹 Delete many
db.users.deleteMany({ age: { $lt: 20 } })

---

# 🔢 Counting

db.users.countDocuments()
db.users.countDocuments({ active: true })

---

# 📊 Indexes

## 🔹 Create index
db.users.createIndex({ email: 1 })

## 🔹 List indexes
db.users.getIndexes()

## 🔹 Drop index
db.users.dropIndex("email_1")

---

# 📄 Load a JS file in mongosh

load("script.js")

---

# 🔒 Users & Roles (Admin)

## 🔹 Create admin user
use admin
db.createUser({
  user: "admin",
  pwd: "password",
  roles: ["root"]
})

## 🔹 Show users
show users
