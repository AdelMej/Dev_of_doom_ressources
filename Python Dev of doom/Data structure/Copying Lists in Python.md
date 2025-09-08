# Copying Lists in Python

Sometimes you want a **new list** that doesn’t affect the original. Python gives you several ways to do this.

## 1. Using Slicing

`original = [1, 2, 3] copy_list = original[:]  # [:] creates a shallow copy`

---

## 2. Using `list()`

`original = [1, 2, 3] copy_list = list(original)`

---

## 3. Using `.copy()`

`original = [1, 2, 3] copy_list = original.copy()`

---

## ⚠️ Shallow vs Deep Copy

All the above methods create a **shallow copy**:

`original = [[1, 2], [3, 4]] copy_list = original.copy() copy_list[0][0] = 99 print(original)  # [[99, 2], [3, 4]]  <- original is affected!`

- Inner mutable objects are **still shared**.
    

---

### 4. Using `copy.deepcopy()` for a true copy

`import copy  original = [[1, 2], [3, 4]] copy_list = copy.deepcopy(original) copy_list[0][0] = 99 print(original)  # [[1, 2], [3, 4]]  <- original is safe`

- Creates a **deep copy**, so even nested objects are duplicated.