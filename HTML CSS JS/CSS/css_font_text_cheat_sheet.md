# 🎨 CSS Font & Text Cheat Sheet

## 🧱 Font Family & Basics

```css
body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 16px;
  font-weight: 400;
  font-style: normal;
  font-variant: small-caps;
  line-height: 1.6;
}
```

---

## 🔤 Font Size Units

| Unit | Description | Example |
|------|--------------|----------|
| `px` | Pixels (fixed size) | `font-size: 16px;` |
| `em` | Relative to parent | `font-size: 1.2em;` |
| `rem` | Relative to root element | `font-size: 1rem;` |
| `%` | Relative to parent font-size | `font-size: 120%;` |
| `vw/vh` | Relative to viewport width/height | `font-size: 2vw;` |

---

## ✍️ Font Weight

```css
font-weight: 100;  /* thin */
font-weight: 400;  /* normal */
font-weight: 700;  /* bold */
font-weight: bold; /* same as 700 */
```

---

## ⚙️ Text Alignment

```css
text-align: left;
text-align: center;
text-align: right;
text-align: justify;
```

---

## 🧭 Text Decoration

```css
text-decoration: underline;
text-decoration: overline;
text-decoration: line-through;
text-decoration: none;
text-decoration-style: wavy;
text-decoration-color: #d73953;
text-underline-offset: 4px;
```

**Example (modern links):**
```css
a {
  text-decoration: underline;
  text-decoration-color: #d73953;
  text-underline-offset: 3px;
}
a:hover {
  text-decoration-style: wavy;
}
```

---

## 🔠 Text Transform

```css
text-transform: uppercase;
text-transform: lowercase;
text-transform: capitalize;
```

---

## 🔡 Letter & Word Spacing

```css
letter-spacing: 1px;
word-spacing: 5px;
```

---

## 🪞 Text Shadow

```css
text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4);
```

---

## 🧍 Vertical Alignment

```css
vertical-align: baseline; /* top | middle | bottom | sub | super */
```

---

## 🧩 Overflow & Wrapping

```css
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```

---

## 🎭 Custom Fonts

**Google Fonts example:**
```css
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

body {
  font-family: 'Roboto', sans-serif;
}
```

**Local font example:**
```css
@font-face {
  font-family: "MyFont";
  src: url("fonts/MyFont.woff2") format("woff2");
}
```

---

## 💡 Quick Reference Table

| Property | Description | Example |
|-----------|--------------|----------|
| `font-family` | Font style(s) | `"Arial", sans-serif` |
| `font-size` | Text size | `16px`, `1.2rem` |
| `font-weight` | Thickness | `400`, `bold` |
| `font-style` | Italic or normal | `italic` |
| `text-align` | Alignment | `center` |
| `text-transform` | Capitalization | `uppercase` |
| `text-decoration` | Underline, etc. | `underline wavy` |
| `letter-spacing` | Letter distance | `2px` |
| `line-height` | Line spacing | `1.5` |
| `color` | Text color | `#d73953` |
