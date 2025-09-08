# 🐍 Python Guide: Dictionaries & Sets
## 1. Dictionaries (dict)

A dictionary stores key → value pairs. Keys are unique; values can be anything.

## Creating Dictionaries

### Empty dict
```python
my_dict = {}
```

### With values
```python
person = {
    "name": "Alice",
    "age": 25,
    "city": "Paris"
}
```

## Accessing Values
```python
print(person["name"])       # Alice
print(person.get("age"))    # 25
print(person.get("job", "N/A"))  # "N/A" if key doesn't exist
```

## Adding & Updating
```python
person["age"] = 26           # update
person["job"] = "Engineer"   # add
```

## Removing Items
```python
person.pop("city")      # removes "city"
del person["age"]       # also removes
person.clear()          # empties dictionary
```

## Iterating
```python
for key in person:
    print(key)           # keys

for value in person.values():
    print(value)         # values

for key, value in person.items():
    print(f"{key}: {value}")  # key-value pairs
```

## Dictionary Comprehension
```python
squares = {x: x**2 for x in range(5)}
print(squares)  # {0:0, 1:1, 2:4, 3:9, 4:16}
```

# 2. Sets (set)

A set is an unordered collection of unique items.

## Creating Sets
```python
numbers = {1, 2, 3, 3, 2}
print(numbers)  # {1, 2, 3} (duplicates removed)

empty_set = set()  # {} would create a dict, not a set
```

### Adding & Removing
```python
numbers.add(4)
numbers.remove(2)     # KeyError if not found
numbers.discard(10)   # safe remove
```

### Set Operations
```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)  # Union → {1,2,3,4,5}
print(a & b)  # Intersection → {3}
print(a - b)  # Difference → {1,2}
print(a ^ b)  # Symmetric Difference → {1,2,4,5}
```

# 3. Key Takeaways

Dictionaries: fast key-based lookup, store key → value.

Sets: store unique items, support math-like operations.

Dict ≈ Map (Python’s map data structure).

Use comprehensions to create them quickly.