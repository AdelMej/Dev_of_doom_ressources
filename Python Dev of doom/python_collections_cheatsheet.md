# Python `collections` Module Cheat Sheet (2025)

The `collections` module provides specialized container datatypes
offering alternatives to Python's general-purpose built-in containers
(`dict`, `list`, `set`, `tuple`).

------------------------------------------------------------------------

## **1. `namedtuple`**

Lightweight, immutable objects with named fields.

``` python
from collections import namedtuple

Point = namedtuple("Point", ["x", "y"])
p = Point(10, 20)

p.x  # 10
p.y  # 20
```

------------------------------------------------------------------------

## **2. `deque`**

A fast double-ended queue.

``` python
from collections import deque

dq = deque([1, 2, 3])
dq.append(4)
dq.appendleft(0)
dq.pop()
dq.popleft()
```

**Use cases:** queues, BFS, sliding windows.

------------------------------------------------------------------------

## **3. `Counter`**

Counts occurrences of hashable items.

``` python
from collections import Counter

c = Counter("banana")
c.most_common(2)  # [('a', 3), ('n', 2)]
```

------------------------------------------------------------------------

## **4. `defaultdict`**

Dictionary with default values.

``` python
from collections import defaultdict

dd = defaultdict(int)
dd['a'] += 1
```

Useful for counting, grouping, or avoiding key errors.

------------------------------------------------------------------------

## **5. `OrderedDict`**

A dict that remembers insertion order.\
*⚠️ Deprecated after Python 3.7 since built-in `dict` is ordered.*

``` python
from collections import OrderedDict
```

------------------------------------------------------------------------

## **6. `ChainMap`**

Combine multiple dicts into a single view.

``` python
from collections import ChainMap

config = ChainMap(user_config, system_config, defaults)
```

------------------------------------------------------------------------

## **7. `UserDict`, `UserList`, `UserString`**

Wrapper classes useful for subclassing built-ins safely.

``` python
from collections import UserDict

class MyDict(UserDict):
    pass
```

------------------------------------------------------------------------

## **8. `collections.abc` --- Abstract Base Classes**

These are structural types for type checking and validating containers.

Examples:

``` python
from collections.abc import Iterable, Mapping, Sequence
```

Common ABCs: - `Iterable` - `Iterator` - `Sequence` - `Mapping` -
`MutableMapping` - `Set` - `Callable`

------------------------------------------------------------------------

## Summary Table

  Feature                          Purpose
  -------------------------------- -----------------------------------------
  `namedtuple`                     Immutable objects with named attributes
  `deque`                          Fast queue/stack operations
  `Counter`                        Frequency counting
  `defaultdict`                    Dictionaries with default values
  `OrderedDict`                    Ordered dictionary (legacy)
  `ChainMap`                        Combine multiple dicts
  `UserDict/UserList/UserString`   Base classes for custom containers
  `collections.abc`                Abstract container classes

------------------------------------------------------------------------

Let me know if you want an advanced version or tutorials for each class!
