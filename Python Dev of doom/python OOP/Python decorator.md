# Python Method Decorators Cheat Sheet

Python provides several built-in decorators that change the way methods behave inside a class.  

---

## 1. `@staticmethod`

- Does **not** take `self` or `cls`
- Behaves like a plain function, but lives inside the class’s namespace
- Use when the method **doesn’t depend on instance or class state**

```python
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b

print(MathUtils.add(2, 3))  # 5
```

## 2. @classmethod
Takes cls as the first argument (the class itself)

Can access/modify class attributes

Use when the method works with the class rather than specific instances

```python
class Rectangle:
    number_of_instances = 0

    def __init__(self):
        type(self).number_of_instances += 1

    @classmethod
    def get_instance_count(cls):
        return cls.number_of_instances

r1 = Rectangle()
r2 = Rectangle()
print(Rectangle.get_instance_count())  # 2
```

## 3. @property
Turns a method into an attribute

Provides getter functionality for computed or protected values

Often combined with setters and deleters

```python
class Rectangle:
    def __init__(self, width, height):
        self._width = width
        self._height = height

    @property
    def area(self):
        return self._width * self._height

r = Rectangle(3, 4)
print(r.area)  # 12 (looks like an attribute, but is computed)
```

## 4. @`<property>`.setter
Defines a setter for a property

Allows validation or transformation before assigning values

```python
class Rectangle:
    def __init__(self, width):
        self._width = width

    @property
    def width(self):
        return self._width

    @width.setter
    def width(self, value):
        if value < 0:
            raise ValueError("Width must be >= 0")
        self._width = value
```

## 5. @`<property>`.deleter
Defines a deleter for a property

Allows custom cleanup logic

```python
class Rectangle:
    def __init__(self, width):
        self._width = width

    @property
    def width(self):
        return self._width

    @width.deleter
    def width(self):
        print("Deleting width...")
        del self._width
```

| Decorator             | First Argument | Description / Use Case                                      |
|-----------------------|----------------|-------------------------------------------------------------|
| `@staticmethod`       | None           | Utility function tied to the class namespace; does not access `self` or `cls` |
| `@classmethod`        | `cls`          | Works with the class itself; can access/modify class attributes |
| `@property`           | `self`         | Makes a method act like a read-only attribute              |
| `@<property>.setter`  | `self`         | Defines a setter for a property                            |
| `@<property>.deleter` | `self`         | Defines a deleter for a property                           |
