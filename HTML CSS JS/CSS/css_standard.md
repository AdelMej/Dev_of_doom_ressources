# 🧩 CSS Modern Standards Cheat Sheet

## 1. Core Concepts

| Concept | Example | Notes |
|----------|----------|-------|
| **Selectors** | `.class`, `#id`, `element`, `[attr=value]`, `:hover`, `::before` | Basic targeting |
| **Box Model** | `width`, `padding`, `border`, `margin` | How elements take up space |
| **Box Sizing** | `box-sizing: border-box;` | Standard now — always include it (see universal rule below) |
| **Display** | `block`, `inline`, `inline-block`, `flex`, `grid`, `none` | Controls layout behavior |
| **Positioning** | `static`, `relative`, `absolute`, `fixed`, `sticky` | Placement control |
| **Colors** | `#fff`, `rgb(255,255,255)`, `hsl(0, 0%, 100%)`, `var(--color)` | Use `var()` for custom properties |
| **Units** | `px`, `em`, `rem`, `%`, `vw/vh`, `fr` | `rem` and `fr` are modern favorites |

---

## 2. Text & Fonts

| Property | Example | Notes |
|-----------|----------|-------|
| `font-family` | `"Helvetica Neue", Arial, sans-serif` | Always include fallbacks |
| `font-size` | `16px` or `1rem` | `rem` scales with user settings |
| `font-weight` | `400`, `bold` | Use numbers for precision |
| `line-height` | `1.5` | Controls text spacing |
| `text-align` | `left`, `center`, `right`, `justify` | Horizontal alignment |
| `text-decoration` | `none`, `underline` | For links, etc. |
| `color` | `var(--text-color)` | Use CSS variables for consistency |

---

## 3. Layout (Modern Standards)

| Feature | Use Case | Notes |
|----------|-----------|-------|
| **Flexbox** | Centering, alignment, rows/columns | `display: flex;` — modern standard |
| **Grid** | Complex layouts | `display: grid;` — powerful and clean |
| **Gap** | `gap: 1rem;` | Works in both flex and grid |
| **Normalize.css / Reset.css** | Makes CSS consistent across browsers | Always include one of them |
| **Container Queries** | Responsive components | `@container` (new but growing support) |

---

## 4. CSS Variables (Custom Properties)

```css
:root {
  --color-primary: #d73953;
  --color-black: #090909;
  --color-white: #ffffff;
  --color-light-grey: #f3f3f3;
  --color-dark-grey: #353535;
  --text-color: var(--color-black);
}
a {
  color: var(--color-primary);
}
```

✅ Standardized, used everywhere. Great for themes and consistency.

---

## 5. Transitions & Animations

| Property | Example | Notes |
|-----------|----------|-------|
| `transition` | `all 0.3s ease-in-out;` | Smooth interactions |
| `@keyframes` | `@keyframes fade { from {opacity: 0;} to {opacity: 1;} }` | Animations |
| `animation` | `fade 1s ease-in;` | Apply animations |

---

## 6. Pseudo-Classes & Pseudo-Elements

| Type | Example | Description |
|------|----------|-------------|
| Pseudo-class | `:hover`, `:focus`, `:active`, `:nth-child()` | React to user actions |
| Pseudo-element | `::before`, `::after`, `::marker`, `::selection` | Add virtual elements |

---

## 7. Responsive Design

| Property | Example | Notes |
|-----------|----------|-------|
| `@media` | `@media (max-width: 768px) { ... }` | Responsive breakpoints |
| `max-width` / `min-width` | `max-width: 1200px;` | Prevent stretching |
| `%` / `vw` / `vh` | Relative sizing | Use for fluid layouts |

---

## 8. Tools & Ecosystem

| Tool / File | Purpose |
|--------------|----------|
| **normalize.css** | Fixes browser inconsistencies |
| **stylelint** | Keeps CSS consistent |
| **Prettier** | Auto-formats |
| **PostCSS / Autoprefixer** | Adds vendor prefixes automatically |
| **CSS Variables** | For theming and DRY design |
| **Flexbox + Grid** | Modern layout engines |

---

## ✨ Universal Box-Sizing (Don't skip this)

Make **all** elements use the `border-box` sizing model — this is the standard "box-sizing trick" you always add at the top of your CSS to avoid weird width math:

```css
/* Universal box-sizing: include padding & borders in width/height */
*, *::before, *::after {
  box-sizing: border-box;
}
```

Put this **before** any other layout rules, typically right after importing normalize.css.

---

## Example Base Setup (with normalize + box-sizing)

```css
/* 1) Normalize browsers */
@import url('https://necolas.github.io/normalize.css/8.0.1/normalize.css');

/* 2) Box-sizing rule (universal) */
*, *::before, *::after {
  box-sizing: border-box;
}

/* 3) CSS variables */
:root {
  --color-primary: #d73953;
  --text-color: #090909;
  --font-family-base: "Helvetica Neue", Helvetica, Arial, sans-serif;
  --font-family-title: "Helvetica Neue", Helvetica, Arial, sans-serif;
  --line-height-base: 1.6;
}

/* 4) Base html/body */
html {
  scroll-behavior: smooth;
  font-family: var(--font-family-base);
  color: var(--text-color);
  line-height: var(--line-height-base);
}

/* 5) Links only if they have href */
a[href] {
  color: var(--color-primary);
  text-decoration: none;
}
a[href]:hover {
  text-decoration: underline;
}
```

---

**Pro Tip:** Add the universal box-sizing early and you’ll never fight mysterious width math again.

---

