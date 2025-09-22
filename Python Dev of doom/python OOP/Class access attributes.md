# Accessing Class Attributes in Instance Methods

When you need to access or modify a **class attribute** from within an instance method, you usually have two main options:

---

## 1. `type(self)`

- **Shorter and more idiomatic in Python**
- Commonly used in tutorials, libraries, and production code
- Instantly tells the reader: “get the class of this object”

```python
class Rectangle:
    number_of_instances = 0

    def __init__(self):
        type(self).number_of_instances += 1

    def __del__(self):
        type(self).number_of_instances -= 1
        print("Bye rectangle...")
```

## 2. self.__class__
More explicit: literally says “give me my class”

Slightly longer, but perfectly valid

Sometimes preferred when teaching beginners

```python
class Rectangle:
    number_of_instances = 0

    def __init__(self):
        self.__class__.number_of_instances += 1

    def __del__(self):
        self.__class__.number_of_instances -= 1
        print("Bye rectangle...")
```

Which One Is Cleaner?
Use type(self) → cleaner, more Pythonic, and widely seen in real-world code.

Use self.__class__ → more explicit, sometimes clearer in teaching contexts.

Both work the same in normal classes — it’s mostly a matter of style and consistency.