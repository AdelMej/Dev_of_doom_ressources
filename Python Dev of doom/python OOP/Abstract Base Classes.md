# 🧩 Abstract Base Classes (ABC) in Python

### What is an ABC?

- An **abstract base class** defines a **blueprint** for other classes.
- You can’t **instantiate** it directly.
- It may contain **abstract methods** (methods with no implementation).
- Subclasses **must implement** all abstract methods, or they’ll also be abstract.

---

### Importing ABC Tools

```python
from abc import ABC, abstractmethod
```

- `ABC`: base class to define abstract classes.
- `@abstractmethod`: decorator that marks methods as abstract.

---

### Defining an Abstract Class

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

    @abstractmethod
    def perimeter(self):
        pass
```

- `Shape` cannot be instantiated (`Shape()` → ❌ `TypeError`).
- Any subclass **must** implement `area()` and `perimeter()`.

---

### Implementing a Subclass

```python
class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)
```

- ✅ `Rectangle` is instantiable because it implemented all abstract methods.

---

### Key Points

- ABCs let you **enforce an interface** across subclasses.
- You can mix abstract and concrete methods:
```python
class Animal(ABC):
    @abstractmethod
    def speak(self):
        pass

    def sleep(self):
        return "zzz..."
```
- You can also declare **abstract properties** and **classmethods** with the same decorator.

---

### Abstract Property Example

```python
class Person(ABC):
    @property
    @abstractmethod
    def name(self):
        pass

```
---

### Why use ABCs?

- Ensures subclasses follow a **consistent contract**.
- Useful for frameworks, libraries, or large projects where multiple devs implement subclasses.
- 
---

✅ **Summary:**

- `ABC` → base class for abstract classes.
- `@abstractmethod` → defines methods subclasses must implement.
- Can’t instantiate abstract classes.
- Subclasses missing any abstract method → still abstract.