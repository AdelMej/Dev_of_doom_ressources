# 🧩 Jinja2 Macros & Filters Cheat Sheet

## 🧠 Macros

### 📦 Definition
Macros let you define reusable blocks of template code (like functions in Python).

```jinja2
{% macro greet(name) %}
  Hello, {{ name }}!
{% endmacro %}

<p>{{ greet("Alice") }}</p>
```

### 📥 With Parameters
```jinja2
{% macro user_card(name, age) %}
  <div class="card">
    <p>Name: {{ name }}</p>
    <p>Age: {{ age }}</p>
  </div>
{% endmacro %}

{{ user_card("John", 25) }}
```

### 🧱 With Default Parameters
```jinja2
{% macro greet(name="Guest") %}
  Hello, {{ name }}!
{% endmacro %}

{{ greet() }}
{{ greet("Bob") }}
```

### 🔁 Macros and Loops
```jinja2
{% macro list_items(items) %}
  <ul>
    {% for item in items %}
      <li>{{ item }}</li>
    {% endfor %}
  </ul>
{% endmacro %}

{{ list_items(["Apple", "Banana", "Cherry"]) }}
```

### 📄 Importing Macros from Another File
```jinja2
{# in macros.html #}
{% macro button(label, color="blue") %}
  <button style="background-color: {{ color }};">{{ label }}</button>
{% endmacro %}

{# in another template #}
{% import "macros.html" as ui %}
{{ ui.button("Click Me", "red") }}
```

---

## 🧹 Filters

Filters modify data before rendering.  
You can chain multiple filters using the pipe `|` syntax.

### 🧩 Common Filters

| Filter | Example | Output |
|--------|----------|---------|
| `lower` | `{{ "HELLO"|lower }}` | `hello` |
| `upper` | `{{ "hi"|upper }}` | `HI` |
| `title` | `{{ "hello world"|title }}` | `Hello World` |
| `capitalize` | `{{ "jinja"|capitalize }}` | `Jinja` |
| `length` | `{{ [1,2,3]|length }}` | `3` |
| `join` | `{{ ['a','b','c']|join(', ') }}` | `a, b, c` |
| `replace` | `{{ "I love Python"|replace("Python", "Jinja") }}` | `I love Jinja` |
| `default` | `{{ name|default('Guest') }}` | `Guest` |
| `trim` | `{{ "  hello  "|trim }}` | `hello` |
| `safe` | Marks content as safe (no escaping) |

### 🧮 Numeric Filters
| Filter | Example | Output |
|--------|----------|---------|
| `abs` | `{{ -5|abs }}` | `5` |
| `round` | `{{ 3.14159|round(2) }}` | `3.14` |
| `int` | `{{ "42"|int }}` | `42` |
| `float` | `{{ "3.5"|float }}` | `3.5` |

### 🧠 Conditional Filters
```jinja2
{{ value|default("N/A") if not value }}
```

### 🔗 Chaining Filters
```jinja2
{{ "  hello "|trim|upper }}
# Output: HELLO
```

### 🧰 Custom Filters (Python Side)
```python
def reverse_string(s):
    return s[::-1]

app.jinja_env.filters["reverse"] = reverse_string
```
Then in template:
```jinja2
{{ "Hello"|reverse }}
# Output: olleH
```

---

## ⚡ Quick Reference

- **Define macro:** `{% macro name(params) %}...{% endmacro %}`  
- **Call macro:** `{{ macro_name(args) }}`  
- **Import macros:** `{% import "macros.html" as alias %}`  
- **Use filters:** `{{ variable|filter }}`  
- **Chain filters:** `{{ variable|filter1|filter2 }}`  
- **Add custom filters (Python):** `app.jinja_env.filters["name"] = func`

---

✨ **Pro Tip:** Combine macros and filters to create reusable, clean, and DRY templates!
