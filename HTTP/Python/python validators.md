# 🧭 Python `validators` Cheat Sheet

## 🔹 Installation
```bash
pip install validators
```

---

## 🧩 Basic Usage
```python
import validators

validators.url('https://example.com')   # ✅ True
validators.email('user@example.com')    # ✅ True
validators.ipv4('192.168.1.1')          # ✅ True
validators.ipv6('2001:db8::ff00:42:8329')  # ✅ True
validators.domain('example.com')        # ✅ True
validators.uuid('550e8400-e29b-41d4-a716-446655440000')  # ✅ True
```

All functions return:
- `True` → valid  
- `ValidationFailure` object → invalid  

You can check validity like this:
```python
if validators.url(url):
    print("Valid URL")
else:
    print("Invalid URL")
```

---

## 🧱 Common Validators

| Function | Validates | Example |
|-----------|------------|---------|
| `validators.url(value)` | URL format | `https://openai.com` |
| `validators.domain(value)` | Domain name | `example.org` |
| `validators.email(value)` | Email address | `test@mail.com` |
| `validators.ipv4(value)` | IPv4 address | `192.168.0.1` |
| `validators.ipv6(value)` | IPv6 address | `2001:db8::1` |
| `validators.mac_address(value)` | MAC address | `00:1B:44:11:3A:B7` |
| `validators.uuid(value)` | UUID string | `550e8400-e29b-41d4-a716-446655440000` |
| `validators.slug(value)` | Slug format | `my-article-title` |
| `validators.length(value, min=0, max=None)` | String length | `validators.length('abc', min=2)` |
| `validators.between(value, min=None, max=None)` | Number range | `validators.between(10, min=5, max=15)` |
| `validators.integer(value)` | Integer type | `validators.integer('123')` |
| `validators.boolean(value)` | Boolean-like values | `validators.boolean('true')` |
| `validators.card_number(value)` | Credit card number | `validators.card_number('4111111111111111')` |
| `validators.hash(value, algorithm='sha256')` | Hash string | `validators.hash('a'*64, 'sha256')` |

---

## ⚙️ Custom Validation Example
```python
def is_valid_username(username):
    if not validators.length(username, min=3, max=20):
        return False
    if not validators.slug(username):
        return False
    return True
```

---

## 🔍 Checking ValidationFailure Details
```python
result = validators.url('invalid_url')
if result is not True:
    print(result)          # ValidationFailure object
    print(result.args)     # See details
```

---

## 🧰 Tips
- Combine with **regex** for advanced custom rules.
- Wrap validations in a function for reusable input checks.
- Works great for web apps, APIs, or CLI input validation.
