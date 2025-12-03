# PyMongo Cheat Sheet

## 1. Install & Import

``` bash
pip install pymongo
```

``` python
from pymongo import MongoClient
```

## 2. Connect to MongoDB

``` python
client = MongoClient("mongodb://localhost:27017/")
db = client["mydatabase"]
collection = db["users"]
```

## 3. Insert Documents

### Insert One

``` python
doc_id = collection.insert_one({"name": "John", "age": 25}).inserted_id
```

### Insert Many

``` python
ids = collection.insert_many([
    {"name": "Alice", "age": 30},
    {"name": "Bob", "age": 22}
]).inserted_ids
```

## 4. Find Documents

### Find One

``` python
user = collection.find_one({"name": "John"})
```

### Find Many

``` python
for user in collection.find({"age": {"$gt": 20}}):
    print(user)
```

### Projection

``` python
collection.find({}, {"name": 1, "_id": 0})
```

### Sorting

``` python
collection.find().sort("age", 1)
```

### Limit & Skip

``` python
collection.find().skip(5).limit(10)
```

## 5. Update Documents

### Update One

``` python
collection.update_one(
    {"name": "John"},
    {"$set": {"age": 26}}
)
```

### Update Many

``` python
collection.update_many(
    {"age": {"$lt": 25}},
    {"$inc": {"age": 1}}
)
```

### Upsert

``` python
collection.update_one(
    {"name": "Eve"},
    {"$set": {"age": 20}},
    upsert=True
)
```

## 6. Delete Documents

### Delete One

``` python
collection.delete_one({"name": "John"})
```

### Delete Many

``` python
collection.delete_many({"age": {"$gt": 40}})
```

## 7. Array Operations

### Push

``` python
collection.update_one(
    {"name": "John"},
    {"$push": {"tags": "new"}}
)
```

### Add Unique

``` python
collection.update_one(
    {"name": "John"},
    {"$addToSet": {"tags": "unique"}}
)
```

### Pull

``` python
collection.update_one(
    {"name": "John"},
    {"$pull": {"tags": "old"}}
)
```

## 8. Counting Documents

``` python
count = collection.count_documents({"age": {"$gt": 25}})
```

## 9. Aggregation Pipelines

``` python
pipeline = [
    {"$match": {"age": {"$gt": 20}}},
    {"$group": {"_id": "$age", "count": {"$sum": 1}}}
]

result = collection.aggregate(pipeline)
for doc in result:
    print(doc)
```

## 10. Creating Indexes

``` python
collection.create_index("name")
collection.create_index([("age", 1), ("name", -1)])
```

## 11. Replace Document

``` python
collection.replace_one(
    {"name": "John"},
    {"name": "John", "age": 30, "city": "Paris"}
)
```

## 12. Bulk Operations

``` python
from pymongo import InsertOne, DeleteOne, UpdateOne

collection.bulk_write([
    InsertOne({"name": "Paul", "age": 18}),
    UpdateOne({"name": "Alice"}, {"$inc": {"age": 1}}),
    DeleteOne({"name": "Bob"})
])
```
