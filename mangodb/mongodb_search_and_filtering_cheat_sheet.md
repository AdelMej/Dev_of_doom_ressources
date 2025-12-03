# MongoDB Collection Search Cheat Sheet

## Basic Reads

``` js
db.collection.find()                  // list all documents
db.collection.findOne()               // return first matching doc
```

## Filtering

### Equality

``` js
db.collection.find({ field: value })
```

### Comparison Operators

``` js
db.collection.find({ field: { $gt: value } })   // >
db.collection.find({ field: { $lt: value } })   // <
db.collection.find({ field: { $gte: value } })  // >=
db.collection.find({ field: { $lte: value } })  // <=
db.collection.find({ field: { $ne: value } })   // !=
```

### Multiple Conditions

``` js
db.collection.find({ field1: value1, field2: value2 })
```

### Logical

``` js
db.collection.find({ $and: [ {a:1}, {b:2} ] })
db.collection.find({ $or:  [ {a:1}, {b:2} ] })
db.collection.find({ $nor: [ {a:1}, {b:2} ] })
db.collection.find({ field: { $not: { $gt: 5 } } })
```

## String Search

### Exact Match

``` js
db.collection.find({ name: "John" })
```

### Regex (case-insensitive)

``` js
db.collection.find({ name: /john/i })
```

## Arrays

### Contains value

``` js
db.collection.find({ tags: "red" })
```

### Match all values

``` js
db.collection.find({ tags: { $all: ["red", "blue"] } })
```

### Query element by index

``` js
db.collection.find({ "tags.0": "red" })
```

### Match document inside array

``` js
db.collection.find({ items: { $elemMatch: { type: "book", qty: { $gt: 5 } } } })
```

## Subdocuments

``` js
db.collection.find({ "address.city": "Paris" })
db.collection.find({ "stats.age": { $gt: 20 } })
```

## Projection (choose fields)

### Include fields

``` js
db.collection.find({}, { name: 1, age: 1 })
```

### Exclude fields

``` js
db.collection.find({}, { password: 0 })
```

## Sorting

``` js
db.collection.find().sort({ field: 1 })   // ascending
db.collection.find().sort({ field: -1 })  // descending
```

## Limit & Skip

``` js
db.collection.find().limit(5)
db.collection.find().skip(10)
db.collection.find().limit(5).skip(10)
```

## Counting

``` js
db.collection.find(query).count()
```

## Distinct Values

``` js
db.collection.distinct("field")
```

## Aggregation Lite

``` js
db.collection.aggregate([
  { $match: { status: "active" } }
])
```
