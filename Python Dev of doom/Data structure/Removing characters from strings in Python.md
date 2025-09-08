# Removing Characters from Strings in Python – Complete Guide

Python strings are **immutable**, so you always create a **new string** with the characters removed.

---

## 1. Using `replace()`
```python
s = "hello world" # Remove all 'o' new_s = s.replace("o", "") 
print(new_s) # hell wrld
```
- Simple and readable.
- Removes **all occurrences** of the character or substring.


---

## 2. Using Slicing (by Index)
```python
s = "hello" # Remove character at index 2 ('l') 
new_s = s[:2] + s[3:]
print(new_s)  # helo
```

- Use this when you know the **position** of the character.

---

## 3. Using List Comprehension + `join()` (by Value)
```python
s = "hello world" # Remove all 'l' 
new_s = "".join(c for c in s if c != "l") 
print(new_s)  # heo word
```
- Works for **any condition**, not just a specific character.

---

## 4. Using `str.translate()` (Advanced, Multiple Characters)
```python
s = "hello world" # Remove 'l' and 'o' 
new_s = s.translate(str.maketrans("", "", "lo"))
print(new_s)  # he wrd
```
- Efficient for removing **multiple different characters**.

---

### ✅ Summary

|Method|When to Use|Notes|
|---|---|---|
|`replace()`|Remove all occurrences of a specific character or substring|Simple and readable|
|Slicing|Remove a character at a known **index**|Works for single characters|
|`join()` + comprehension|Remove by **value**, any condition|Flexible, works for multiple conditions|
|`translate()`|Remove **multiple characters efficiently**|Powerful for bigger strings or multiple removals|