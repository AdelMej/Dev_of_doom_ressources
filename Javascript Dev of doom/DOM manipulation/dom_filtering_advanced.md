# Advanced DOM Looping & Filtering Cheat Sheet

A more complete template for real-world UI filtering, including:
- text search
- range filters (price)
- tag/category filters
- show/hide utilities
- debounce
- animations (CSS-based)

---

## 1. Base HTML Structure

```html
<input id="search" type="text" placeholder="Search..." />

<input id="price" type="range" min="0" max="500" />

<div id="tags">
  <button data-tag="wifi">Wi‑Fi</button>
  <button data-tag="parking">Parking</button>
  <button data-tag="kitchen">Kitchen</button>
</div>

<div class="place-card hidden" 
     data-name="Cozy Apartment"
     data-price="120"
     data-tags="wifi,kitchen">
  Cozy Apartment — $120
</div>

<div class="place-card hidden"
     data-name="Luxury Studio"
     data-price="300"
     data-tags="parking,wifi">
  Luxury Studio — $300
</div>

<div class="place-card hidden"
     data-name="Budget Room"
     data-price="60"
     data-tags="kitchen">
  Budget Room — $60
</div>
```

---

## 2. CSS Utilities

```css
.hidden {
  display: none;
}

.fade {
  transition: opacity 0.2s ease;
  opacity: 1;
}

.fade.hidden {
  opacity: 0;
}
```

---

## 3. JavaScript Utilities

### Debounce (prevents spam filtering)
```js
function debounce(fn, delay = 200) {
  let timeout;
  return (...args) => {
    clearTimeout(timeout);
    timeout = setTimeout(() => fn(...args), delay);
  };
}
```

---

## 4. Core Filtering Logic

```js
const searchInput = document.getElementById("search");
const priceInput = document.getElementById("price");
const tagButtons = document.querySelectorAll("#tags button");
let activeTags = new Set();

function filterPlaces() {
  const searchValue = searchInput.value.toLowerCase();
  const maxPrice = Number(priceInput.value);

  const places = document.querySelectorAll(".place-card");

  for (const place of places) {
    const name = place.dataset.name.toLowerCase();
    const price = Number(place.dataset.price);
    const tags = place.dataset.tags.split(",");

    const matchSearch = name.includes(searchValue);
    const matchPrice = price <= maxPrice;
    const matchTags = [...activeTags].every(tag => tags.includes(tag));

    if (matchSearch && matchPrice && matchTags) {
      place.classList.remove("hidden");
    } else {
      place.classList.add("hidden");
    }
  }
}
```

---

## 5. Search Input (with debounce)

```js
searchInput.addEventListener("input", debounce(filterPlaces, 150));
```

---

## 6. Price Filter

```js
priceInput.addEventListener("input", filterPlaces);
```

---

## 7. Tag Filter Buttons

```js
for (const btn of tagButtons) {
  btn.addEventListener("click", () => {
    const tag = btn.dataset.tag;

    if (activeTags.has(tag)) {
      activeTags.delete(tag);
      btn.classList.remove("active");
    } else {
      activeTags.add(tag);
      btn.classList.add("active");
    }

    filterPlaces();
  });
}
```

---

## 8. Optional: Smooth Fade-In

Add the `.fade` class on load:

```js
window.addEventListener("DOMContentLoaded", () => {
  const cards = document.querySelectorAll(".place-card");
  cards.forEach(card => card.classList.add("fade"));
});
```

---

## 9. Summary

This template gives you:

✔ Reusable `.hidden` class to toggle visibility  
✔ Debounced text search  
✔ Price filtering  
✔ Multi-tag/category filter  
✔ Smooth animations  
✔ Clean data-driven UI using `data-*` attributes  

Let me know if you want:
- sorting (price low→high, alphabetically)
- pagination
- advanced animation
- modularizing this into separate JS files
