# 📘 CSS Positioning Cheat Sheet

## 🧩 Position Values

| Property Value | Description |
|----------------|-------------|
| `static` | Default position for all elements. Not affected by `top`, `right`, `bottom`, or `left`. |
| `relative` | Positioned **relative to its normal position**. Offsets (`top`, `left`, etc.) shift it from where it would normally be. |
| `absolute` | Positioned **relative to the nearest positioned ancestor** (not `static`). Removed from the normal document flow. |
| `fixed` | Positioned **relative to the viewport**. Stays fixed when scrolling. |
| `sticky` | Acts like `relative` until a scroll threshold is reached, then behaves like `fixed`. |

---

## 🎯 Offset Properties

| Property | Description |
|-----------|--------------|
| `top` | Moves the element down (positive value) or up (negative value). |
| `right` | Moves the element left (positive) or right (negative). |
| `bottom` | Moves the element up (positive) or down (negative). |
| `left` | Moves the element right (positive) or left (negative). |

---

## 🧱 Z-Index (Stacking)

| Property | Description |
|-----------|--------------|
| `z-index` | Controls the stack order (which element appears on top). Only works on positioned elements (`relative`, `absolute`, `fixed`, `sticky`). |

---

## 🧭 Examples

```css
/* Relative: offset from normal position */
.box1 {
  position: relative;
  top: 20px;
  left: 10px;
}

/* Absolute: positioned relative to parent */
.container {
  position: relative; /* anchor for absolute children */
}
.box2 {
  position: absolute;
  top: 0;
  right: 0;
}

/* Fixed: always visible at the same spot */
.navbar {
  position: fixed;
  top: 0;
  width: 100%;
}

/* Sticky: scrolls until it sticks */
.header {
  position: sticky;
  top: 0;
  background: white;
}
```

---

## 🧠 Tips

- If `position: absolute` has **no positioned ancestor**, it uses the **document `<html>`** as reference.  
- `z-index` defaults to `auto` — same stacking order as in the DOM.  
- For `sticky` to work, the parent element **must not have `overflow: hidden` or `overflow: auto`**.  
- `transform`, `filter`, or `opacity < 1` create new stacking contexts.
