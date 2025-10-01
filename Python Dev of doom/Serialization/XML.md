# 📂 XML Cheat Sheet (Python `xml.etree.ElementTree`)

### Writing a dict to XML

```python
import xml.etree.ElementTree as ET

data = {"name": "John", "age": 28, "is_student": False}
root = ET.Element("data")

for k, v in data.items():
	ET.SubElement(root, k).text = str(v) 

tree = ET.ElementTree(root)
tree.write("data.xml", encoding="utf-8", xml_declaration=True)`
```
**Output (`data.xml`):**
```xml
<?xml version='1.0' encoding='utf-8'?>
<data>
	<name>John</name>
	<age>28</age>
	<is_student>False</is_student>
</data>`
```

---

### Reading XML

```python
tree = ET.parse("data.xml")
root = tree.getroot()
for child in root:
	print(child.tag, child.text)`
```

**Output:**
```python
name John
age 28
is_student False
```

---

### Navigating XML

`root.find("name").text         # Get <name> content root.findall("item")           # Get all <item> elements root.attrib                    # Access attributes if present`

---

### Updating XML
```python
root.find("age").text = "29"
tree.write("data.xml")`
```

---

### Key Points

- XML stores all values as **strings**; convert types manually.
- Requires a **root element** and an `ElementTree` object.
- More verbose than JSON/CSV, but useful for hierarchical data and legacy systems.