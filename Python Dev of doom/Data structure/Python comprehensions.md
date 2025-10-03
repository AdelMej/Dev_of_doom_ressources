# 🐍 Python Comprehensions Cheat Sheet
## 1. List Comprehension

👉 Build a list in one line.
```python
# Basic
[expr for item in iterable]

# With condition
[expr for item in iterable if condition]

# Example
squares = [x*x for x in range(5)]        # [0, 1, 4, 9, 16]
evens   = [x for x in range(10) if x%2==0]  # [0, 2, 4, 6, 8]
```

---
## 2. Set Comprehension

👉 Same as list, but creates a set (no duplicates).
```python
{x*x for x in range(5)}   # {0, 1, 4, 9, 16}
```

---
## 3. Dictionary Comprehension

👉 Build dicts on the fly.

```python
{key_expr: value_expr for item in iterable}

# Example
squares = {x: x*x for x in range(5)}   # {0:0, 1:1, 2:4, 3:9, 4:16}
```
---
## 4. Nested Comprehensions

👉 Comprehension inside another.
```python
# Flatten a list of lists
matrix = [[1,2],[3,4],[5,6]]
flat = [num for row in matrix for num in row]  # [1,2,3,4,5,6]
```

---
## 5. With Conditionals

👉 Inline if for filtering or choosing values.

```python
# Filtering
[x for x in range(10) if x%2 == 0]   # evens

# Inline if/else
["even" if x%2==0 else "odd" for x in range(5)]
# ['even', 'odd', 'even', 'odd', 'even']
```

---
## 6. Generator Comprehension

👉 Like list comprehension but with () → lazy evaluation.
```python
(x*x for x in range(5))   # generator object
sum(x*x for x in range(5))  # 30
```

| Type      | Brackets | Example                     |
| --------- | -------- | --------------------------- |
| List      | `[]`     | `[x*x for x in range(5)]`   |
| Set       | `{}`     | `{x*x for x in range(5)}`   |
| Dict      | `{k:v}`  | `{x:x*x for x in range(5)}` |
| Generator | `()`     | `(x*x for x in range(5))`   |
