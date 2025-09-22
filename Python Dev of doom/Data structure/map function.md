# 🐍 Python Guide: The `map()` Function

---

## 1. What is `map()`?

`map()` applies a **function** to each item in an **iterable** (like a list, tuple, or set) and returns a `map` object (which you can turn into a list, tuple, or set).

**Syntax:**
```python
map(function, iterable)
```
---

## 2. Basic Example
```python
nums = [1, 2, 3, 4]  
# double each number
result = map(lambda x: x * 2, nums)  
print(list(result))   # [2, 4, 6, 8]
```

---

## 3. With Built-in Functions

You can pass any function, not just lambdas.
```python
words = ["hello", "world", "python"] 
result = map(str.upper, words)  
print(list(result))   # ['HELLO', 'WORLD', 'PYTHON']`
```

---

## 4. With Multiple Iterables

If you pass more than one iterable, `map()` applies the function to **each group of elements** in parallel.

```python
a = [1, 2, 3] 
b = [10, 20, 30]  
result = map(lambda x, y: x + y, a, b) 
print(list(result))   # [11, 22, 33]`
```
---

## 5. Converting the `map` object

Since `map()` returns a special object, you usually convert it:
```python
nums = [1, 2, 3]  
print(list(map(lambda x: x**2, nums)))   # [1, 4, 9] 
print(tuple(map(str, nums)))             # ('1', '2', '3') 
print(set(map(lambda x: x % 2, nums)))   # {0, 1}`
```
---

## 6. Map vs List Comprehension

Most Python devs prefer **list comprehensions** because they’re more readable:
```python
nums = [1, 2, 3]  
# Using
map result1 = list(map(lambda x: x**2, nums))  
# Using list comprehension 
result2 = [x**2 for x in nums]  
print(result1)  # [1, 4, 9] 
print(result2)  # [1, 4, 9]`
```
👉 Both do the same thing, but comprehensions are cleaner. Still, `map()` is useful when you already have a named function or multiple iterables.

---

## 7. When to Use `map()`

- When applying an **existing function** to a list.
    
- When using **multiple iterables**.
    
- When writing **functional-style code**.
    

---