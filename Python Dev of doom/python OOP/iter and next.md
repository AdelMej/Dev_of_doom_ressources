# Python Iterators & `next()`

## Core Concepts

- **Iterator protocol** in Python:  
  An object is an *iterator* if it implements:
  - `__iter__()` → returns the iterator object itself
  - `__next__()` → returns the next item, or raises `StopIteration` when no more items remain

- **Iterable**:  
  An object that implements `__iter__()` (returns an iterator).  
  Examples: `list`, `tuple`, `str`, `dict`, `set`, etc.

---

## Functions

### `iter(obj)`
- Returns an **iterator** for the given iterable.  
- If `obj` is already an iterator, it just returns itself.

```python
nums = [1, 2, 3]
it = iter(nums)  # get iterator
```
### next(iterator, default)

- Fetches the next item from an iterator.
- Raises StopIteration if there are no items left.
- If default is provided, it returns that instead of raising an error.

```python
it = iter([10, 20])

print(next(it))      # 10
print(next(it))      # 20
print(next(it, -1))  # -1 (default, since iterator is exhausted)
```

---

### Looping vs. Manual next()
- for loops automatically call iter() and next() for you:

```python
for x in [1, 2, 3]:
    print(x)  # under the hood: iter(...) + next(...)
```
is equivalent to:
```python
it = iter([1, 2, 3])
while True:
    try:
        x = next(it)
        print(x)
    except StopIteration:
        break
```
---

### Custom Iterator Example
```python
class CountedIterator:
    def __init__(self, obj):
        self._iterator = iter(obj)
        self._count = 0

    def __next__(self):
        value = next(self._iterator)  # raises StopIteration when done
        self._count += 1
        return value

    def get_count(self):
        return self._count
```

Usage:

```python
it = CountedIterator([1, 2, 3])
print(next(it))  # 1
print(next(it))  # 2
print(it.get_count())  # 2
```
