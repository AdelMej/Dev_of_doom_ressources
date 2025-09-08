# 1. Variables & Data Types
## Numbers
```python
x = 5       # int
y = 3.14    # float
```
## Strings
```python
name = "Bazzite"
```

## Boolean
```python
flag = True
```

## List
```python
my_list = [1, 2, 3]
 ```
## Tuple (immutable)
```python
my_tuple = (1, 2, 3)
```
## Dictionary
```python
my_dict = {"key": "value"}
```
## Set
```python
my_set = {1, 2, 3}
```
# 2. Control Flow
## If / Else
```python
x = 10
if x > 5:
    print("x is greater than 5")
elif x == 5:
    print("x equals 5")
else:
    print("x is less than 5")
```
## Loops
### For loop
```python
for i in range(5):  # 0 to 4
    print(i)
```

### While loop
```python
i = 0
while i < 5:
    print(i)
    i += 1
```

# 3. Functions
```python
def greet(name):
    return f"Hello, {name}!"

print(greet("Bazzite"))
```

Functions can have default arguments, variable-length arguments (*args, **kwargs).

# 4. Lists
## Creating & Accessing
```python
my_list = [1, 2, 3, 4]
print(my_list[0])  # 1
print(my_list[-1]) # 4
```

## Adding / Removing
```python
my_list.append(5)     # Add at the end
my_list.insert(2, 99) # Insert at index
my_list.remove(3)     # Remove value
my_list.pop()         # Remove last element
```

## Iterating
### Normal
```python
for item in my_list:
    print(item)
```

### Reverse
```python
for item in reversed(my_list):  # GOAT method
    print(item)
```

## Or using slicing
```python
for item in my_list[::-1]:
    print(item)
```
## Or using reverse indices
```python
for i in range(len(my_list)-1, -1, -1):
    print(my_list[i])
```

### List Comprehension
```python
squared = [x**2 for x in my_list]
```
# 5. Dictionaries
```python
my_dict = {"name": "Bazzite", "age": 20}
```
## Access
```python
print(my_dict["name"])
```
## Iterate
```python
for key, value in my_dict.items():
    print(key, value)
```

# 6. Strings
```python
s = "hello world"

print(s.upper())    # HELLO WORLD
print(s.lower())    # hello world
print(s.replace("hello", "hi"))
print(s.split())    # ['hello', 'world']
```

# 7. Modules & Imports
```python
import math
print(math.sqrt(16))

from random import randint
print(randint(1, 10))
```

# 8. Pythonic Tricks
## Multiple assignment
```python
a, b = 1, 2
```

## Enumerate
```python
for i, val in enumerate(my_list):
    print(i, val)
```

## Reversed + Enumerate
```python
for i, val in enumerate(reversed(my_list)):
    print(i, val)  # i = 0 for last element
```

# 9. File Handling
```python
with open("file.txt", "r") as f:
    data = f.read()

with open("file.txt", "w") as f:
    f.write("Hello Python!")
```

# 10. Exceptions
```python
try:
    x = 1 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
finally:
    print("Always runs")
```