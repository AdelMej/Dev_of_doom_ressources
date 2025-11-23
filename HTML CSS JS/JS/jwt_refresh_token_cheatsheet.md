# JWT Refresh Token Cheat Sheet (Frontend + Backend)

This guide explains how to implement **refresh tokens** safely using:
- **Short-lived access tokens (JWT)** stored in memory
- **Long-lived refresh tokens** stored in **HTTP-only cookies**
- A `/refresh` route to obtain new access tokens
- Automatic frontend token refreshing

---

# ⚙️ Backend Implementation (Flask + JWT)

## 1. **Issue Refresh Token on Login**

Example (Flask-JWT-Extended):
```python
access_token = create_access_token(identity=user.id, additional_claims={"is_admin": user.is_admin})
refresh_token = create_refresh_token(identity=user.id)

response = jsonify({"access_token": access_token})
set_refresh_cookies(response, refresh_token)
return response, 200
```

### Why HTTP-only cookie?
- JS cannot access it ⇒ safe
- Automatically sent with `/refresh`

---

## 2. **Refresh Route**

```python
@api.route('/refresh')
class Refresh(Resource):
    @jwt_required(refresh=True)
    def post(self):
        user_id = get_jwt_identity()
        claims = get_jwt()

        new_access = create_access_token(
            identity=user_id,
            additional_claims={"is_admin": claims.get("is_admin")}
        )

        return {"access_token": new_access}, 200
```

No refresh token returned — it's already in the cookie.

---

## 3. **Logout / Revoke**

```python
@api.route('/logout')
class Logout(Resource):
    def post(self):
        response = jsonify({"msg": "logged out"})
        unset_jwt_cookies(response)
        return response, 200
```

---

# 🎨 Frontend Implementation (JavaScript)

## 1. **Store Access Token In Memory Only**

```js
let accessToken = null;
```

Never store it in `localStorage`.

---

## 2. **Login**

```js
async function login(email, password) {
    const res = await fetch("/login", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ email, password }),
        credentials: "include" // <= send & receive cookies
    });

    const data = await res.json();
    accessToken = data.access_token;
}
```

---

## 3. **Attach Access Token to Requests**

```js
function authHeaders() {
    return accessToken ? { Authorization: `Bearer ${accessToken}` } : {};
}
```

---

## 4. **Auto-Refresh Wrapper (The Magic Sauce)**

```js
async function apiFetch(url, options = {}) {
    if (!options.headers) options.headers = {};
    Object.assign(options.headers, authHeaders());
    options.credentials = "include";

    let res = await fetch(url, options);

    if (res.status === 401) {
        const refreshed = await refreshToken();
        if (!refreshed) return res; // refresh failed

        options.headers = authHeaders();
        res = await fetch(url, options); // retry
    }

    return res;
}
```

---

## 5. **Refresh Token Function**

```js
async function refreshToken() {
    const res = await fetch("/refresh", {
        method: "POST",
        credentials: "include" // send cookie
    });

    if (!res.ok) {
        accessToken = null;
        return false;
    }

    const data = await res.json();
    accessToken = data.access_token;
    return true;
}
```

---

## 6. **Logout**

```js
async function logout() {
    await fetch("/logout", {
        method: "POST",
        credentials: "include"
    });

    accessToken = null;
}
```

---

# 🔐 Flow Summary

### **Login:**
- Get access token (short-lived)
- Receive refresh cookie (httpOnly)
- Store access token in memory

### **Every request:**
- If 401 ⇒ auto-call `/refresh`
- Replace access token
- Retry request

### **Logout:**
- Clear refresh cookie
- Clear access token

---

# 🚀 Final Notes
- Refresh tokens stay server-deletable because they're cookies.
- No XSS can steal them.
- Access tokens stay short-lived.
- The workflow stays clean AF.

This is the standard production architecture used by:
- Google
- GitHub
- Amazon
- Modern SPAs

---