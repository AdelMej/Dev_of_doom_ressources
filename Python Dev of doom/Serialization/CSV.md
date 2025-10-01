# 📊 CSV Cheat Sheet (Python csv)
### Writing a list of dicts to CSV
```python
import csv

rows = [
    {"name": "John", "age": 28, "city": "New York"},
    {"name": "Alice", "age": 24, "city": "Los Angeles"},
]

with open("data.csv", "w", newline="", encoding="utf-8") as f:
    writer = csv.DictWriter(f, fieldnames=["name", "age", "city"])
    writer.writeheader()     # writes: name,age,city
    writer.writerows(rows)   # writes each dict as a row
```

### Output (data.csv):
```python
name,age,city
John,28,New York
Alice,24,Los Angeles

Reading CSV into dicts
with open("data.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row)

```
---
### Reading CSV into dicts
```python
with open("data.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row)
```
### Output:
```python
{'name': 'John', 'age': '28', 'city': 'New York'}
{'name': 'Alice', 'age': '24', 'city': 'Los Angeles'}
```

	Note: All values are strings. Convert types manually if necessary.

---

### Reading CSV as raw lists
```pytthon
with open("data.csv", "r", encoding="utf-8") as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
```

### Output:
```python
['name', 'age', 'city']
['John', '28', 'New York']
['Alice', '24', 'Los Angeles']
```

---

### Appending rows
```python
with open("data.csv", "a", newline="", encoding="utf-8") as f:
    writer = csv.writer(f)
    writer.writerow(["Bob", 22, "Chicago"])
```

### Key Points

- Best for tabular data.
- All values are stored as strings.
- Use newline="" on Windows to avoid blank lines.
- Explicit fieldnames are required when using DictWriter.