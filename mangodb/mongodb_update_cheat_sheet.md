# MongoDB Update Cheat Sheet

## Basic Update Operations

### Update One Document

``` js
db.collection.updateOne(
  { filterField: value },
  { $set: { field: newValue } }
)
```

### Update Many Documents

``` js
db.collection.updateMany(
  { filterField: value },
  { $set: { field: newValue } }
)
```

### Replace Entire Document

``` js
db.collection.replaceOne(
  { _id: someId },
  { new: "document", only: "these fields remain" }
)
```

------------------------------------------------------------------------

## Update Operators

### \$set --- Change or Add Fields

``` js
db.collection.updateOne(
  { name: "John" },
  { $set: { age: 30, city: "Paris" } }
)
```

### \$unset --- Remove Fields

``` js
db.collection.updateOne(
  { name: "John" },
  { $unset: { age: "" } }
)
```

### \$inc --- Increment Numbers

``` js
db.collection.updateOne(
  { name: "John" },
  { $inc: { score: 5 } }
)
```

### \$mul --- Multiply Numbers

``` js
db.collection.updateOne(
  { name: "John" },
  { $mul: { score: 2 } }
)
```

### \$rename --- Rename Fields

``` js
db.collection.updateOne(
  { name: "John" },
  { $rename: { "oldField": "newField" } }
)
```

------------------------------------------------------------------------

## Array Updates

### Add to Array (\$push)

``` js
db.collection.updateOne(
  { name: "John" },
  { $push: { tags: "newTag" } }
)
```

### Add Multiple (\$push with \$each)

``` js
db.collection.updateOne(
  { name: "John" },
  { $push: { tags: { $each: ["red", "blue"] } } }
)
```

### Add Only If Not Exists (\$addToSet)

``` js
db.collection.updateOne(
  { name: "John" },
  { $addToSet: { tags: "uniqueTag" } }
)
```

### Remove from Array (\$pull)

``` js
db.collection.updateOne(
  { name: "John" },
  { $pull: { tags: "removeMe" } }
)
```

### Remove Multiple with Condition (\$pull)

``` js
db.collection.updateOne(
  { name: "John" },
  { $pull: { scores: { $lt: 5 } } }
)
```

### Update Array Element by Index

``` js
db.collection.updateOne(
  { name: "John" },
  { $set: { "tags.0": "updated" } }
)
```

------------------------------------------------------------------------

## Upsert (Insert if Not Exists)

``` js
db.collection.updateOne(
  { name: "John" },
  { $set: { age: 30 } },
  { upsert: true }
)
```

------------------------------------------------------------------------

## Find One and Update (returns updated or old document)

### Return Old Document

``` js
db.collection.findOneAndUpdate(
  { name: "John" },
  { $set: { age: 50 } }
)
```

### Return Updated Document

``` js
db.collection.findOneAndUpdate(
  { name: "John" },
  { $set: { age: 50 } },
  { returnNewDocument: true }
)
```

------------------------------------------------------------------------

## \$currentDate --- Set Field to Current Date

``` js
db.collection.updateOne(
  { name: "John" },
  { $currentDate: { lastUpdated: true } }
)
```

------------------------------------------------------------------------

## Combine Multiple Operators

``` js
db.collection.updateOne(
  { name: "John" },
  {
    $set: { city: "Paris" },
    $inc: { visits: 1 },
    $push: { tags: "active" }
  }
)
```
