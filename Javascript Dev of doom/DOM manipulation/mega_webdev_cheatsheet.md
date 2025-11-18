# Mega Web Dev Cheat Sheet
A combined cheat sheet for **DOM Manipulation**, **Datasets**, **Forms**, and **Async Fetch + JWT**.

---

# 🧩 1. DOM Manipulation

## Selecting Elements
```js
document.querySelector("css");
document.querySelectorAll("css");
document.getElementById("id");
document.getElementsByClassName("class");
document.getElementsByTagName("tag");
```

## Creating Elements
```js
const div = document.createElement("div");
const p = document.createElement("p");
```

## Adding / Removing
```js
parent.appendChild(child);
element.remove();
element.replaceWith(newNode);
element.append("text", node);
element.prepend(node);
```

## Classes
```js
element.classList.add("active");
element.classList.remove("hidden");
element.classList.toggle("open");
```

## Attributes
```js
element.setAttribute("role", "button");
element.getAttribute("role");
element.removeAttribute("role");
```

## Insert HTML
```js
element.innerHTML = "<p>Hello</p>";
element.textContent = "Hello";

element.insertAdjacentHTML("beforeend", "<p>Inserted!</p>");
```

---

# 🏷️ 2. Dataset API
Store custom metadata on elements.

## Set dataset
```js
element.dataset.price = "120";
element.dataset.id = "f92a-22b";
```

## Get dataset
```js
console.log(element.dataset.price);
console.log(element.dataset.id);
```

## Example HTML
```html
<div class="place-card" data-price="89" data-id="123"></div>
```

---

# 📝 3. Forms + FormData

## Get form data
```js
const form = document.getElementById("login-form");
const data = new FormData(form);

const email = data.get("email");
const password = data.get("password");
```

## Convert FormData → JS object
```js
const obj = Object.fromEntries(data.entries());
```

## Prevent submit reload
```js
form.addEventListener("submit", e => {
    e.preventDefault();
});
```

---

# 🌐 4. Async Fetch (GET / POST)

## Basic GET
```js
const data = await fetch("/api/places").then(r => r.json());
```

## Basic POST (JSON)
```js
await fetch("/api/login", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ email, password })
});
```

## Handling errors
```js
const res = await fetch(url);

if (!res.ok) {
    console.error("HTTP Error:", res.status);
    return;
}
```

---

# 🔐 5. JWT in JavaScript

## Save JWT (cookie or localStorage)

### Option A — Cookie
```js
document.cookie = "token=JWT_VALUE; path=/; max-age=86400; samesite=strict";
```

### Option B — LocalStorage
```js
localStorage.setItem("token", jwt);
```

## Read JWT

### Cookie
```js
function getCookie(name) {
    return document.cookie
        .split("; ")
        .find(row => row.startsWith(name + "="))
        ?.split("=")[1];
}
```

### LocalStorage
```js
const token = localStorage.getItem("token");
```

## Authenticated Fetch
```js
await fetch("/api/protected", {
    headers: {
        "Authorization": "Bearer " + token
    }
});
```

---

# 🔄 6. DOM Looping Patterns

## Loop through NodeList
```js
document.querySelectorAll(".card").forEach(card => {
    console.log(card.dataset.id);
});
```

## Filter DOM elements
```js
const maxPrice = 100;

document.querySelectorAll(".place-card").forEach(card => {
    if (Number(card.dataset.price) > maxPrice) {
        card.style.display = "none";
    } else {
        card.style.display = "";
    }
});
```

---

# 🎯 Summary

If you know:
- `querySelector`, `createElement`
- `classList`
- `dataset`
- `FormData`
- `fetch` + async/await
- JWT handling (cookie/localStorage)

You can build **any front-end for any API**.

