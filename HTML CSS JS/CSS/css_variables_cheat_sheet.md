# 🎨 CSS Variables Cheat Sheet

A quick guide to mastering CSS custom properties (`--variables`).

---

## 🎯 1. Declaring Variables

Define them inside a selector, usually `:root` for global scope:

```css
:root {
  --color-primary: #d73953;
  --font-size-large: 1.5rem;
}
```

✅ Variables **must start with `--`**

---

## 🧩 2. Using Variables

Use the `var()` function to access them:

```css
body {
  color: var(--color-primary);
  font-size: var(--font-size-large);
}
```

---

## 🚨 3. Fallback Values

Provide a default if the variable isn’t defined:

```css
p {
  color: var(--text-color, #000); /* fallback to #000 */
}
```

---

## 🧬 4. Scope

Variables follow **CSS scoping rules**:

```css
:root {
  --color: blue;
}

header {
  --color: red; /* overrides inside header only */
  color: var(--color); /* red */
}

main {
  color: var(--color); /* blue */
}
```

---

## 🔁 5. Reusing & Chaining

You can reference other variables:

```css
:root {
  --base-color: #d73953;
  --border-color: var(--base-color);
}
```

---

## 🧮 6. Math with `calc()`

Math doesn’t work *inside* `var()`, but it works *with* `calc()`:

```css
:root {
  --spacing: 10px;
}

div {
  margin: calc(var(--spacing) * 2);
}
```

---

## 🎨 7. Dynamic Updates with JavaScript

They’re live — you can update them at runtime:

```js
document.documentElement.style.setProperty('--color-primary', '#00ff00');
```

---

## ⚡ Common Use Cases

| Purpose | Example Variables |
|----------|-------------------|
| Theme colors | `--color-primary`, `--color-bg`, `--color-text` |
| Spacing | `--space-small`, `--space-large` |
| Typography | `--font-family`, `--font-size-base` |
| Light/Dark mode | override in `@media (prefers-color-scheme: dark)` |

---

## 🌓 Bonus: Simple Dark Mode Example

```css
:root {
  --bg: #ffffff;
  --text: #000000;
}

@media (prefers-color-scheme: dark) {
  :root {
    --bg: #121212;
    --text: #f5f5f5;
  }
}

body {
  background: var(--bg);
  color: var(--text);
}
```

---

✨ **Pro tip:** CSS variables are reusable, themeable, and dynamic — perfect for consistent design systems and theming.
