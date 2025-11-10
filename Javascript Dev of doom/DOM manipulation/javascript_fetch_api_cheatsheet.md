# ⚡ JavaScript Fetch API Cheat Sheet

## 🧭 Basic Syntax

```js
fetch(url, options)
  .then(response => response.json()) // or .text(), .blob(), etc.
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

---

## 📥 GET Request

```js
fetch('https://api.example.com/data')
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

---

## 📤 POST Request (Send JSON)

```js
fetch('https://api.example.com/data', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({ name: 'Alice', age: 25 })
})
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

---

## ✏️ PUT / PATCH / DELETE

**PUT (replace resource):**
```js
fetch('https://api.example.com/item/1', {
  method: 'PUT',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ name: 'Updated' })
});
```

**PATCH (update partial):**
```js
fetch('https://api.example.com/item/1', {
  method: 'PATCH',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ price: 99.9 })
});
```

**DELETE:**
```js
fetch('https://api.example.com/item/1', { method: 'DELETE' });
```

---

## 🧾 Sending Form Data

```js
const formData = new FormData();
formData.append('username', 'john');
formData.append('avatar', fileInput.files[0]);

fetch('/upload', {
  method: 'POST',
  body: formData
});
```

---

## 🔐 Custom Headers & Auth

```js
fetch('/user/profile', {
  headers: {
    'Authorization': 'Bearer YOUR_TOKEN',
    'Accept': 'application/json'
  }
});
```

---

## ⚠️ Error Handling

```js
fetch('/api/data')
  .then(res => {
    if (!res.ok) throw new Error(`HTTP error! Status: ${res.status}`);
    return res.json();
  })
  .then(data => console.log(data))
  .catch(err => console.error('Fetch failed:', err));
```

---

## 🕐 Async / Await Style

```js
async function getData() {
  try {
    const res = await fetch('https://api.example.com/data');
    if (!res.ok) throw new Error(res.status);
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.error('Error:', err);
  }
}
```

---

## ⏳ Abort Fetch (Cancel Request)

```js
const controller = new AbortController();
const signal = controller.signal;

fetch('/api/slow', { signal })
  .then(res => res.json())
  .catch(err => {
    if (err.name === 'AbortError') console.log('Request aborted');
  });

// Cancel after 2 seconds
setTimeout(() => controller.abort(), 2000);
```

---

## 🧩 Common Response Methods

| Method | Description |
|--------|--------------|
| `.json()` | Parse response as JSON |
| `.text()` | Parse as text |
| `.blob()` | Binary data (images, files) |
| `.arrayBuffer()` | Low-level binary buffer |
| `.formData()` | Parse multipart form data |

---

## 🔁 Parallel & Sequential Requests

**Parallel:**
```js
Promise.all([
  fetch('/api/users').then(r => r.json()),
  fetch('/api/posts').then(r => r.json())
]).then(([users, posts]) => console.log(users, posts));
```

**Sequential:**
```js
const user = await fetch('/api/user').then(r => r.json());
const posts = await fetch(`/api/posts?user=${user.id}`).then(r => r.json());
```

---

## 🧠 Tips

- Always handle `.ok` to catch 4xx/5xx HTTP errors.
- Use `AbortController` for timeouts or user cancellations.
- `fetch` doesn’t reject on HTTP errors (only on network errors).
- Use `mode: 'cors'` for cross-origin requests if needed.
