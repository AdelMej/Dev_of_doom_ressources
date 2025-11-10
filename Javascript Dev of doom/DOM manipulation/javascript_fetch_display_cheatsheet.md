# 🧠 JavaScript Fetch + Display Cheat Sheet

## 📦 Basic Fetch

```js
fetch("https://api.example.com/data")
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error("Error:", error));
```

---

## ✅ Check for HTTP Errors

```js
fetch("https://api.example.com/data")
  .then(response => {
    if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error("Error:", error));
```

---

## 🧩 Using `async/await`

```js
async function getData() {
  try {
    const response = await fetch("https://api.example.com/data");
    if (!response.ok) throw new Error(`HTTP error! ${response.status}`);
    const data = await response.json();
    console.log(data);
  } catch (err) {
    console.error("Error:", err);
  }
}

getData();
```

---

## 🖋️ Display Data in HTML List

```html
<ul id="data-list"></ul>

<script>
fetch("https://api.example.com/items")
  .then(response => response.json())
  .then(data => {
    const list = document.querySelector("#data-list");
    data.forEach(item => {
      const li = document.createElement("li");
      li.textContent = item.name;
      list.appendChild(li);
    });
  })
  .catch(error => console.error("Error:", error));
</script>
```

---

## 🔁 Working with Nested Data

If your JSON has a structure like:
```js
{
  results: [
    { title: "Item 1" },
    { title: "Item 2" }
  ]
}
```

Then:
```js
fetch("https://api.example.com/data")
  .then(response => response.json())
  .then(data => {
    data.results.forEach(item => console.log(item.title));
  });
```

---

## 🎨 Display Data Dynamically

```html
<div id="container"></div>

<script>
fetch("https://api.example.com/users")
  .then(res => res.json())
  .then(users => {
    const container = document.getElementById("container");
    users.forEach(u => {
      const card = document.createElement("div");
      card.innerHTML = `
        <h3>${u.name}</h3>
        <p>Email: ${u.email}</p>
      `;
      container.appendChild(card);
    });
  })
  .catch(err => console.error("Error:", err));
</script>
```

---

## 🕒 Loading Indicator Example

```html
<div id="status">Loading...</div>
<ul id="list"></ul>

<script>
const status = document.getElementById("status");
const list = document.getElementById("list");

fetch("https://api.example.com/data")
  .then(res => res.json())
  .then(data => {
    status.textContent = ""; // clear loading
    data.forEach(item => {
      const li = document.createElement("li");
      li.textContent = item.name;
      list.appendChild(li);
    });
  })
  .catch(err => {
    status.textContent = "Failed to load data.";
    console.error("Error:", err);
  });
</script>
```

---

## ⚙️ Tips
- `response.json()` also returns a **Promise**, so you must `await` or `.then()` it.  
- Always check `response.ok` to handle bad HTTP status codes.  
- Use `try/catch` or `.catch()` to handle network errors.  
- Use `innerHTML` carefully — prefer `textContent` for safe text.  

---

## 🧩 Example with SWAPI
```js
fetch("https://swapi-api.hbtn.io/api/films/?format=json")
  .then(res => res.json())
  .then(data => {
    data.results.forEach(film => console.log(film.title));
  })
  .catch(err => console.error("Error:", err));
```
