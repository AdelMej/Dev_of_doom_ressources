## What is Object-Oriented Programming (OOP) and why is it important in Python?

OOP is a way of coding that uses classes
classes are like blueprints to create objects/instances
they can contain public/private attributes
and methods (class specific functions)
example:
```python
class dog:
	def __init__(self, age, race, name):
		self.age = age
		self.race = race
		self.name = name
```
It's important because in python everything is an object
## What does “first-class everything” mean in Python?

In Python, the concept of "first-class everything" means that all objects, regardless of their type, are treated with equal status and can be manipulated in the same ways as any other object.

Every object in Python is an instance of a class
## Explain the difference between a class and an object.

The class is like a blueprint to create an object/instance.
An Object is an instance of that class it is independent of it

## What is an instance in Python?

An instance is a synonym of an object they are the same thing

## How do you define a class in Python?

```python
class My_class:
	pass
```
it is standard to captilize class names
## What is a private attribute and why use it

public attribute: `name`
protected attribute: `_name`
private attribute: `__name`

public can be accessed directly using for example
```python
dog.name
```

private can only be accessed using setters and getters
```python
class Dog:
	def __init__(self):
		self.__name = "lola" # private attribute can't be accessed directly

```
protected works the same as public atttributes but it signals the user that it's an internal variable with a name convention:
`_my_protected_attribute`
start with `_` to signal it's a protected attribute

publics are directly accessible for public use and can be modified directly
you use them for direct access to an attribute without the need of setters and getters

private attributes you use them when you want to hide an attribute internally, only accessible through helper methods (setters and getters)
usable to avoid name conflicts in inheritence (i don't know what that mean don't ask me)
## What is a public attribute and how does it differ from a private one?

publics are directly accessible for public use and can be modified directly

private attributes you use them when you want to hide an attribute internally, only accessible through helper methods (setters and getters)
usable to avoid name conflicts in inheritence (i don't know what that mean don't ask me)

## What is the self keyword used for in Python classes?

self keyword refers to the object itself so when you do self.name it refers to the object name not the class name

## Explain what a method is in Python.

It's a function inside a class
```python
class Dog:
	def my_method(self):
		# do something
		
dog = Dog()
dog.my_method() # use the class method
```

## What is the __init__ method and what is its purpose?

The init method is a constructor to initialize attributes
```python
class Dog:
	def __init__(self, name, age, race):
		self.name = name
		self.age = age
		self.race = race
```

## How do you create a new instance of a class?

```python
instance1 = Dog("lola", 2, "chiwawa")
instance2 = Dog("chewy", 1,"chewbaka")
instance3 = Dog("squidy", 5, "squid")
```

## What is data abstraction and why is it important?

abstraction allows to show only necessary operations, while hidding complexity for the user
it's important because it makes code reusable and intuitive

## What is data encapsulation in Python?

data inside an object and controlled with methods

## Explain information hiding and give an example using a private attribute.

It's for hidding internal attributes to also hide complexity
```python
class Pc:
	def __init__(self):
		self.__cpu_core = 4
```
cpu_core is now a hidden private attribute that doesn't matter for the user but will matter for the class itself
## How do you create a property in Python?

by using the decorator `@property`
```python
class Dog:
	def __init__(self, name):
		self.__name = name
	
	@property
	def name(self):
		return self.__name
```
dog.name
## What is the difference between an attribute and a property in Python?

a property is a special attribute that can only be directly accessed if you setup a @property decorator method
an attribute doesn't do that you'll need to use a getter and setter function for private attributes

## How do you write Pythonic getters and setters?

```python
class Dog:
	def __init__(self, name):
		self.__name = name
	
	@property
	def name(self):
		return self.__name
	
	@name.setter
	def name(self, value):
		self.__name = value
		
	@name.getter
	def name(self):
		return self.__name
```

## How can you dynamically add new attributes to an instance?
```python
dog.new_attribute = 12
```

## How can attributes be bound to a class versus an object?

a class attribute can be defined like this
```python
Class.attribute = ""
```
every instances will inherit it

an object attribute will be defined like this
```python
object.attributes = ""
```
it will only affect that specific object and not it's class

## What is the __dict__ of a class and what does it contain?

```python
>>> print(Dog.__dict__)
{
	'__module__': '__main__',
	'__firstlineno__': 1,
	'__init__': <function Dog.__init__ at 0x7fbbfbfa6020>,
	'name': <property object at 0x7fbbfbbd0e50>,
	'__static_attributes__': ('__name',),
    '__dict__': <attribute '__dict__' of 'Dog' objects>,
    '__weakref__': <attribute '__weakref__' of 'Dog' objects>,
    '__doc__': None, 'age': 14
}
```
the dict of a class contain all it's attributes and methods including the inherited ones
## How does Python search for an attribute in an object or class?

It uses `__dict__` to search for a method or an attribute when you call it using Obj.attribute or Obj.method()

## How do you use the getattr function?

```python
>>> getattr(instance, "attribute_name")
```

## What is the purpose of the __str__ method?

define what is shown when you print an object default to repr if it's not defined
should be user friendly
## What is the purpose of the __repr__ method?

define the official string representation of an object used for debugging
should be developper friendly

## Explain the difference between __str__ and __repr__.

str is user friendly while repr is for debugging and is developper friendly

## How do you implement a method to compute the area of a square?

```python
class Square:
	def __init__(self, size):
		if not isinstance(size, int):
			raise TypeError("size must be an integer")

		self.__size = size
	
	def area(self):
		return self.__size**2
```

## How do you implement a method to print a square using the # character?

```python
class Square:
	def __init__(self, size):
		if not isinstance(size, int):
			raise TypeError("size must be an integer")

		self.__size = size

	def my_print(self):
		for _ in range(self.__size):
			for _ in range(self.__size):
				print("#", end="")
			print()

```

## What is a tuple and how can it be used to define a position attribute for a square?

A tuple is a sequence that's immutable (cannot be modified)

```python
class Square:
	def __init__(self, size=0, position=(0, 0)):
		if not isinstance(size, int):
			raise TypeError("size must be an integer")

		if not isinstance(position, tuple):
			raise TypeError("position must be a tuple")

		self.__size = size
		self.position = position
```

position = (height_start, width_start)
## How do you implement comparison methods (`<`, `<=`, `==`, `!=`, `>`, `>=`) for a class?

| Method   | Operator / Purpose |
| -------- | ------------------ |
| `__eq__` | `==`               |
| `__ne__` | `!=`               |
| `__lt__` | `<`                |
| `__le__` | `<=`               |
| `__gt__` | `>`                |
| `__ge__` | `>=`               |
```python
class Square:
	def __init__(self, size=0, position=(0, 0)):
		if not isinstance(size, int):
			raise TypeError("size must be an integer")

		if not isinstance(position, tuple):
			raise TypeError("position must be a tuple")

		self.__size = size
		self.position = position
	
	def __eq__(self, other):
		return self.__size == other.__size
		
	def __ne__(self, other):
		return self.__size != other.__size
	
	def __lt__(self, other):
		return self.__size < other.__size
		
	def __le__(self, other):
		return self.__size <= other.__size
		
	def __gt__(self, other):
		return self.__size > other.__size
		
	def __ge__(self, other):
		return self.__size >= other.__size
```
## How do you implement a singly linked list and ensure nodes are inserted in sorted order?

```python
class Node:
	def __init__(self):
		self.data = None
		self.next = None
		
class Linked_list:
	def __init__(self):
		self.head = None
		
	def add_node(self, data):
		current = self.head
		if current is None:
			new_node = Node()
			new_node.data = data
			self.head = new_node
			return
		
		if current.data > data:
			new_node = Node()
			new_node.data = data
			new_node.next = current
			self.head = new_node
			return
		
		while current.next is not None:
			if current.next.data > data:
				break
			current = current.next
		new_node = Node()
		new_node.data = data
		new_node.next = current.next
		current.next = new_node

	
	def list_print(self):
		node = self.head
		while node != None:
			print(node.data)
			node = node.next

```
i made sure they were inserted in ascending order also thanks for stack overflow because that one was just...

```python
>>> ll = Linked_list()
>>> ll.add_node(23)
>>> ll.add_node(12)
>>> ll.add_node(15)
>>> ll.add_node(5)
>>> ll.add_node(42)
>>> ll.add_node(63)
>>> ll.list_print()
5
12
15
23
42
63
```