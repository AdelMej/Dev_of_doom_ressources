# 🧩 Jinja2 Cheat Sheet

## 🏗️ Basic Syntax
| Description                        | Syntax                    |
| ---------------------------------- | ------------------------- |
| Variable interpolation             | `{{ variable }}`          |
| Expression                         | `{{ user.name }}`         |
| Escaped output (no HTML rendering) | `{{ variablee }}`         |
| Comment                            | `{# This is a comment #}` |
| Statement (logic/control)          | `{% statement %}`         |

---

## 🔁 Control Structures
### If / Else
```jinja2
{% if user.is_admin %}
  Welcome, admin!
{% elif user.is_guest %}
  Hello, guest!
{% else %}
  Access denied.
{% endif %}
```

### For Loop
```jinja2
{% for item in list %}
  {{ loop.index }}. {{ item }}
{% endfor %}
```
**Loop Variables:**
- `loop.index` → 1-based index  
- `loop.index0` → 0-based index  
- `loop.revindex` → Reverse count  
- `loop.first` / `loop.last` → Booleans  
- `loop.length` → Total items  

---

## 🧱 Filters
```jinja2
{{ name | upper }}
{{ price | round(2) }}
{{ text | replace("old", "new") }}
{{ list | join(", ") }}
{{ data | default("N/A") }}
```

**Common Filters:**
| Filter | Description |
|-----------|-------------|
| `upper`, `lower`, `title` | Text case changes |
| `length` | Returns length |
| `join` | Joins list items |
| `replace(old, new)` | Replace substring |
| `default(value)` | Fallback if variable undefined |
| `safe` | Disable auto-escaping |
| `truncate(length)` | Shorten text |
| `sort`, `unique` | List utilities |


---

## 🧠 Macros (Reusable Templates)
```jinja2
{% macro greet(name) %}
  <p>Hello, {{ name }}!</p>
{% endmacro %}

{{ greet("Alice") }}
```

---

## 🪣 Includes & Inheritance
### Include
```jinja2
{% include "header.html" %}
```

### Template Inheritance
```jinja2
<!-- base.html -->
<html>
  <body>
    {% block content %}{% endblock %}
  </body>
</html>
```

```jinja2
<!-- child.html -->
{% extends "base.html" %}

{% block content %}
  <p>This is the child template.</p>
{% endblock %}
```

---

## 🧮 Expressions
```jinja2
{{ 5 + 10 }}
{{ "Hi " + name }}
{{ items|length > 3 }}
{{ "admin" in roles }}
```

---

## ⚙️ With Flask Example
```python
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def home():
    user = {"name": "Alice", "is_admin": True}
    return render_template("index.html", user=user)
```

```jinja2
<!-- templates/index.html -->
<h1>Hello, {{ user.name }}!</h1>
{% if user.is_admin %}
  <p>You have admin privileges.</p>
{% endif %}
```

---

## 💡 Useful Tips
- Use `{% set var = value %}` to define variables inside a template.
- Use `{% raw %} ... {% endraw %}` to prevent Jinja from interpreting code blocks.
- Use `{% for ... %} {% else %} {% endfor %}` to handle empty lists.

---

**Docs:** [https://jinja.palletsprojects.com](https://jinja.palletsprojects.com)
