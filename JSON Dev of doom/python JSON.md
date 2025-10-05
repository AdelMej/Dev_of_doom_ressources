# 📄 JSON Cheat Sheet (Python, General Use)

### 📦 Import
```python
import json
```

---

## 🔹 Strings vs Files (easy mental model)

- **`dumps` / `loads`** → work with **strings** (`s = string`)
- **`dump` / `load`** → work with **files**

---

## 🔹 Convert Python ↔ JSON

### Python → JSON string

```python
data = {"name": "Alice", "age": 25}
json_str = json.dumps(data)
# '{"name": "Alice", "age": 25}
```

Pretty print JSON:
```python
print(json.dumps(data, indent=4))
```

---
### JSON string → Python
```python
json_str = '{"name": "Alice", "age": 25}'
data = json.loads(json_str) 
# {"name": "Alice", "age": 25}`
```

---
## 🔹 Save to / Load from File

### Save JSON to file

```python
with open("data.json", "w", encoding="utf-8") as f:
     json.dump(data, f, indent=4)  # indent for pretty file
```

### Load JSON from file
```python
with open("data.json", "r", encoding="utf-8") as f:     data = json.load(f)
```

---
## 🔹 Advanced Tricks

### Sort keys
```python
print(json.dumps(data, indent=2, sort_keys=True))
```

### Handle non-serializable objects
```python
import datetime

data = {"time": datetime.datetime.now()}

def default(obj):
	if isinstance(obj, datetime.datetime):
		return obj.isoformat()
	raise TypeError("Type not serializable")

print(json.dumps(data, default=default))
```
### Decode into custom objects

```python
class User:
	def __init__(self, name, age): 
		self.name, self.age = name, age  

def user_hook(d):
	return User(d["name"], d["age"])
	
json_str = '{"name": "Alice", "age": 25}'
user = json.loads(json_str, object_hook=user_hook)

print(user.name)  # Alice
```

---

## 🔹 Common Pitfalls

- JSON supports only: **dict, list, str, int, float, bool, None**
- Tuples become lists in JSON.
- Keys in JSON must be **strings**, not numbers.
- Watch out for encoding: always use `utf-8` for cross-platform safety.