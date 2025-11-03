# 🧩 Flask-Bcrypt Complete Cheat Sheet

## ⚙️ Installation

```bash
pip install Flask Flask-Bcrypt Flask-SQLAlchemy
```
Optional for full project setup:

```bash
pip install Flask-WTF email-validator
```

---

## 🧱 Basic Setup

### Minimal app

```python
from flask import Flask
from flask_bcrypt import Bcrypt

app = Flask(__name__)
bcrypt = Bcrypt(app)
```

### With an app factory

```python
bcrypt = Bcrypt()

def create_app():
    app = Flask(__name__)
    bcrypt.init_app(app)
    return app
```

---

## 🔐 Core API

### 1️⃣ Generate Hash

```python
password = "mysecret"
hashed = bcrypt.generate_password_hash(password)  # returns bytes
hashed_str = hashed.decode("utf-8")               # store as string in DB
```

### 2️⃣ Verify Password

```python
bcrypt.check_password_hash(hashed, "mysecret")    # → True
bcrypt.check_password_hash(hashed, "wrongpass")   # → False
```

💡 _Never compare strings manually. Always use `check_password_hash()` — it’s timing-attack safe._

---

## 🧩 Integration with SQLAlchemy

### User Model

```python
from flask_sqlalchemy import SQLAlchemy
db = SQLAlchemy()

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    email = db.Column(db.String(120), unique=True, nullable=False)
    password_hash = db.Column(db.String(128), nullable=False)

    def set_password(self, password):
        self.password_hash = bcrypt.generate_password_hash(password).decode("utf-8")

    def check_password(self, password):
        return bcrypt.check_password_hash(self.password_hash, password)
```

---

## 🚪 Registration Example

```python
from flask import request, jsonify

@app.route('/register', methods=['POST'])
def register():
    data = request.json
    if User.query.filter_by(email=data['email']).first():
        return jsonify(error="Email already exists"), 400
    user = User(email=data['email'])
    user.set_password(data['password'])
    db.session.add(user)
    db.session.commit()
    return jsonify(message="User registered"), 201
```

---

## 🔑 Login Example

```python
@app.route('/login', methods=['POST'])
def login():
    data = request.json
    user = User.query.filter_by(email=data['email']).first()
    if user and user.check_password(data['password']):
        return jsonify(message="Login successful"), 200
    return jsonify(error="Invalid credentials"), 401
```

---

## ⚙️ Configuration

### Adjusting Hash Strength

```python
app.config['BCRYPT_LOG_ROUNDS'] = 12  # default ~12; increase for stronger but slower hashes
bcrypt = Bcrypt(app)
```

|Value|Description|
|---|---|
|`4–10`|Too weak for production|
|`11–14`|Recommended range|
|`>15`|Very strong, but slower (may affect API response time)|

🧮 _Each +1 roughly doubles the computation time._

---

## 🔁 Rehashing on Upgrade

If you raise the work factor (rounds), old hashes remain valid but weaker.  
Rehash them automatically after successful login:

```python
stored_cost = int(user.password_hash.split('$')[2])
current_cost = app.config['BCRYPT_LOG_ROUNDS']
if stored_cost < current_cost:
    user.set_password(password)
    db.session.commit()
```

---

## 🧱 Database Tips

- Use `String(128)` column for bcrypt hashes (60–72 chars typically).
- Store the full hash, not parts.
- Never store raw passwords, even temporarily.

---

## 🧠 Advanced Patterns

### Pre-hashing (optional)

If you accept **extremely long passwords**, hash them first using SHA-256:

```python
import hashlib
raw_pw = hashlib.sha256(password.encode()).hexdigest()
hashed_pw = bcrypt.generate_password_hash(raw_pw)
```
(_Useful for systems enforcing 72-char bcrypt limit._)

---

### With Flask-WTF (Form Example)

```python
from flask_wtf import FlaskForm
from wtforms import StringField, PasswordField, SubmitField
from wtforms.validators import DataRequired, Email, EqualTo

class RegisterForm(FlaskForm):
    email = StringField('Email', validators=[DataRequired(), Email()])
    password = PasswordField('Password', validators=[DataRequired()])
    confirm = PasswordField('Confirm', validators=[EqualTo('password')])
    submit = SubmitField('Register')
```

Then:

```python
@app.route('/register', methods=['GET', 'POST'])
def register():
    form = RegisterForm()
    if form.validate_on_submit():
        user = User(email=form.email.data)
        user.set_password(form.password.data)
        db.session.add(user)
        db.session.commit()
        return redirect('/login')
    return render_template('register.html', form=form)
```

---

## 🧰 Utilities

### Verify Password Strength (custom)

```python
import re

def strong_password(pw):
    if len(pw) < 8: return False
    if not re.search(r"[A-Z]", pw): return False
    if not re.search(r"[a-z]", pw): return False
    if not re.search(r"\d", pw): return False
    if not re.search(r"[^A-Za-z0-9]", pw): return False
    return True
```

---

## 🧠 Common Mistakes & Fixes

|Mistake|Fix|
|---|---|
|Forgot `.decode()`|Decode before storing (`.decode('utf-8')`)|
|Comparing manually|Always use `check_password_hash()`|
|Using plaintext in DB|Never store passwords directly|
|Too few rounds|Minimum 12 for production|
|Using bcrypt for API tokens|Use JWT or `itsdangerous` instead|
|No HTTPS|Always encrypt traffic in production|

---

## 🔒 Security Best Practices

✅ Use HTTPS (Flask-Talisman or reverse proxy)  
✅ Rate-limit login attempts (Flask-Limiter)  
✅ Add MFA (time-based OTP) for admin accounts  
✅ Rehash passwords when increasing rounds  
✅ Log failed login attempts  
✅ Rotate user passwords periodically  
✅ Use session protection (`SESSION_PROTECTION = "strong"`)

---

## 🧩 Integration Example (JWT + Bcrypt)

```python
from flask_jwt_extended import JWTManager, create_access_token

app.config['JWT_SECRET_KEY'] = 'supersecret'
jwt = JWTManager(app)

@app.route('/login', methods=['POST'])
def login():
    data = request.json
    user = User.query.filter_by(email=data['email']).first()
    if user and user.check_password(data['password']):
        token = create_access_token(identity=user.id)
        return jsonify(access_token=token)
    return jsonify(error="Invalid credentials"), 401
```

---

## 🧪 Testing Password Hashes (Unit Tests)

```python
def test_password_hashing():
    pw = "Test123!"
    hash_ = bcrypt.generate_password_hash(pw)
    assert bcrypt.check_password_hash(hash_, pw)
    assert not bcrypt.check_password_hash(hash_, "wrong")
```

---

## 🧾 Quick Reference Table

|Task|Code Snippet|
|---|---|
|Create hash|`bcrypt.generate_password_hash(pw).decode()`|
|Verify|`bcrypt.check_password_hash(hash, pw)`|
|Change cost|`app.config['BCRYPT_LOG_ROUNDS'] = N`|
|Use factory|`bcrypt.init_app(app)`|
|Get current rounds|`int(hash.split('$')[2])`|
|Rehash if cost < new|`user.set_password(pw)`|
|Hash limit|72 chars max input before truncation|

---

## ✅ Full Minimal Example

```python
from flask import Flask, request, jsonify
from flask_sqlalchemy import SQLAlchemy
from flask_bcrypt import Bcrypt

app = Flask(__name__)
app.config.update(
    SQLALCHEMY_DATABASE_URI='sqlite:///users.db',
    SQLALCHEMY_TRACK_MODIFICATIONS=False,
    BCRYPT_LOG_ROUNDS=12
)
db = SQLAlchemy(app)
bcrypt = Bcrypt(app)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    email = db.Column(db.String(120), unique=True, nullable=False)
    password_hash = db.Column(db.String(128), nullable=False)
    def set_password(self, pw):
        self.password_hash = bcrypt.generate_password_hash(pw).decode()
    def check_password(self, pw):
        return bcrypt.check_password_hash(self.password_hash, pw)

@app.route('/register', methods=['POST'])
def register():
    data = request.json
    user = User(email=data['email'])
    user.set_password(data['password'])
    db.session.add(user)
    db.session.commit()
    return jsonify(message="User created"), 201

@app.route('/login', methods=['POST'])
def login():
    data = request.json
    user = User.query.filter_by(email=data['email']).first()
    if user and user.check_password(data['password']):
        return jsonify(message="Login success")
    return jsonify(error="Invalid credentials"), 401

if __name__ == '__main__':
    db.create_all()
    app.run(debug=True)
```