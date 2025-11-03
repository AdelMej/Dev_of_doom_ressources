# 🧭 Python Regex Cheat Sheet

## 🔹 Import
```python
import re
```

---

## 🧩 Basic Functions

| Function | Description | Example |
|-----------|--------------|----------|
| `re.match(pattern, string)` | Match only at the beginning | `re.match(r'cat', 'catfish')` |
| `re.search(pattern, string)` | Search anywhere in the string | `re.search(r'cat', 'dogcat')` |
| `re.findall(pattern, string)` | Return all non-overlapping matches | `re.findall(r'\d+', 'a1b2c3')` |
| `re.finditer(pattern, string)` | Return iterator of matches | `re.finditer(r'\w+', 'Hello world')` |
| `re.sub(pattern, repl, string)` | Replace occurrences | `re.sub(r'\d', '#', 'a1b2c3')` |
| `re.split(pattern, string)` | Split by pattern | `re.split(r'[,; ]+', 'a,b;c d')` |
| `re.fullmatch(pattern, string)` | Match entire string | `re.fullmatch(r'\d+', '123')` |

---

## 🔠 Common Patterns

| Pattern | Description | Example Match |
|----------|--------------|---------------|
| `.` | Any character except newline | `a.b` → `acb` |
| `^` | Start of string | `^Hello` → `Hello world` |
| `$` | End of string | `world$` → `Hello world` |
| `\d` | Digit (0–9) | `\d+` → `123` |
| `\D` | Non-digit | `\D+` → `abc` |
| `\w` | Word character | `\w+` → `Hello123` |
| `\W` | Non-word character | `\W` → `!` |
| `\s` | Whitespace | `\s+` → space, tab, newline |
| `\S` | Non-whitespace | `\S+` → `text` |

---

## 🔢 Quantifiers

| Quantifier | Meaning | Example |
|-------------|----------|----------|
| `*` | 0 or more | `a*` → ``, `a`, `aaa` |
| `+` | 1 or more | `a+` → `a`, `aaa` |
| `?` | 0 or 1 | `colou?r` → `color`, `colour` |
| `{n}` | Exactly n times | `a{3}` → `aaa` |
| `{n,}` | n or more times | `a{2,}` → `aa`, `aaaa` |
| `{n,m}` | Between n and m | `a{2,4}` → `aa`, `aaa` |

---

## 🧱 Character Classes

| Class | Description | Example |
|--------|--------------|----------|
| `[abc]` | a, b, or c | `b` |
| `[^abc]` | Not a, b, or c | `d` |
| `[a-z]` | Lowercase letters | `g` |
| `[A-Z]` | Uppercase letters | `F` |
| `[0-9]` | Digits | `7` |
| `[A-Za-z0-9_]` | Alphanumeric | `G4` |

---

## ⚙️ Groups and Alternation

| Syntax | Meaning | Example |
|---------|----------|----------|
| `(abc)` | Capture group | `re.search('(abc)', '123abc456')` |
| `(?:abc)` | Non-capturing group | No capture |
| `(?P<name>abc)` | Named group | Access via `match.group('name')` |
| `a|b` | a or b | `cat|dog` → `dog` |

---

## 🧩 Useful Anchors & Assertions

| Pattern | Description | Example |
|----------|--------------|----------|
| `^` | Start of string | `^Hello` |
| `$` | End of string | `world$` |
| `\b` | Word boundary | `\bword\b` |
| `\B` | Non-word boundary | `py\B` |
| `(?=...)` | Positive lookahead | `\d(?=px)` matches number before `px` |
| `(?!...)` | Negative lookahead | `\d(?!px)` excludes `px` |
| `(?<=...)` | Positive lookbehind | `(?<=USD)\d+` matches number after USD |
| `(?<!...)` | Negative lookbehind | `(?<!USD)\d+` excludes USD |

---

## 🧰 Examples

```python
# Validate email
re.match(r'^[\w\.-]+@[\w\.-]+\.\w+$', 'test@mail.com')

# Validate IPv4
re.match(r'^(\d{1,3}\.){3}\d{1,3}$', '192.168.1.1')

# Validate URL
re.match(r'^https?://[\w.-]+(?:\.[\w\.-]+)+[/#?]?.*$', 'https://example.com')

# Extract numbers
re.findall(r'\d+', 'Item 12 costs 30 euros')

# Replace multiple spaces
re.sub(r'\s+', ' ', 'Hello   world')
```

---

## 🧠 Tips
- Use **raw strings** (`r"pattern"`) to avoid escape hell.
- Combine with `re.compile()` for efficiency.
- Use named groups for clarity.
- Test regexes on sites like [regex101.com](https://regex101.com/).
