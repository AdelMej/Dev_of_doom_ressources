
# 🚀 CSS Grid Cheat Sheet (DaMini Edition)

The **no-bullshit**, actually useful reference for modern CSS Grid.

---

## 🎛️ 1. Enable Grid

```css
.container {
  display: grid;
}
```

---

## 📐 2. Define Columns & Rows

```css
grid-template-columns: 1fr 1fr 1fr;   /* 3 equal columns */
grid-template-columns: 200px 1fr;     /* fixed + fluid */
grid-template-rows: auto 1fr auto;    /* header, body, footer */
```

**Units you’ll use:**
- `px` → fixed
- `fr` → fraction of available space (BEST)
- `auto` → size based on content

---

## 🧱 3. Gaps

```css
gap: 20px;
column-gap: 10px;
row-gap: 30px;
```

---

## 🗺️ 4. Template Areas (The Cheat Code)

### Define layout:

```css
.container {
  display: grid;
  grid-template-areas:
    "nav nav nav"
    "side content ads"
    "footer footer footer";
}
```

### Assign areas:

```css
nav     { grid-area: nav; }
aside   { grid-area: side; }
main    { grid-area: content; }
.ad     { grid-area: ads; }
footer  { grid-area: footer; }
```

---

## 🎯 5. Manual Placement

```css
.item {
  grid-column: 1 / 3;  /* spans col 1 → 2 */
  grid-row: 2 / 4;     /* spans row 2 → 3 */
}
```

---

## ♻️ 6. Repeat()

```css
grid-template-columns: repeat(3, 1fr);
grid-template-columns: repeat(4, 200px);
```

---

## 🧲 7. Auto-fill / Auto-fit (Responsive Cards)

```css
grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
```

---

## 🔄 8. Alignment

### Whole Grid:
```css
justify-items: center;
align-items: center;
```

### Single Item:
```css
.item {
  justify-self: end;
  align-self: start;
}
```

---

## 📱 9. Media Queries + Grid = Ultra Cheat Mode

```css
@media (max-width: 768px) {
  .container {
    grid-template-areas:
      "nav"
      "content"
      "side"
      "ads"
      "footer";
    grid-template-columns: 1fr;
  }
}
```

---

## 🔥 10. Full Example

```css
.container {
  display: grid;
  grid-template-columns: 200px 1fr 200px;
  grid-template-areas:
    "nav nav nav"
    "side content ads"
    "footer footer footer";
  gap: 20px;
}

nav     { grid-area: nav; }
aside   { grid-area: side; }
main    { grid-area: content; }
.ad     { grid-area: ads; }
footer  { grid-area: footer; }

@media (max-width: 768px) {
  .container {
    grid-template-columns: 1fr;
    grid-template-areas:
      "nav"
      "content"
      "side"
      "ads"
      "footer";
  }
}
```

---

## 🎉 Summary

**Grid = layout god mode**  
Use it for big structure.  
Combine with Flexbox for internal content.

DaMini-scss is about to be cracked af kekw.
