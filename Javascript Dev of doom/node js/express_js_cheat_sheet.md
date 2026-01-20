# Express.js Cheat Sheet 🚀

Minimal, explicit, no-magic Express reference.

---

## Install
```bash
npm init -y
npm install express
```

---

## Basic Server
```js
const express = require('express');

const app = express();
const PORT = 1245;

app.listen(PORT);

module.exports = app;
```

---

## Routes

### GET
```js
app.get('/', (req, res) => {
  res.type('text').send('Hello');
});
```

### POST
```js
app.post('/users', (req, res) => {
  res.send('Created');
});
```

### Multiple routes
```js
app.get('/a', handlerA);
app.get('/b', handlerB);
```

---

## Route matching rules

| Pattern | Matches |
|------|------|
| `app.get('/')` | ONLY `/` |
| `app.get('/students')` | ONLY `/students` |
| `app.use()` | EVERYTHING |
| `app.get('*')` | EVERYTHING (GET only) |

---

## 404 Handler (explicit)
```js
app.use((req, res) => {
  res.status(404).type('text').send('Not found');
});
```

Must be LAST.

---

## Middleware

### Simple middleware
```js
function logger(req, res, next) {
  console.log(req.method, req.url);
  next();
}

app.use(logger);
```

### Route-level middleware
```js
app.get('/secure', authMiddleware, handler);
```

---

## Request Object (`req`)

```js
req.url
req.method
req.headers
req.params
req.query
req.body
```

---

## Response Object (`res`)

```js
res.status(200)
res.type('text')
res.send('OK')
res.json({ ok: true })
res.end()
```

---

## Async routes
```js
app.get('/data', async (req, res) => {
  const result = await getData();
  res.send(result);
});
```

Errors must be caught manually unless you use next(err).

---

## Error handling
```js
app.use((err, req, res, next) => {
  res.status(500).send(err.message);
});
```

Must have 4 parameters.

---

## Exporting app (for tests)
```js
module.exports = app;
```

DO NOT export `app.listen()`.

---

## Common Mistakes ❌

❌ Using `app.use()` to respond  
❌ Responding outside route handlers  
❌ Exporting the server instead of app  
❌ Forgetting 404 handler  

---

## Clean Layout (recommended)

```js
// public routes
app.get('/', handler);

// protected routes
app.get('/students', handler);

// fallback
app.use(notFoundHandler);
```

---

## Mental Model 🧠

Requests flow **top → bottom**  
First matching handler wins  
If nobody responds → 404

---

Built to avoid Holberton traps.
