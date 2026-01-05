
# Flask JWT Cookie Integration Guide

## 1. Overview
This guide explains how to use **Flask-JWT-Extended** with **HTTP-only cookies** for secure storage of refresh (and optionally access) tokens.

---

## 2. Installation

```bash
pip install flask-jwt-extended
```

---

## 3. Basic Configuration

```python
from flask import Flask
from flask_jwt_extended import JWTManager

app = Flask(__name__)
app.config["JWT_SECRET_KEY"] = "super-secret"

# Enable cookies for tokens
app.config["JWT_TOKEN_LOCATION"] = ["cookies"]

# Cookies should be HTTP-only (secure)
app.config["JWT_COOKIE_SECURE"] = False  # True in production (HTTPS)
app.config["JWT_COOKIE_HTTPONLY"] = True

# Optional: allow reading cookies from JS (not recommended)
# app.config["JWT_COOKIE_SAMESITE"] = "None"

jwt = JWTManager(app)
```

---

## 4. Setting Tokens in Cookies

```python
from flask_jwt_extended import create_access_token, create_refresh_token, set_access_cookies, set_refresh_cookies
from flask import jsonify

@app.post("/login")
def login():
    user_id = 123
    access = create_access_token(identity=user_id)
    refresh = create_refresh_token(identity=user_id)

    response = jsonify({"msg": "Logged in"})
    set_access_cookies(response, access)
    set_refresh_cookies(response, refresh)
    return response
```

---

## 5. Protecting Routes Using JWT Cookies

```python
from flask_jwt_extended import jwt_required, get_jwt_identity

@app.get("/profile")
@jwt_required()
def profile():
    return {"user": get_jwt_identity()}
```

This reads the **access token** automatically from the cookie.

---

## 6. Refreshing Tokens

```python
from flask_jwt_extended import jwt_required

@app.post("/refresh")
@jwt_required(refresh=True)
def refresh():
    identity = get_jwt_identity()
    new_access = create_access_token(identity=identity)

    response = jsonify({"msg": "Token refreshed"})
    set_access_cookies(response, new_access)
    return response
```

---

## 7. Clearing Cookies (Logout)

```python
from flask_jwt_extended import unset_jwt_cookies

@app.post("/logout")
def logout():
    response = jsonify({"msg": "Logged out"})
    unset_jwt_cookies(response)
    return response
```

---

## 8. JavaScript Integration Notes

### Access token
Not readable from JS (because HTTP-only).  
You simply send a normal request:

```js
fetch("/profile", {
  method: "GET",
  credentials: "include"
});
```

### Refresh token
Same as above, NOT readable.  
The endpoint `/refresh` automatically reads the cookie.

---

## 9. When to Use This Approach
Use **HTTP-only cookies** if:
- You want maximum security.
- You want no access to tokens from JS.
- Your frontend uses `fetch(..., { credentials: "include" })`.

Use **memory/localStorage** if:
- You're doing SPA-style manual token handling.
- You need to read the access token in JS.

---

## 10. Summary

| Storage method     | Security | JS Readable | Ideal Use Case |
|-------------------|----------|-------------|----------------|
| HTTP-only cookie  | ⭐⭐⭐⭐⭐ | ❌ No | Most secure apps |
| Memory (JS)       | ⭐⭐⭐ | ✔ Yes | SPAs with silent refresh |
| localStorage       | ⭐⭐ | ✔ Yes | Low security requirement |

---

