# 🎨 CSS Cheat Sheet (Quick Reference)

## 🔹 Selectors
| Type | Example | Description |
|------|----------|-------------|
| Element | `div {}` | Targets all `<div>` elements |
| Class | `.container {}` | Targets elements with `class="container"` |
| ID | `#main {}` | Targets the element with `id="main"` |
| Descendant | `div p {}` | Targets `<p>` inside `<div>` |
| Child | `div > p {}` | Targets **direct** `<p>` children of `<div>` |
| Sibling | `h2 + p {}` | Targets `<p>` immediately after `<h2>` |
| Attribute | `input[type="text"] {}` | Targets inputs with `type="text"` |
| Pseudo-class | `a:hover {}` | Targets links when hovered |
| Pseudo-element | `p::first-line {}` | Styles the first line of a paragraph |

---

## 🧱 Box Model
Each element has:  
`margin` → `border` → `padding` → `content`

| Property | Description |
|-----------|--------------|
| `margin` | Space **outside** the element |
| `border` | Line around the element |
| `padding` | Space **inside** the border |
| `width`, `height` | Content size |
| `box-sizing: border-box;` | Includes border & padding in total width/height |

---

## 🎯 Positioning
| Property | Values | Notes |
|-----------|---------|-------|
| `position` | `static` (default), `relative`, `absolute`, `fixed`, `sticky` | Changes how an element is placed |
| `top`, `right`, `bottom`, `left` | Used with relative/absolute/fixed positioning |
| `z-index` | Controls stacking order |

---

## ⚙️ Display & Layout
| Property | Description |
|-----------|-------------|
| `display: block` | Element takes full width |
| `display: inline` | Element fits content width |
| `display: inline-block` | Inline + respects height/width |
| `display: flex` | Enables Flexbox |
| `display: grid` | Enables Grid layout |
| `visibility: hidden` | Hides element but keeps space |
| `opacity: 0` | Makes element invisible (but interactive) |

---

## 🧭 Flexbox Essentials
```css
.container {
  display: flex;
  flex-direction: row; /* or column */
  justify-content: center; /* horizontal */
  align-items: center; /* vertical */
  flex-wrap: wrap;
  gap: 1rem;
}
.item {
  flex: 1; /* Grow/shrink equally */
  align-self: flex-start;
}
```

---

## 🧩 Grid Basics
```css
.container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
}
.item {
  grid-column: 1 / 3; /* span columns */
}
```

---

## 🖋️ Typography
```css
font-family: 'Roboto', sans-serif;
font-size: 16px;
font-weight: 400;
line-height: 1.5;
text-align: center;
text-transform: uppercase;
letter-spacing: 1px;
```

---

## 🎨 Colors & Background
```css
color: #333;
background-color: #f5f5f5;
background-image: url("bg.jpg");
background-size: cover;
background-position: center;
```

---

## 💡 Borders & Shadows
```css
border: 1px solid #ccc;
border-radius: 8px;
box-shadow: 0 4px 8px rgba(0,0,0,0.1);
```

---

## 🌀 Transitions & Animations
```css
transition: all 0.3s ease-in-out;
```

```css
@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}
.element {
  animation: fadeIn 1s ease-in;
}
```

---

## 📱 Responsive Design
```css
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }
}
```

---

## 🧍‍♂️ Common Reset
```css
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
```
