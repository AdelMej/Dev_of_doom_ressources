### ❌ Wrong vs ✅ Correct JSON

#### 1. Keys must be **double-quoted strings**

❌ {name: "John"}
✅ {"name": "John"}

---

#### 2. Trailing commas not allowed

❌ {"a": 1, "b": 2,}
✅ {"a": 1, "b": 2}

---

#### 3. Values must be valid types

Allowed: string, number, object, array, true, false, null

❌ {"is_active": true, 12 } 
✅ {"is_active": true, "id": 12}

---

#### 4. Strings must use double quotes

❌ {'city': "Tokyo"}
✅ {"city": "Tokyo"}

---

#### 5. Special numbers (NaN, Infinity) not valid in strict JSON

❌ {"value": NaN}
✅ {"value": null}