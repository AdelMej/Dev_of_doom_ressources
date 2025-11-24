# Python Typing Cheat Sheet (2025)

## Basic Types

``` python
x: int
y: float
name: str
flag: bool
```

## Lists, Tuples, Sets, Dicts

### Python 3.9+

``` python
numbers: list[int]
names: list[str]
matrix: list[list[int]]

coords: tuple[int, int]
unique: set[str]

ages: dict[str, int]
```

### Older Python

``` python
from typing import List, Tuple, Set, Dict
numbers: List[int]
coords: Tuple[int, int]
unique: Set[str]
ages: Dict[str, int]
```

## Optional

``` python
age: int | None
```

## Union

``` python
value: int | float
```

## Functions

``` python
def add(a: int, b: int) -> int:
    return a + b
```

## Typed Arguments With Defaults

``` python
def greet(name: str = "Guest") -> None:
    print(f"Hello {name}")
```

## Any

``` python
from typing import Any
data: Any
```

## Type Aliases

``` python
UserId = int
Vector = list[float]
```

## TypedDict

``` python
from typing import TypedDict
class User(TypedDict):
    name: str
    age: int
```

## Generics

``` python
from typing import TypeVar, Generic
T = TypeVar("T")
class Box(Generic[T]):
    def __init__(self, value: T):
        self.value = value
```

## Callable

``` python
from typing import Callable
Operation = Callable[[int, int], int]
```

## Example

``` python
def sum_list(input_list: list[float]) -> float:
    return sum(input_list)
```
