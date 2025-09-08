# Deleting Values from a List in Python

Python provides multiple ways to remove elements from a list.  
Some work by **index**, some by **value**.

---

## 1. Using `del` (by index)

Deletes the element at the specified index **in place**.

```python
my_list = [10, 20, 30, 40]
del my_list[2]   # removes element at index 2 (30)
print(my_list)   # [10, 20, 40]
```
## 2. Using Slicing (create a new list)
Rebuilds the list without the element at that index.

```python
my_list = [10, 20, 30, 40]
index = 2
my_list = my_list[:index] + my_list[index+1:]
print(my_list)   # [10, 20, 40]
```

## 3. Using remove() (by value)
Removes the first occurrence of a value.
⚠️ Only works if you know the value itself, not just the index.

```python
my_list = [10, 20, 30, 40]
my_list.remove(30)
print(my_list)   # [10, 20, 40]
```

## 4. Using pop() (by index, returns the value)
Removes and returns the element at the given index.
If no index is given, removes the last element.

```python
my_list = [10, 20, 30, 40]
value = my_list.pop(2)   # removes element at index 2 (30)
print(value)             # 30
print(my_list)           # [10, 20, 40]

last = my_list.pop()     # removes last element (40)
print(last)              # 40
print(my_list)           # [10, 20]
```
---

## ✅ Summary
del → Delete by index, no return

Slicing → Create a new list without the element

remove() → Delete by value, only first match

pop() → Delete by index (default last) and return the removed element