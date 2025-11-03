# 🧠 Flask-Login Cheat Sheet

Flask-Login is a Flask extension that simplifies user session management.

---

## 🚀 Installation

```bash
pip install flask flask-login
```

---

## 🧩 Basic Setup

```python
from flask import Flask
from flask_login import LoginManager, UserMixin, login_user, login_required, logout_user, current_user

app = Flask(__name__)
app.secret_key = "supersecretkey"  # Required for session management

login_manager = LoginManager()
login_manager.init_app(app)
login_manager.login_view = "login"  # Redirect here if @login_required fails
```

---

## 👤 User Model

You can use your own user class (e.g., from a database), but it must implement certain properties/methods.

Simplest form:

```python
class User(UserMixin):
    def __init__(self, id, username, password):
        self.id = id
        self.username = username
        self.password = password
```

**`UserMixin`** provides default implementations for:
- `is_authenticated`
- `is_active`
- `is_anonymous`
- `get_id()`

---

## 🧭 User Loader

Flask-Login needs a way to load users from IDs stored in sessions:

```python
users = {
    "1": User("1", "admin", "password123"),
    "2": User("2", "bob", "hunter2"),
}

@login_manager.user_loader
def load_user(user_id):
    return users.get(user_id)
```

---

## 🔑 Login Route

```python
from flask import request, redirect, url_for

@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form["username"]
        password = request.form["password"]

        for user in users.values():
            if user.username == username and user.password == password:
                login_user(user, remember=True)  # creates session
                return redirect(url_for("protected"))
        return "Invalid credentials!", 401
    return '''
        <form method="post">
            <input name="username" placeholder="Username">
            <input name="password" placeholder="Password" type="password">
            <input type="submit" value="Login">
        </form>
    '''
```

---

## 🔒 Protecting Routes

Use `@login_required` to restrict access:

```python
@app.route("/protected")
@login_required
def protected():
    return f"Hello, {current_user.username}! You are logged in."
```

If a user is not logged in, Flask-Login redirects them to `login_view`.

---

## 🚪 Logout Route

```python
@app.route("/logout")
@login_required
def logout():
    logout_user()
    return "Logged out!"
```

---

## 👁️ Checking Login State

```python
if current_user.is_authenticated:
    print(current_user.id)
else:
    print("Not logged in.")
```

---

## 🍪 “Remember Me” Sessions

`login_user(user, remember=True)` keeps users logged in using a cookie.

To configure its duration:

```python
from datetime import timedelta

login_manager.remember_cookie_duration = timedelta(days=7)
```

---

## ⚠️ Common Gotchas

| Problem | Fix |
|----------|-----|
| `RuntimeError: The session is unavailable` | Set `app.secret_key` |
| Redirect loop on login | Ensure `@login_required` doesn’t wrap `/login` |
| User not persisting | Check `user_loader` and that `get_id()` returns a string |
| Not redirected to login | Set `login_manager.login_view = "login"` |

---

## 🧰 Useful Functions

| Function | Description |
|-----------|-------------|
| `login_user(user, remember=False)` | Logs in a user |
| `logout_user()` | Logs out the user |
| `current_user` | Proxy to the current user |
| `login_required` | Decorator to protect routes |
| `fresh_login_required` | Stricter decorator for sensitive actions |
| `login_manager.user_loader` | Function that loads users from ID |

---

## 🧠 Example: Full Minimal App

```python
from flask import Flask, redirect, url_for, request
from flask_login import LoginManager, UserMixin, login_user, logout_user, login_required, current_user

app = Flask(__name__)
app.secret_key = "supersecret"

login_manager = LoginManager(app)
login_manager.login_view = "login"

class User(UserMixin):
    def __init__(self, id, username, password):
        self.id = id
        self.username = username
        self.password = password

users = {"1": User("1", "admin", "password")}

@login_manager.user_loader
def load_user(user_id):
    return users.get(user_id)

@app.route("/login", methods=["GET", "POST"])
def login():
    if request.method == "POST":
        username = request.form["username"]
        password = request.form["password"]
        for u in users.values():
            if u.username == username and u.password == password:
                login_user(u)
                return redirect(url_for("protected"))
        return "Invalid login!"
    return '<form method="post"><input name="username"><input name="password" type="password"><input type="submit"></form>'

@app.route("/protected")
@login_required
def protected():
    return f"Welcome {current_user.username}!"

@app.route("/logout")
@login_required
def logout():
    logout_user()
    return "Logged out."

if __name__ == "__main__":
    app.run(debug=True)
```

---

## 🧩 Optional: Integrating with Flask-SQLAlchemy

You can replace the dictionary-based `users` with a real database model.

```python
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy(app)

class User(UserMixin, db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True)
    password = db.Column(db.String(200))
```

Then modify `load_user`:

```python
@login_manager.user_loader
def load_user(user_id):
    return User.query.get(int(user_id))
```

---

## 🔐 Pro Tips

- Always **hash passwords** (e.g., with `werkzeug.security` or `bcrypt`).
- Use `@fresh_login_required` for sensitive actions like changing passwords.
- For APIs, consider using **Flask-JWT-Extended** instead.
- Combine with **Flask-WTF** for secure login forms and CSRF protection.

---

✅ **Summary:**
Flask-Login saves time, reduces boilerplate, and makes authentication clean, secure, and scalable for any Flask project.
