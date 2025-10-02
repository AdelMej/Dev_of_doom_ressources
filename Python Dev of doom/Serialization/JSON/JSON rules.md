### 🔑 JSON Core Rules

#### 1. **Keys must be strings**
    
    - Always wrapped in **double quotes**.
    - ✅ `{"name": "Alice"}`
    - ❌ `{name: "Alice"}`
#### 2. **Strings must use double quotes (`"`)**
    
    - ✅ `"Hello"`
    - ❌ `'Hello'`
#### 3. **Values can only be**:
    
    - String → `"text"`
    - Number → `123`, `3.14` (no `NaN`, `Infinity`, or `-Infinity`)
    - Boolean → `true`, `false` (lowercase!)
    - Null → `null` (not `None`)
    - Object → `{ "key": value }`
    - Array → `[1, 2, 3]`
#### 4. **No trailing commas**
    
    - ✅ `{"a": 1, "b": 2}`
    - ❌ `{"a": 1, "b": 2, }`
#### 5. **Whitespace is allowed but ignored**

- ✅ Both `{"a":1}` and
```css
        {
           "a": 1 
        }
```
are valid.

#### 6. **Numbers must be valid JSON numbers**
    
    - No leading zeroes except for `0`.
    - ✅ `0`, `42`, `-3.5`, `2e10`
    - ❌ `0123`, `.5`, `NaN`

#### 7. **JSON is UTF-8 by default**
    - Unicode is supported (`"café"`, `"\u00e9"`).

---

### ⚠️ Common gotchas for Python devs

- JSON `true` / `false` ≠ Python `True` / `False` → need conversion.
- JSON `null` ≠ Python `None`.
- JSON doesn’t allow **tuples** (`(1, 2, 3)`) → they become lists (`[1, 2, 3]`).
- JSON object keys must be strings, but Python dicts can use other types (like numbers).