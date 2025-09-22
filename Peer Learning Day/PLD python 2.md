# **Python - Data Structures: Lists, Tuples**

- What are lists and how to use them?
	In python a list is a data structure that can contain any object and is mutable

	```python
	my_list = [1, 2, "abcd", [1, 2, 3]]
	```
	usage:
	```python
	my_list[0] #indexing
	
	for i in my_list:
		# do something with i
	
	my_list[2:3] # slicing ["abcd", [1, 2, 3]]
	```
- What are the differences and similarities between strings and lists?
#### difference 
list can contain any types string can't, string has more methods for formating strings, list has only list specific methods, the declaration is different

#### similarities
you can use them in python, has the same max size, can both be indexed, can both be sliced, 

- What are the most common methods of lists and how to use them?
append:
```python
my_list = [1, 2, 3]
my_list.append(4)
print(my_list)
[1, 2, 3, 4]
```
remove:
```python
my_list = [1, 2, 3, 4]
my_list.remove(2)
print(my_list)
[1, 3, 4]
```
sort:
```python
>>> my_list = [14, 3, 24, 5, 56]
>>> my_list.sort()
>>> print(my_list)
[3, 5, 14, 24, 56]
```

- How to use lists as stacks and queues?
for a stack(LIFO):
use append and pop
```python
>>> stack = []
>>> stack.append(  1)
>>> stack.append(56)
>>> stack.append(26)
>>> print(stack)
[1, 56, 26]
>>> top = stack.pop()
>>> print(top)
26
>>> print(stack)
[1, 56]
```
for queues(FIFO):
use append and pop(0)
```python
>>> queue = []
>>> queue.append("aya")
>>> queue.append("noooooo")
>>> queue.append("who toucha some spaghet")
>>> print(queue)
['aya', 'noooooo', 'who toucha some spaghet']
>>> next = queue.pop(0)
>>> print(next)
aya
>>> print(queue)
['noooooo', 'who toucha some spaghet']
```

- What are list comprehensions and how to use them?
list comprehension are instruction you can give to create a list for example:
```python
>>> my_list = [x for x in range(1, 11, 2)]
>>> print(my_list)
[1, 3, 5, 7, 9]
```
- What are tuples and how to use them?
tuples are immutable lists you cannot modify them once set
```python
>>> my_tuple = (1, 2)
>>> print(tuple)
(1, 2)
```
- When to use tuples versus lists?
you use tuples when you want the values to be constant and not mutable you use list when you plan to have a variable amounts of items or change values

- What is a sequence?
sequences are iterable object they can store multiple variables and types, example : list, dic, tuple, set

- What is tuple packing?
it's a syntax to deckare a tuble
```python
>>> packed_tuple = 1, 2, 3
>>> print(packed_tuple)
(1, 2, 3)
```
- What is sequence unpacking?
a sequence unpacking consist of using multiple variables to store the conten of a tuple
```python
>>> packed_tuple = 1, 2, 3
>>> x, y, z = packed_tuple
>>> print(x)
1
>>> print(y)
2
>>> print(z)
3
>>> print(packed_tuple)
(1, 2, 3)
```
- What is the del statement and how to use it?
the del statement is for deleting values at a certain index
```python
>>> my_list = [1, 2, 3]
>>> del my_list[2]
>>> print(my_list)
[1, 2]
```
- How do you safely access elements in a list without using try/except?
with a for loop iteration
```python
>>> my_list = ["its but a scratch", 42, "messiah"]
>>> for i in my_list:
...     print(i)
...
its but a scratch
42
messiah
```
- How do you print a list of integers using str.format()?
```python
>>> my_list = [1, 2, 3, 4, 5]
>>> for i in my_list:
...     print("{:d}".format(i))
...
1
2
3
4
5
>>>
```
- How do you replace elements in a list and handle invalid indices?
```python
>>> print(my_list)
[1, 2, 3, 4, 5]
>>> my_list[2] = 45
>>> print(my_list)
[1, 2, 45, 4, 5]
```
invalid index
```python
>>> try:
...     my_list[56]
... except IndexError:
...     print("Invalid index")
...
Invalid index
```
- How do you work with nested data structures like matrices?
you use nested loops one for the external list and one for the inside lists
```python
>>> for row in matrix:
...     for cols in row:
...         print(cols, end ="")
...     print()
...
123
123
```
**Python - More Data Structures: Set, Dictionary**

- Why Python programming is awesome
it's not, c is better!

- What are sets and how to use them
a set cannot have duplicate values, cannot be indexed 
 it allows for set operations:
 ```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a | b)  # Union → {1,2,3,4,5}
print(a & b)  # Intersection → {3}
print(a - b)  # Difference → {1,2}
print(a ^ b)  # Symmetric Difference → {1,2,4,5}
 ```
 
- What are the most common methods of set and how to use them
```python
numbers.add(4)
numbers.remove(2)     # KeyError if not found
numbers.discard(10)   # safe remove
```
- When to use sets versus lists
when you want to use set specific operations avoid duplicate values and quick search

- How to iterate into a set
```python
for i in my_set:
	print(i)
```
- What are dictionaries and how to use them
it's a data structure that associates a key and a value
```python
my_dic = {"key": "value"}
print(my_dic["key"])
```

- When to use dictionaries versus lists or sets
when you want a key associated with a value

- What is a key in a dictionary
a key is a way to identify a value for example you can set the key to "age" and the age value

- How to iterate over a dictionary
```python
for i in my_dic:
	print(i)
```
- What is a lambda function
it's a one liner function
```python
custom_print = lambda x: print(x*2)
```

- What are the map, reduce and filter functions
map is for using a function per element in a given sequence
```python
map(lambda x: x*x, my_list)
```
reduce applies operation on a pair of values in a list
it does it by cumulating each value and applying the function to the result and the next value
```python
from functools import reduce

reduce(lambda x, y: x + y, my_list)
```
filter use a lambda function for filtering value
```python
filter(lambda x: x > 0, my_list)
```
- How to work with matrix operations using nested data structures
```python
for row in matrix:
	for clos in row:
		# do soething
```
- How to implement set operations like intersection and symmetric difference

- How to find maximum values in dictionaries without using built-in functions

- How to use functional programming concepts with map to transform data

**Python - Exceptions**

- Why Python programming is awesome
- What's the difference between errors and exceptions
- What are exceptions and how to use them
- When do we need to use exceptions
- How to correctly handle an exception
- What's the purpose of catching exceptions
- How to raise a builtin exception
- When do we need to implement a clean-up action after an exception
- How to use try/except blocks to handle list index errors safely
- How to validate data types using exception handling instead of type checking
- How to handle multiple exception types in a single function
- How to use finally blocks for cleanup operations
- How to print error messages to stderr using exception handling
- How to execute functions safely and handle any potential exceptions
- How to convert bytecode logic into proper exception handling patterns

**Python - Test-driven development**

- Why Python programming is awesome
- What's an interactive test
- Why tests are important
- How to write Docstrings to create tests
- How to write documentation for each module and function
- What are the basic option flags to create tests
- How to find edge cases
- How to validate input parameters and raise appropriate exceptions
- How to write comprehensive test cases for mathematical operations
- How to test functions that modify or validate data structures like matrices
- How to use unittest module for more complex test scenarios
- How to test string manipulation and formatting functions
- How to handle edge cases like empty inputs, invalid types, and boundary conditions
- How to write tests that cover both normal operations and error conditions