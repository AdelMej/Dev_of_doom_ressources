### **Python Mini JSON & Dynamic Attribute Cheat Sheet**

#### **1. Convert object to dictionary**

- `obj.__dict__` → gives all **public instance attributes** as a dictionary.
```python
s = Student("Alice", "Smith", 20)
print(s.__dict__)
# {'first_name': 'Alice', 'last_name': 'Smith', 'age': 20}
```
- Filter attributes using a list (`attrs`):
```python
attrs = ["first_name", "age"] 
json_dict = {k: v for k, v in s.__dict__.items() if k in attrs}`
```

---

#### **2. Update object from dictionary**

- Loop through dictionary keys and dynamically set attributes:
```python
data = {"first_name": "Bob", "age": 25} 
for key in data:
     if key in s.__dict__: # only existing attributes
         setattr(s, key, data[key])
```

- Result: `s.first_name == "Bob"`, `s.age == 25`

---

#### **3. Handy dynamic functions**

|Function|Purpose|
|---|---|
|`setattr(obj, attr, value)`|Set attribute dynamically|
|`getattr(obj, attr[, default])`|Get attribute dynamically, with optional default|
|`hasattr(obj, attr)`|Check if attribute exists|
|`delattr(obj, attr)`|Delete attribute|

Example:
```python
if hasattr(s, "last_name"):
	print(getattr(s, "last_name"))  # Smith
	setattr(s, "last_name", "Johnson") 
	delattr(s, "age")
```


---

#### **4. Tips for `to_json` / `reload_from_json`**

- **to_json(attrs=None)**:
    
    - Use `__dict__` as base.
    - Filter attributes if `attrs` is a list.

- **reload_from_json(json_dict)**:
    
    - Loop through dictionary keys.
    - Use `setattr` to safely update attributes.
    - Optionally check `hasattr` or `key in self.__dict__` to avoid extra keys.