# Python Magic Methods Cheat Sheet

## 1. Object Lifecycle
| Method     | When it’s called / Purpose                          |
|------------|----------------------------------------------------|
| `__init__` | Constructor — runs when an instance is created    |
| `__del__`  | Destructor — runs when the instance is deleted    |
| `__new__`  | Creates a new instance (rarely overridden)        |

## 2. String Representations
| Method     | Purpose                                           |
|------------|-------------------------------------------------|
| `__str__`  | Human-readable string — used by `print()`       |
| `__repr__` | Developer-friendly string — for debugging/REPL |

## 3. Comparison Operators
| Method   | Operator / Purpose |
| -------- | ------------------ |
| `__eq__` | `==`               |
| `__ne__` | `!=`               |
| `__lt__` | `<`                |
| `__le__` | `<=`               |
| `__gt__` | `>`                |
| `__ge__` | `>=`               |

## 4. Arithmetic Operators
| Method     | Operator / Purpose                     |
|------------|---------------------------------------|
| `__add__`  | `+`                                   |
| `__sub__`  | `-`                                   |
| `__mul__`  | `*`                                   |
| `__truediv__` | `/`                                |
| `__floordiv__` | `//`                               |
| `__mod__`  | `%`                                   |
| `__pow__`  | `**`                                  |
| `__neg__`  | Unary `-`                             |
| `__pos__`  | Unary `+`                             |
| `__abs__`  | `abs()`                               |

## 5. Container / Sequence Behavior
| Method         | Purpose                             |
|----------------|-------------------------------------|
| `__len__`      | `len(obj)`                          |
| `__getitem__`  | `obj[key]`                           |
| `__setitem__`  | `obj[key] = value`                   |
| `__delitem__`  | `del obj[key]`                       |
| `__iter__`     | Iteration (for loops)                |
| `__next__`     | Next item in iterator                 |
| `__contains__` | `in` operator                         |

## 6. Attribute Access / Introspection
| Method / Attribute | Purpose                                  |
|-------------------|------------------------------------------|
| `__getattr__`     | Called when attribute not found          |
| `__setattr__`     | Called when setting any attribute        |
| `__delattr__`     | Called when deleting an attribute        |
| `__dict__`        | Dictionary of instance attributes        |
| `__class__`       | Returns the class of the instance       |
| `__doc__`         | Object’s docstring                        |
| `__call__`        | Make instance callable like a function   |

## 7. Context Managers
| Method     | Purpose                                |
|------------|----------------------------------------|
| `__enter__` | Enter context (with statement)         |
| `__exit__`  | Exit context                            |

## 8. Others / Fun Stuff
| Method         | Purpose                                  |
|----------------|------------------------------------------|
| `__bool__`     | Boolean value of instance (`if obj`)    |
| `__format__`   | `format(obj, spec)`                      |
| `__copy__`     | Shallow copy support                      |
| `__deepcopy__` | Deep copy support                         |

---

**Notes:**
- Most of these are optional; override only if needed.  
- Python falls back to defaults (usually `object.__repr__`) if you don’t define them.  
- `__str__` falls back to `__repr__` if undefined.  
- Use `__repr__` for unambiguous debugging output; `__str__` for human-friendly output.  
