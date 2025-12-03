# MongoDB Delete Cheat Sheet

## Basic Delete Operations

### Delete One Document

``` js
db.collection.deleteOne(
  { field: value }
)
```

Deletes the **first** matching document only.

------------------------------------------------------------------------

### Delete Many Documents

``` js
db.collection.deleteMany(
  { field: value }
)
```

Deletes **all** documents matching the criteria.

------------------------------------------------------------------------

### Delete All Documents in a Collection

``` js
db.collection.deleteMany({})
```

------------------------------------------------------------------------

## Safe Delete Patterns

### Delete by ID

``` js
db.collection.deleteOne({ _id: ObjectId("YOUR_ID_HERE") })
```

### Delete Using Multiple Conditions

``` js
db.collection.deleteMany({
  status: "inactive",
  attempts: { $gt: 3 }
})
```

### Delete with Logical Operators

``` js
db.collection.deleteMany({
  $or: [
    { expired: true },
    { age: { $lt: 18 } }
  ]
})
```

------------------------------------------------------------------------

## findOneAndDelete (Return Document Before Removing)

### Simple Use

``` js
db.collection.findOneAndDelete(
  { username: "john" }
)
```

### With Sorting (delete first by order)

``` js
db.collection.findOneAndDelete(
  { score: { $lt: 50 } },
  { sort: { score: 1 } }
)
```

------------------------------------------------------------------------

## Drop Entire Collection

### Drop Collection

``` js
db.collection.drop()
```

### Drop Database

``` js
db.dropDatabase()
```

⚠️ **Dangerous: irreversible!**

------------------------------------------------------------------------

## Bulk Deletes Using BulkWrite

### Bulk Delete Example

``` js
db.collection.bulkWrite([
  { deleteOne: { filter: { status: "inactive" } } },
  { deleteMany: { filter: { level: { $lt: 3 } } } }
])
```

------------------------------------------------------------------------

## Common Delete Patterns

### Delete Empty Fields / Bad Data

``` js
db.collection.deleteMany({ field: { $exists: false } })
```

### Delete Documents Missing a Required Field

``` js
db.collection.deleteMany({ importantField: null })
```

### Delete All Matching RegEx

``` js
db.collection.deleteMany({ name: /test/i })
```

------------------------------------------------------------------------

## Safety Tips

-   Always test delete filters with:

``` js
db.collection.find(query)
```

before running:

``` js
db.collection.deleteMany(query)
```

-   For risky operations, prefer `findOneAndDelete()` to verify the
    document first.
