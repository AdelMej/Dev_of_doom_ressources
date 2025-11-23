# Python Mutability Cheat Sheet (Complete)

## Immutable Types

-   **int**
-   **float**
-   **bool**
-   **str**
-   **tuple**
-   **frozenset**
-   **bytes**
-   **NoneType**

### Operations That Create a New Object

-   `a + b` for strings, tuples
-   `replace`, `upper`, etc. for strings
-   arithmetic (`+`, `-`, `*`, `/`)
-   tuple concatenation
-   slicing immutable objects

------------------------------------------------------------------------

## Mutable Types

-   **list**
-   **dict**
-   **set**
-   **bytearray**
-   **custom class instances**
-   **most collections from `collections` module**

### In‑Place Operations (do NOT create new object)

-   `list.append(x)`
-   `list.extend(iterable)`
-   `list.sort()`
-   `list.reverse()`
-   `list[i] = x`
-   `dict[key] = value`
-   `dict.update(...)`
-   `set.add(x)`
-   `set.remove(x)`
-   `set.update(...)`

### Operations That Create a NEW Object (not in-place)

-   `new_list = old_list + [x]`
-   `subset = set1 | set2` (union operator)
-   `copy = list(old_list)` (explicit copy)
-   any slicing: `a[:]`

------------------------------------------------------------------------

## Special Case: `+=`

### Mutable objects → in place

``` python
l = [1, 2]
id(l)
l += [3]
id(l)  # SAME ID
```

### Immutable objects → new object

``` python
t = (1, 2)
id(t)
t += (3,)
id(t)  # DIFFERENT ID
```

------------------------------------------------------------------------

## C Analogy

-   Immutable = allocated once, "rebinding" creates new object (like
    allocating new struct)
-   Mutable = modifying existing memory (like modifying struct fields)

------------------------------------------------------------------------

## Aliasing Pitfall

``` python
l1 = [1, 2]
l2 = l1
l1.append(3)
# l2 is ALSO modified
```

To avoid:

``` python
l2 = l1[:]  # or list(l1)
```

------------------------------------------------------------------------

## Safe Copy Patterns

### Shallow Copies

    new = old[:]        # lists
    new = list(old)
    new = old.copy()    # dict, set

### Deep Copies

    from copy import deepcopy
    new = deepcopy(old)

------------------------------------------------------------------------

## Empty Tuple / Small Integer Interning

-   CPython interns:
    -   empty tuples `()`
    -   small integers \[-5, 256\]
-   They always refer to the same object in memory.

------------------------------------------------------------------------

## Final Rule of Thumb

-   **If it's immutable: any change makes a NEW object.**
-   **If it's mutable: most changes happen IN PLACE.**
