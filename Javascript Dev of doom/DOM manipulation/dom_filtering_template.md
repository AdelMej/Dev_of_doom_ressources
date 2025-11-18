# DOM Filtering System Template (with .hidden class)

## CSS
```css
.hidden {
    display: none;
}
```

## HTML Example
```html
<div class="place-card"
     data-price="79"
     data-name="cozy cottage"
     data-amenities="wifi,parking,kitchen">
    <h3>Cozy Cottage</h3>
    <p>Price: 79€</p>
</div>

<div class="place-card"
     data-price="120"
     data-name="luxury villa"
     data-amenities="wifi,pool,garden">
    <h3>Luxury Villa</h3>
    <p>Price: 120€</p>
</div>

<input id="priceFilter" type="number" placeholder="Max price…">
<input id="nameFilter" type="text" placeholder="Search name…">
```

## JavaScript (filter.js)
```js
const cards = document.querySelectorAll(".place-card");

function applyFilters() {
    const maxPrice = document.getElementById("priceFilter").value;
    const keyword = document.getElementById("nameFilter").value.toLowerCase();

    for (const card of cards) {
        const cardPrice = Number(card.dataset.price);
        const cardName = card.dataset.name.toLowerCase();

        let show = true;

        if (maxPrice && cardPrice > maxPrice) show = false;
        if (keyword && !cardName.includes(keyword)) show = false;

        if (show) card.classList.remove("hidden");
        else card.classList.add("hidden");
    }
}

document.getElementById("priceFilter").addEventListener("input", applyFilters);
document.getElementById("nameFilter").addEventListener("input", applyFilters);
```

## How It Works
- `.hidden` class controls visibility
- JavaScript applies filters like price and text search
- Logic stays clean and scalable
