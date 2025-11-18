# JWT Tokens in JavaScript — Cheat Sheet

A compact guide to **creating**, **storing**, **sending**, and **validating** JWT tokens in JavaScript.

---

## 🔑 What is a JWT?
A **JSON Web Token (JWT)** is a signed string used to authenticate users.  
Example format:  
`xxxxx.yyyyy.zzzzz`  
- **Header** (algorithm + type)  
- **Payload** (claims: user ID, roles, expiry…)  
- **Signature** (verifies authenticity)

---

## ✔️ 1. Sending a Login Request (Get JWT)

```js
async function login(email, password) {
    const response = await fetch("http://localhost:5000/api/v1/auth/login", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ email, password })
    });

    if (!response.ok) {
        throw new Error("Login failed");
    }

    const data = await response.json();
    return data.access_token;
}
```

---

## 💾 2. Storing the Token

### **LocalStorage (simple, less secure)**

```js
localStorage.setItem("token", jwt);
```

### **Cookie (safer, if httpOnly on server)**

```js
document.cookie = `token=${jwt}; path=/; max-age=86400; samesite=strict;`;
```

👉 Prefer **httpOnly cookies** when possible (JavaScript can't read them — prevents XSS theft).

---

## 📤 3. Sending Authenticated Requests

```js
async function apiGet(url) {
    const token = localStorage.getItem("token"); // or cookie parser

    const response = await fetch(url, {
        headers: {
            "Authorization": `Bearer ${token}`
        }
    });

    if (response.status === 401) {
        console.log("Token invalid or expired.");
    }

    return response.json();
}
```

---

## 🔒 4. Checking if Token Exists

```js
const token = localStorage.getItem("token");
if (!token) {
    window.location.href = "/login.html";
}
```

---

## 🧪 5. Decoding a JWT (Client-side)

⚠️ *Only for reading — NEVER trust decoded client-side values for security.*

```js
function decodeJWT(token) {
    const base64 = token.split(".")[1].replace(/-/g, "+").replace(/_/g, "/");
    return JSON.parse(atob(base64));
}
```

---

## ⏲️ 6. Checking Expiry

```js
function isExpired(token) {
    const { exp } = decodeJWT(token);
    return Date.now() >= exp * 1000;
}
```

---

## ❌ 7. Logging Out

```js
localStorage.removeItem("token");
document.cookie = "token=; max-age=0; path=/;";
```

---

## 📝 Summary Table

| Task | Code |
|------|------|
| Get JWT from login | `fetch(...).json()` |
| Store | `localStorage.setItem` / `cookie` |
| Send authenticated request | `Authorization: Bearer <token>` |
| Decode | `JSON.parse(atob(...))` |
| Check expiry | compare `exp` |
| Logout | remove token |

---

## 💡 Recommended Practice
- Use **httpOnly cookies** for production.  
- Refresh tokens should be handled server-side.  
- Never trust client-decoded claims.

---

Happy authentication, soldier. 🔐🔥  
