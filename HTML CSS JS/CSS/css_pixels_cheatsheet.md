# 🧭 CSS Pixel Units Cheat Sheet

## 🖏 Absolute Units (Fixed Size)
These units always represent the same physical size regardless of screen resolution (mostly in print or high-precision layouts).

| Unit | Meaning | Equivalent |
|------|----------|-------------|
| `px` | Pixel | 1 pixel (screen dot) |
| `pt` | Point | 1/72 inch |
| `pc` | Pica | 12 points |
| `in` | Inch | 96 px |
| `cm` | Centimeter | 37.8 px |
| `mm` | Millimeter | 3.78 px |

🗝 **Example:**
```css
div {
  width: 200px;
  height: 100px;
}
```
This will always be **200 pixels wide**, no matter the viewport or zoom level.

---

## 🧮 Relative Units (Scale with Context)
Relative units depend on the font size or viewport dimensions.

| Unit | Relative To | Example |
|------|--------------|----------|
| `em` | Font size of the element | `2em` = 2 × element’s font-size |
| `rem` | Root element (`html`) font size | `1rem` = 16px (by default) |
| `%` | Parent element size | `width: 50%` = half the parent width |
| `vw` | 1% of viewport width | `50vw` = half of viewport width |
| `vh` | 1% of viewport height | `100vh` = full viewport height |

---

## 🎨 When to Use `px`
✅ Use `px` when you need **pixel-perfect precision**, e.g.:
- Icons and borders
- Static images or layout components
- Consistent element sizing regardless of font or viewport

❌ Avoid `px` when designing for **responsiveness**, use `%`, `em`, `rem`, `vw`, or `vh` instead.

---

## 🔍 Bonus: Device Pixel Ratio (DPR)
Modern screens (like Retina displays) may have multiple *physical pixels* per *CSS pixel*.  
So `1px` in CSS may actually use **2–4 physical pixels** for sharper rendering.

You can check DPR with JavaScript:
```js
console.log(window.devicePixelRatio);
```

