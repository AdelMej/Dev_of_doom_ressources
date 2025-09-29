### **Python Object Attribute Helpers Cheat Sheet**

|Function|What it does|Example|
|---|---|---|
|`setattr(obj, name, value)`|Sets `obj.name = value` dynamically.|`setattr(s, "age", 20)`|
|`getattr(obj, name[, default])`|Gets `obj.name` dynamically; returns `default` if missing.|`getattr(s, "first_name", "Unknown")`|
|`hasattr(obj, name)`|Checks if `obj` has an attribute `name`.|`hasattr(s, "last_name")` → True/False|
|`delattr(obj, name)`|Deletes an attribute dynamically.|`delattr(s, "age")`|
|`obj.__dict__`|Dictionary of all **public instance attributes** of the object.|`s.__dict__` → `{'first_name': 'Alice', 'last_name': 'Smith'}`|
|`type(obj)`|Returns the type of the object.|`type(s)` → `<class '__main__.Student'>`|

---

💡 **Tip:** Using `setattr` + `getattr` + `hasattr` is basically Python’s way of doing **dynamic object manipulation**, very handy for JSON parsing, serializers, or generic class utilities.