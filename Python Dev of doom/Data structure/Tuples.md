# Python Tuples – Complete Guide

A **tuple** is an **immutable, ordered collection** of elements. Think of it like a list, but you **cannot change it** after creation.

---

## 1. Creating Tuples
```python
Empty tuple t1 = ()  # Single-element 
tuple (note the comma!) t2 = (5,)   
t3 = (1, 2, 3)  # Multiple elements
t4 = (1, "hello", 3.14) # Mixed types 
t5 = tuple([1, 2, 3]) # Using tuple() constructor
```


---

## 2. Accessing Elements
```python
t = (10, 20, 30, 40)  
print(t[0])  # 10 (first element) 
print(t[-1]) # 40 (last element) 
print(t[1:3]) # (20, 30) slicing
```

---

## 3. Tuple Operations
```python
t1 = (1, 2) 
t2 = (3, 4)  
t3 = t1 + t2   # Concatenation (1, 2, 3, 4)
t4 = t1 * 2    # Repetition (1, 2, 1, 2)
print(2 in t1) # Membership
```
---

## 4. Tuple Methods

Tuples have **very few built-in methods** because they are immutable:
```python
t = (1, 2, 2, 3, 4) 
print(t.count(2))  # 2 → counts occurrences 
print(t.index(3))  # 3 → index of first occurrence
```
---

## 5. Unpacking Tuples
```python
t = (1, 2, 3) 
a, b, c = t 
print(a, b, c)  # 1 2 3  
# Ignoring elements 
a, _, c = t 
print(a, c)     # 1 3  
# Swapping variables 
x, y = 5, 10 
x, y = y, x
```

---

## 6. Advantages of Tuples

- **Immutable** → safer for fixed collections
    
- **Hashable** → can be used as dictionary keys
    
- **Faster** than lists for iteration and access
    

---

## 7. When to Use Tuples

- Fixed-size collections (e.g., coordinates `(x, y)`)
    
- Returning multiple values from a function
    
- Keys in dictionaries