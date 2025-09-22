# **Python OOP Cheat Sheet**

### **1. Class vs Object / Instance**

|Concept|Description|Example|
|---|---|---|
|Class|Blueprint for objects|`class Circle: ...`|
|Object/Instance|Concrete entity created from a class|`c = Circle(5)`|

---

### **2. Attributes**

|Type|Access / Convention|Example|
|---|---|---|
|Public|Fully accessible|`self.radius = 5`|
|Protected|`_name` → internal use|`self._radius = 5`|
|Private|`__name` → name-mangled|`self.__radius = 5`|
|Class attr|Shared across instances|`Circle.material = "plastic"`|
|Dynamic|Add at runtime|`c.color = "red"`|

**Access Lookup Order:**  
`instance.__dict__ → class.__dict__ → parent classes`

---

### **3. Methods**

|Type|Access|Example|
|---|---|---|
|Instance method|`self`|`def area(self): ...`|
|Class method|`cls`|`@classmethod def from_diameter(cls, d): ...`|
|Static method|None|`@staticmethod def add(a, b): ...`|

**Special Method:**

- `__init__(self, ...)` → constructor, sets up initial attributes
    

---

### **4. Property**

- Wraps getter/setter around an attribute
    
- Makes attribute access clean and controlled
    
```python
class Circle:
     def __init__(self, r):
              self._radius = r

	@property
	def radius(self):
		return self._radius
	
	@radius.setter
	def radius(self, r):
		if r < 0: raise ValueError
			self._radius = r  

c.radius += 2  # getter + setter used
```


**Attribute vs Property**

|Attribute|Property|
|---|---|
|Raw data|Getter/setter disguised as attribute|
|Direct access|Controlled access with logic|

---

### **5. Special Concepts**

|Concept|Description|Example|
|---|---|---|
|Data Abstraction|Expose only essential info|Using property for access|
|Encapsulation|Bundle data + behavior|Class attributes + methods|
|Information Hiding|Restrict internal access|`_` or `__` attributes|
|`self`|Refers to instance of object|`self.radius = r`|
|`__dict__`|Stores all attributes of instance/class|`c.__dict__`|
|`getattr`|Get attribute dynamically|`getattr(c, "radius", 0)`|

---

### **6. Dynamic Attributes**

```python
!c.new_attr = "hello"      # instance-only 
Circle.color = "blue"      # class-level`
```

---

### **7. Lookup Order (MRO)**

`instance → class → parent classes`

### 8. Python Special Instance Methods Cheat Sheet

| Method / Attribute | When it’s called / Purpose                             |
|-------------------|-------------------------------------------------------|
| `__init__`        | Constructor — runs when an instance is created       |
| `__str__`         | Human-readable string — used by `print()`            |
| `__repr__`        | Developer-friendly string — for debugging/REPL       |
| `__del__`         | Destructor — runs when the instance is deleted       |
| `__doc__`         | Prints the documentation of an object (docstring)    |

**Notes:**
- `__str__` falls back to `__repr__` if not defined  
- `__repr__` defaults to `<__main__.Class object at 0x...>` if not defined  
- `__del__` is optional and runs when Python garbage-collects the object 
- `__doc__` returns `None` if no docstring is defined
