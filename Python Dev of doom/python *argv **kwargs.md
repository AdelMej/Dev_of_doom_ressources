# 🐍 Python `*args` and `**kwargs` Cheat Sheet

---

## 🔹 Basics

|Syntax|Meaning|
|---|---|
|`*args`|Collects **positional arguments** into a tuple|
|`**kwargs`|Collects **keyword arguments** into a dictionary|

---

## 🔸 Using `*args`

```python
def greet(*args):
    for name in args:
        print(f"Hello, {name}!")

greet("Alice", "Bob", "Charlie")
```

**Output:**

```bash
Hello, Alice!
Hello, Bob!
Hello, Charlie!
```

✅ You can pass any number of **positional arguments**.  
🧠 Inside the function, `args` is a **tuple**.

---

## 🔸 Using `**kwargs`

```python
def show_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

show_info(name="Alice", age=25, country="France")
```

**Output:**

```bash
name: Alice
age: 25
country: France
```

✅ You can pass any number of **keyword arguments**.  
🧠 Inside the function, `kwargs` is a **dictionary**.

---

## 🔹 Combining `*args` and `**kwargs`

```python
def demo(pos1, *args, kw1="default", **kwargs):
    print("pos1:", pos1)
    print("args:", args)
    print("kw1:", kw1)
    print("kwargs:", kwargs)

demo("A", "B", "C", kw1="X", key1=123, key2=456)
```

**Output:**

```bash
pos1: A
args: ('B', 'C')
kw1: X
kwargs: {'key1': 123, 'key2': 456}
```

⚠️ **Order matters:**

```bash
def func(positional, *args, keyword_only, **kwargs):
    ...
```

Order =  
1️⃣ Normal args  
2️⃣ `*args`  
3️⃣ Keyword-only args  
4️⃣ `**kwargs`

---

## 🔸 Unpacking with `*` and `**`

### With lists/tuples (`*`)

```python
def add(x, y, z):
    return x + y + z

nums = [1, 2, 3]
print(add(*nums))  # same as add(1, 2, 3)
```
### With dicts (`**`)

```python
def introduce(name, age):
    print(f"{name} is {age} years old.")

data = {"name": "Alice", "age": 25}
introduce(**data)
```

---

## 🧩 Common Use Cases

✅ Flexible APIs  
✅ Wrapping or forwarding arguments  
✅ Logging / debugging  
✅ Function decorators

---

## 🧠 Pro Tip: Forwarding Arguments

```python
def wrapper(*args, **kwargs):
    print("Before call")
    result = target_function(*args, **kwargs)
    print("After call")
    return result
```