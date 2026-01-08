# Node.js HTTP Cheat Sheet

A practical reference for creating and handling HTTP servers in Node.js.

---

## 1. Core HTTP Module

### Import
```js
const http = require("http");
```

### Create a Server
```js
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader("Content-Type", "text/plain");
  res.end("Hello World");
});
```

### Listen on a Port
```js
server.listen(3000, () => {
  console.log("Server running on http://localhost:3000");
});
```

---

## 2. Request Object (`req`)

### Common Properties
```js
req.method      // HTTP method (GET, POST, etc.)
req.url         // Request URL
req.headers     // Request headers
```

### Read Request Body
```js
let body = "";

req.on("data", chunk => {
  body += chunk;
});

req.on("end", () => {
  console.log(body);
});
```

---

## 3. Response Object (`res`)

### Set Status Code
```js
res.statusCode = 404;
```

### Set Headers
```js
res.setHeader("Content-Type", "application/json");
```

### Send Response
```js
res.end("Done");
```

### Send JSON
```js
res.end(JSON.stringify({ ok: true }));
```

---

## 4. Routing (Manual)

```js
if (req.method === "GET" && req.url === "/") {
  res.end("Home");
  return;
}

res.statusCode = 404;
res.end("Not Found");
```

---

## 5. HTTP Methods

- GET — Retrieve data
- POST — Send data
- PUT — Replace data
- PATCH — Modify data
- DELETE — Remove data

```js
req.method === "POST"
```

---

## 6. Status Codes

| Code | Meaning |
|----|----|
| 200 | OK |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Server Error |

---

## 7. JSON API Example

```js
res.setHeader("Content-Type", "application/json");
res.end(JSON.stringify({ message: "Hello API" }));
```

---

## 8. Express.js Quick Reference

### Install
```bash
npm install express
```

### Basic Server
```js
const express = require("express");
const app = express();

app.get("/", (req, res) => {
  res.send("Hello");
});

app.listen(3000);
```

---

## 9. Middleware (Express)

```js
app.use(express.json());
```

```js
app.use((req, res, next) => {
  console.log(req.method, req.url);
  next();
});
```

---

## 10. Static Files

```js
app.use(express.static("public"));
```

---

## 11. Security Basics

- Validate input
- Set proper headers
- Do not expose stack traces
- Use HTTPS in production

---

## 12. Useful Tools

- curl
- Postman
- httpie
- Nginx (reverse proxy)

---

## 13. Run Server

```bash
node server.js
```

---

## Author Notes

- Learn the `http` module first
- Use Express/Fastify for real projects
- Always handle errors and edge cases
