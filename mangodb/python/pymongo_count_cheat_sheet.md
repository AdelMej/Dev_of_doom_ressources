# PyMongo Count Cheat Sheet

## 1. Count All Documents
```python
collection.count_documents({})
```

## 2. Count With One Filter
```python
collection.count_documents({"method": "GET"})
```

## 3. Count With Multiple Filters
```python
collection.count_documents({
    "method": "GET",
    "path": "/status"
})
```

## 4. Count With Operators
### Greater than, less than, etc.
```python
collection.count_documents({"size": {"$gt": 1000}})
```

### OR condition
```python
collection.count_documents({"$or": [
    {"method": "GET"},
    {"method": "POST"}
]})
```

### AND condition (implicit)
```python
collection.count_documents({
    "method": "GET",
    "status": 200
})
```

## 5. Count Using Regex
```python
collection.count_documents({"path": {"$regex": "^/api"}})
```

## 6. Count Using IN
```python
collection.count_documents({"method": {"$in": ["GET", "POST"]}})
```

## 7. Count Using NOT
```python
collection.count_documents({"method": {"$ne": "GET"}})
```
