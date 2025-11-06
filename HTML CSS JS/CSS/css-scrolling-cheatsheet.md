# 🧭 CSS Scrolling Cheat Sheet

## 🔹 Basic Overflow Control
| Property | Description |
|-----------|--------------|
| `overflow` | Controls both horizontal and vertical overflow. Values: `visible`, `hidden`, `scroll`, `auto`. |
| `overflow-x` | Controls horizontal overflow only. |
| `overflow-y` | Controls vertical overflow only. |

### Examples
```css
/* Hides overflow content */
.box {
  overflow: hidden;
}

/* Adds scrollbars only when necessary */
.box {
  overflow: auto;
}

/* Forces scrollbars always visible */
.box {
  overflow: scroll;
}
```

---

## 🔹 Smooth Scrolling
| Property | Description |
|-----------|--------------|
| `scroll-behavior` | Controls how scrolling happens for anchor links or JS scrolling. |

```css
html {
  scroll-behavior: smooth;
}
```

> ✅ Clicking an `<a href="#section">` will scroll smoothly instead of jumping instantly.

---

## 🔹 Scroll Snap
| Property | Description |
|-----------|--------------|
| `scroll-snap-type` | Defines how a container snaps to child elements. |
| `scroll-snap-align` | Defines where the snap happens (start, center, end). |
| `scroll-snap-stop` | Defines if scrolling must stop exactly at a snap point. |

### Example
```css
.container {
  scroll-snap-type: y mandatory;
  overflow-y: scroll;
  height: 100vh;
}

.section {
  scroll-snap-align: start;
}
```

---

## 🔹 Scrollbar Styling (WebKit-based browsers)
```css
/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-thumb {
  background-color: #888;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background-color: #555;
}
```

---

## 🔹 Sticky Scrolling Elements
| Property | Description |
|-----------|--------------|
| `position: sticky` | Keeps an element fixed **relative to its scroll container**. |
| `top`, `bottom`, `left`, `right` | Defines when it becomes sticky. |

### Example
```css
header {
  position: sticky;
  top: 0;
  background: white;
  z-index: 10;
}
```

> ✅ The header stays visible while scrolling down.

---

## 🔹 Overflow Wraps (for text)
```css
overflow-wrap: break-word;  /* Prevents text from overflowing its box */
word-break: break-all;      /* Forces long words to break */
```

---

## 🔹 Hiding Scrollbars (visually)
```css
/* Keep scroll functional, hide bar */
.box {
  overflow: auto;
  scrollbar-width: none; /* Firefox */
}

.box::-webkit-scrollbar {
  display: none; /* Chrome, Safari */
}
```

---

## 🧩 Tips
- Use `overflow: auto` instead of `scroll` for better UX.
- Combine `position: sticky` + `overflow-y: auto` for flexible layouts.
- Use `scroll-behavior: smooth` globally for a polished feel.
