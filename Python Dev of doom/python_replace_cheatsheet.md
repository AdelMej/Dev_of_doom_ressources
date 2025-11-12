# 🐍 Python `replace()` Cheat Sheet

---

## 🔹 Syntax
```python
string.replace(old, new, count)
```
**Parameters:**
- `old`: substring to replace  
- `new`: substring to insert  
- `count` *(optional)*: number of occurrences to replace (default = all)

---

## 🚀 1. Basic Replacement
```python
text = "hello bro"
new_text = text.replace("bro", "world")
print(new_text)
# → hello world
```

---

## 🔁 2. Replace Only the First N Occurrences
```python
text = "bro bro bro"
new_text = text.replace("bro", "dude", 1)
print(new_text)
# → dude bro bro
```

---

## 🧠 3. Case-Sensitive
`replace()` is **case-sensitive**:
```python
"Bro bro".replace("bro", "dude")
# → 'Bro dude'
```

For case-insensitive replacement, use **regex**:

---

## ⚡ 4. Case-Insensitive Replacement with `re.sub()`
```python
import re

text = "Bro bro BRO"
new_text = re.sub(r"bro", "dude", text, flags=re.IGNORECASE)
print(new_text)
# → dude dude dude
```

---

## 🔍 5. Using Capture Groups
```python
import re

text = "Name: John, Age: 30"
new_text = re.sub(r"(\w+): (\w+)", r"\1 => \2", text)
print(new_text)
# → Name => John, Age => 30
```

- `\1`, `\2`, etc. = capture groups  
- `r"..."` = raw string

---

## 🧩 6. Replace with a Function
```python
import re

def replacer(match):
    num = int(match.group())
    return str(num * 2)

text = "Values: 2, 4, 6"
new_text = re.sub(r"\d+", replacer, text)
print(new_text)
# → Values: 4, 8, 12
```

---

## 💡 7. Common Tricks

✅ **Remove characters:**
```python
text = "hello!"
text = text.replace("!", "")
# → hello
```

✅ **Replace multiple spaces:**
```python
import re
re.sub(r"\s+", " ", "hello     world")
# → 'hello world'
```

✅ **Chain replacements:**
```python
text = "hello_bro"
text = text.replace("_", " ").replace("bro", "world")
# → hello world
```

---
