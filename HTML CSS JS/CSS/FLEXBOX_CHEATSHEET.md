
# 🚀 Flexbox Cheat Sheet (DaMini Edition)

The **no-bullshit**, actually useful reference for modern Flexbox.

---

## 🎛️ 1. Enable Flexbox

```css
.container {
  display: flex;
}
```

---

## ↔️ 2. Main Axis vs Cross Axis

- `flex-direction: row` → main axis = horizontal  
- `flex-direction: column` → main axis = vertical

```css
flex-direction: row;    /* default */
flex-direction: column;
```

---

## 📦 3. Justify Content (main axis)

```css
justify-content: flex-start;
justify-content: center;
justify-content: flex-end;
justify-content: space-between;
justify-content: space-around;
justify-content: space-evenly;
```

---

## 🎯 4. Align Items (cross axis)

```css
align-items: stretch;
align-items: center;
align-items: flex-start;
align-items: flex-end;
```

---

## 🧘 5. Center EVERYTHING

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

---

## 🧱 6. Gap

```css
gap: 20px;
```

---

## 🔀 7. Flex Wrap

```css
flex-wrap: wrap;
```

---

## ➡️ 8. Control Item Sizes

### Grow
```css
flex-grow: 1;
```

### Shrink
```css
flex-shrink: 0;
```

### Basis
```css
flex-basis: 200px;
```

### All-in-one
```css
flex: 1 0 200px;
```

---

## 🧲 9. Align Individual Items

```css
align-self: center;
```

---

## 🧱 10. Perfect Card Rows

```css
.cards {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.card {
  flex: 1 1 250px;
}
```

---

## 🔥 11. Flexbox + Media Query

```css
@media (max-width: 768px) {
  .container {
    flex-direction: column;
  }
}
```

---

## 🎉 Summary

**Use Flexbox for:**

- Navbars  
- Centering  
- Card rows  
- Buttons  
- One‑direction layouts

**Grid = structure  
Flex = content**
