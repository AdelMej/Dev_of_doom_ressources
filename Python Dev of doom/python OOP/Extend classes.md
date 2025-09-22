# 🧩 Extending Classes in Python
## What is extending?

- In Python, extending a class means creating a child class (subclass) that inherits attributes and methods from a parent class (superclass).

- The child class can:
	- Use parent methods and attributes.
	- Override parent methods.
	- Add new methods and attributes.

### Syntax
```python
class Parent:
    def greet(self):
        print("Hello from Parent")

class Child(Parent):
    def greet(self):
        print("Hello from Child")  # overrides parent


- Child automatically inherits all of Parent’s methods and attributes unless overridden.
```
---
## Using super()
- super() allows the child class to call methods from the parent (common in __init__).
```python
class Parent:
    def __init__(self, name):
        self.name = name

class Child(Parent):
    def __init__(self, name, age):
        super().__init__(name)  # call Parent.__init__
        self.age = age
```
## Attribute Access in Subclasses

- Public attributes: accessed normally via self.attr.
- Protected attributes (`_attr`): accessible by convention.
- Private attributes (`__attr`): name-mangled (_Parent__attr) — can be accessed, but not recommended.

### Overriding Methods
```python
class Parent:
    def greet(self):
        print("Hello from Parent")

class Child(Parent):
    def greet(self):
        super().greet()  # call parent method
        print("Hello from Child")  # extend behavior
```

- You can replace the parent method entirely or extend it by calling super().method().

## Key Points

- Inheritance allows code reuse and polymorphism.
- Python supports multiple inheritance: a class can extend multiple parents.
- The MRO (method resolution order) determines which parent’s method is called first.

### Multiple Inheritance Example
```python
class A:
    def hello(self):
        print("Hello from A")

class B:
    def hello(self):
        print("Hello from B")

class C(A, B):
    pass

c = C()
c.hello()  # ✅ Hello from A (follows MRO)
```


### ✅ TL;DR:

- Extend a class with class Child(Parent):
- Use super() to access parent methods, especially `__init__`
- Override or add methods as needed
- Be aware of private attributes and MRO in multiple inheritance