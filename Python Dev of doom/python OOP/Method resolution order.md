# 🔎 Python Reference: `__mro__` (Method Resolution Order)

## What is `__mro__`?
- `__mro__` is a special attribute of a class that shows the **Method Resolution Order**.  
- It is a tuple listing the class itself, its base classes, and ultimately `object`.  
- Python uses this order to decide **where to look for attributes and methods**.

---

## Key Points
- **Inheritance resolution**: When you access an attribute or method, Python searches the classes in the order given by `__mro__`.  
- **`issubclass()`** uses `__mro__` internally: a class is considered a subclass if the parent appears anywhere in its MRO.  
- Supports **multiple inheritance** by computing a consistent ordering with the C3 linearization algorithm.  

---

## Syntax
```python
SomeClass.__mro__
```

---

### Example
```python
class A: pass
class B(A): pass
class C(B): pass

print(C.__mro__)
# (<class '__main__.C'>, <class '__main__.B'>, <class '__main__.A'>, <class 'object'>)

issubclass(C, A)   # True, because A is in C.__mro__
```
---
## Multiple Inheritance Example
```python
class X: pass
class Y: pass
class Z(X, Y): pass

print(Z.__mro__)
# (<class '__main__.Z'>, <class '__main__.X'>, <class '__main__.Y'>, <class 'object'>)
```

Here, Python checks Z, then X, then Y, and finally object.

---
## ✅ Summary

- `__mro__` = Method Resolution Order → determines lookup path.
- Directly used by Python to handle inheritance and by f
unctions like `issubclass()`.
- Critical for understanding multiple inheritance and why Python resolves methods in a particular order.