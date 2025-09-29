# 📄 Python File I/O Cheat Sheet
## 1. Opening Files
```python
open(filename, mode="r", encoding="utf-8")
```

- "r" → read (default)
- "w" → write (overwrite if exists)
- "a" → append (add to end)
- "x" → exclusive creation (error if exists)
- "b" → binary mode (e.g. "rb", "wb")
- "t" → text mode (default)

Always use encoding="utf-8" for text files.

---
## 2. With Statement (Best Practice)
```python
with open("file.txt", "r", encoding="utf-8") as f:
    data = f.read()
# file automatically closed here
```

## 3. Reading Files
```python
f.read()         # entire file as string
f.readline()     # one line at a time
f.readlines()    # list of all lines
```

Looping:
```python
with open("file.txt") as f:
    for line in f:
        print(line.strip())   # strip() removes \n
```

## 4. Writing Files
```python
with open("file.txt", "w", encoding="utf-8") as f:
    f.write("Hello!\n")
    f.writelines(["Line1\n", "Line2\n"])
```

## 5. Appending Files
```python
with open("file.txt", "a", encoding="utf-8") as f:
    f.write("New line\n")
```

## 6. File Position
```python
f.tell()    # current position in file
f.seek(0)   # go to beginning
f.seek(5)   # move to byte 5
```

## 7. Checking if File Exists

(⚠️ not needed for your exercise, but useful later)
```python
import os
os.path.exists("file.txt")
```
