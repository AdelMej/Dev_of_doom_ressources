# 🧾 Flask-HTTPAuth Cheat Sheet

## 📦 Install first:
```bash
pip install Flask-HTTPAuth
```


Then:
```python
from flask_httpauth import HTTPBasicAuth, HTTPTokenAuth
```

---
## 🧩 1. Basic Auth (Username + Password)
### 🔹 Setup
```python
from flask import Flask, jsonify
from flask_httpauth import HTTPBasicAuth

app = Flask(__name__)
auth = HTTPBasicAuth()

# Fake database
users = {"admin": "secret", "john": "doe123"}

@auth.verify_password
def verify_password(username, password):
    if username in users and users[username] == password:
        return username  # returning any truthy value == success
    return None
```

### 🔹 Protect a route
```python
@app.route("/secure")
@auth.login_required
def secure():
    return jsonify(message=f"Hello, {auth.current_user()}!")
```

## 🔹 Test it
```bash
curl -u admin:secret http://localhost:5000/secure
```

→ {"message": "Hello, admin!"}

If wrong creds:
→ 401 Unauthorized

---
## 🧩 2. Custom Unauthorized Message
```python
@auth.error_handler
def unauthorized():
    return jsonify(error="Unauthorized access"), 401
```

---
## 🧩 3. Token-Based Auth (Bearer Tokens)
### 🔹 Setup
```python
from flask_httpauth import HTTPTokenAuth

token_auth = HTTPTokenAuth(scheme="Bearer")

# Example token store
tokens = {
    "secrettoken123": "admin",
    "apitoken456": "user"
}

@token_auth.verify_token
def verify_token(token):
    if token in tokens:
        return tokens[token]
    return None
```

### 🔹 Protect route
```python
@app.route("/api/data")
@token_auth.login_required
def get_data():
    return jsonify(user=token_auth.current_user(), data="Sensitive info")
```

## 🔹 Test it
```bash
curl -H "Authorization: Bearer secrettoken123" http://localhost:5000/api/data
```

→ {"user": "admin", "data": "Sensitive info"}

---
## 🧩 4. Combining Basic + Token Auth (Advanced)
```python
@auth.verify_password
def verify_pw(username, password):
    if username in users and users[username] == password:
        return username
    return None

@token_auth.verify_token
def verify_tok(token):
    if token in tokens:
        return tokens[token]
    return None

@app.route("/auth-test")
@auth.login_required
def basic():
    return jsonify(message="Basic OK")

@app.route("/token-test")
@token_auth.login_required
def token():
    return jsonify(message="Token OK")
```

---
## 🧩 5. Use with Blueprints

You can use `@auth.login_required` inside any Blueprint — just import the same `auth` or `token_auth` instance.

---
## 🧩 6. Custom Status Codes or Headers
```python
@auth.error_handler
def custom_error():
    return jsonify(error="Auth failed"), 403
```

---
## ⚡ Quick Reference
| Feature    | Decorator                    | Verification Function      | Header Example                                   |
| ---------- | ---------------------------- | -------------------------- | ------------------------------------------------ |
| Basic Auth | `@auth.login_required`       | `@auth.verify_password`    | `Authorization: Basic base64(username:password)` |
| Token Auth | `@token_auth.login_required` | `@token_auth.verify_token` | `Authorization: Bearer <token>`                  |
|            |                              |                            |                                                  |
