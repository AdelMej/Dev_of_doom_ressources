# 📘 Python Reference: `isinstance()`, `issubclass()`, and `type()`

## 1. `isinstance(object, classinfo)`
**Purpose:**  
Check whether an object is an instance of a class **or a subclass thereof**.

**Syntax:**
- `isinstance(obj, Class)`
- `isinstance(obj, (Class1, Class2, ...))` → check multiple options

**Notes:**
- Returns `True` for subclasses as well.  
- Use `type(obj) is Class` if you need an **exact match**.

---

## 2. `issubclass(class, classinfo)`
**Purpose:**  
Check whether a class is a subclass of another.

**Syntax:**
- `issubclass(Class, ParentClass)`
- `issubclass(Class, (Parent1, Parent2, ...))` → check multiple options

**Notes:**
- Only works with classes, not instances.  
- Raises `TypeError` if the first argument isn’t a class.

---

## 3. `type(object)`
**Purpose:**  
Get the **exact type (class)** of an object.
 
**Syntax:**
- `type(obj)`

**Notes:**
- Use `type(obj) is Class` for exact type comparison.  
- `type(obj) == Class` also works, but `is` is preferred.  
- Does **not** account for inheritance.

---

## ✅ Quick Comparison

| Check               | Inheritance considered? | Exact match? | Works on |
|----------------------|--------------------------|--------------|----------|
| `isinstance(obj, C)` | ✅ Yes                  | ❌ No        | Objects  |
| `issubclass(C, P)`   | ✅ Yes                  | ❌ No        | Classes  |
| `type(obj) is C`     | ❌ No                   | ✅ Yes       | Objects  |

---

## 🔍 Examples

```python
class A: pass
class B(A): pass

a = A()
b = B()

# isinstance
isinstance(a, A)        # True
isinstance(b, A)        # True (because B inherits A)

# issubclass
issubclass(B, A)        # True
issubclass(A, B)        # False

# type
type(a) is A            # True
type(b) is A            # False (b is exactly B, not A)
```

- `isinstance(obj, C)` → check if **object** `obj` is an instance of class `C` or its subclasses.  
- `issubclass(D, C)` → check if **class** `D` is a subclass of class `C` (or the same class).  
- `type(obj) is C` → check if `obj` is an **exact instance** of class `C` (no subclasses).  
