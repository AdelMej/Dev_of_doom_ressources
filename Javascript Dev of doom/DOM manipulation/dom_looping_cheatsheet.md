# DOM Looping Cheat Sheet

## Selecting Elements
- `document.querySelectorAll(".class")` → NodeList (loopable)
- `document.getElementsByClassName("class")` → HTMLCollection (NOT an array)
- `document.getElementsByTagName("tag")` → HTMLCollection

Convert HTMLCollection → array:
```js
const arr = Array.from(collection);
```

## Loop Types

### 1. for...of (best for NodeList)
```js
const items = document.querySelectorAll(".item");
for (const item of items) {
  console.log(item);
}
```

### 2. forEach (only works on NodeList or arrays)
```js
items.forEach(item => {
  console.log(item);
});
```

### 3. Traditional for loop
```js
for (let i = 0; i < items.length; i++) {
  console.log(items[i]);
}
```

### 4. Array methods (map, filter, etc.)
```js
Array.from(items).map(el => el.textContent);
```

## Looping and Modifying DOM
```js
const cards = document.querySelectorAll(".card");
for (const card of cards) {
  if (card.dataset.price < 50) {
    card.style.display = "none";
  }
}
```

## Looping with dataset
```js
for (const card of cards) {
  console.log(card.dataset.id);
  console.log(card.dataset.price);
}
```

## Looping Form Elements
```js
const form = document.querySelector("form");
const data = new FormData(form);

for (const [key, value] of data.entries()) {
  console.log(key, value);
}
```

## Looping Children
```js
for (const child of parentElement.children) {
  console.log(child);
}
```

