# 🧠 **Flask-JWT-Extended Cheat Sheet**

---

## ⚙️ Install

```bash
pip install Flask-JWT-Extended
```

---

## 🚀 Basic Setup

```python
from flask import Flask, jsonify, request
from flask_jwt_extended import (
    JWTManager, create_access_token,
    jwt_required, get_jwt_identity
)

app = Flask(__name__)
app.config["JWT_SECRET_KEY"] = "super-secret-key"  # Change this in production!

jwt = JWTManager(app)
```

---

## 🔑 Create Tokens (Login)

```python
@app.route("/login", methods=["POST"])
def login():
    username = request.json.get("username")
    password = request.json.get("password")

    # Example: verify credentials manually
    if username != "admin" or password != "1234":
        return jsonify({"msg": "Bad credentials"}), 401

    access_token = create_access_token(identity=username)
    return jsonify(access_token=access_token)
```

---

## 🧩 Protect Routes

```python
@app.route("/protected", methods=["GET"])
@jwt_required()
def protected():
    current_user = get_jwt_identity()
    return jsonify(logged_in_as=current_user)
```

### Example request

```bash
curl -H "Authorization: Bearer <your_token>" http://localhost:5000/protected
```

---

## 🧭 Get Current User

```python
from flask_jwt_extended import get_jwt_identity

@jwt_required()
def route():
    user = get_jwt_identity()
```

---

## ⏱️ Token Expiration

```python
from datetime import timedelta

app.config["JWT_ACCESS_TOKEN_EXPIRES"] = timedelta(minutes=30)
```

---

## 🔁 Refresh Tokens

```python
from flask_jwt_extended import create_refresh_token, jwt_required, get_jwt_identity, create_access_token

@app.route("/login", methods=["POST"])
def login():
    username = request.json.get("username")
    password = request.json.get("password")
    if username != "admin" or password != "1234":
        return jsonify({"msg": "Bad credentials"}), 401

    access_token = create_access_token(identity=username)
    refresh_token = create_refresh_token(identity=username)
    return jsonify(access_token=access_token, refresh_token=refresh_token)
```

### Refresh endpoint

```python
from flask_jwt_extended import jwt_required

@app.route("/refresh", methods=["POST"])
@jwt_required(refresh=True)
def refresh():
    current_user = get_jwt_identity()
    new_access_token = create_access_token(identity=current_user)
    return jsonify(access_token=new_access_token)
```

---

## 🍪 Optional: Store JWT in Cookies (safer for browsers)

```python
app.config["JWT_TOKEN_LOCATION"] = ["cookies"]
app.config["JWT_COOKIE_SECURE"] = False  # True for HTTPS only
app.config["JWT_ACCESS_COOKIE_NAME"] = "access_token_cookie"
```

Then use:

```python
set_access_cookies(response, access_token)
unset_jwt_cookies(response)
```

---

## 🧰 Custom Claims (add roles or permissions)

```python
@jwt.additional_claims_loader
def add_claims(identity):
    return {"role": "admin", "active": True}
```

Access it later:

```python
from flask_jwt_extended import get_jwt

@jwt_required()
def protected():
    claims = get_jwt()
    return jsonify(role=claims["role"])
```

---

## 🚨 Error Handling

```python
@jwt.invalid_token_loader
def invalid_token_callback(error):
    return jsonify({"msg": "Invalid token"}), 401

@jwt.expired_token_loader
def expired_token_callback(jwt_header, jwt_payload):
    return jsonify({"msg": "Token expired"}), 401
```

---

## 🧪 Test Example (Quick Run)

```python
if __name__ == "__main__":
	app.run(debug=True)
```

1. Run the server
2. POST to `/login` with JSON `{"username": "admin", "password": "1234"}`
3. Copy the `access_token`
4. GET `/protected` with
    `Authorization: Bearer <token>`

---

## 🔒 Summary of Core Functions

| Function                         | Description                        |
| -------------------------------- | ---------------------------------- |
| `create_access_token(identity)`  | Create new access token            |
| `create_refresh_token(identity)` | Create refresh token               |
| `@jwt_required()`                | Protect endpoint                   |
| `@jwt_required(refresh=True)`    | Protect refresh endpoint           |
| `get_jwt_identity()`             | Get identity of logged-in user     |
| `get_jwt()`                      | Get full token data (claims, etc.) |