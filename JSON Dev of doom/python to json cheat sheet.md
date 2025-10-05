# 🐍 Python ↔ JSON Cheat Sheet

### 🔄 Type Conversion

|Python Type|JSON Equivalent|Example Python → JSON|Example JSON → Python|
|---|---|---|---|
|`dict`|object|`{"a": 1}` → `{"a": 1}`|`{"a": 1}` → `{"a": 1}`|
|`list`|array|`[1, 2, 3]` → `[1, 2, 3]`|`[1, 2, 3]` → `[1, 2, 3]`|
|`tuple`|array|`(1, 2)` → `[1, 2]`|`[1, 2]` → `[1, 2]` (always a list back)|
|`str`|string|`"hello"` → `"hello"`|`"hello"` → `"hello"`|
|`int` / `float`|number|`3.14` → `3.14`|`3.14` → `3.14`|
|`True`|true|`True` → `true`|`true` → `True`|
|`False`|false|`False` → `false`|`false` → `False`|
|`None`|null|`None` → `null`|`null` → `None`|

---

### ⚠️ Things to Watch Out For

#### 1. **Tuples always turn into lists** (no tuple in JSON).
```python
    import json
    json.dumps((1, 2))  # => "[1, 2]"`
```

#### 2. **Dict keys must be strings**.
```python
    json.dumps({1: "a"})
    # => '{"1": "a"}'  (int key auto-converted to string)
```
#### 3. **Special floats (`NaN`, `inf`) are not standard JSON**.
```python
import json
json.dumps(float("nan")) # default: "NaN" (not valid JSON!)
json.dumps(float("nan"), allow_nan=False)
# => raises ValueError`
```

#### 4. **Booleans are lowercase in JSON** (`true` / `false`).
```python
    json.dumps(True)
    # => "true"`
```

#### 5. **UTF-8 is the default** → JSON strings can hold emojis, accents, etc.

```python
json.dumps("café")  # => "\"café\""`
```