# 🧠 Flask Cheat Sheet
## 🚀 Setup
```python
python -m venv venv
source venv/bin/activate
pip install flask
```

Run the app:

```python
python app.py
```

or with auto-reload:

```python
FLASK_APP=app.py FLASK_ENV=development flask run
```

---
## 🏗️ Basic Structure
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, Flask!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8000)
```

---
## 🌐 Routing
```python
@app.route('/hello/<name>')
def hello(name):
    return f"Hello, {name}!"

@app.route('/user/<int:user_id>')
def user(user_id):
    return f"User ID: {user_id}"

@app.route('/path/<path:subpath>')
def show_path(subpath):
    return f"Path: {subpath}"
```

HTTP methods:
```python
@app.route('/submit', methods=['GET', 'POST'])
def submit():
    if request.method == 'POST':
        return "You sent a POST!"
    return "Send me a POST!"
```

---
## 📦 JSON Responses
```python
from flask import jsonify

@app.route('/data')
def data():
    return jsonify(name="John", age=30, city="New York")
```

Equivalent to setting headers manually:

```python
return {"name": "John", "age": 30}, 200
```

---
## 📥 Handling POST & Request Data
```python
from flask import request

@app.route('/login', methods=['POST'])
def login():
    data = request.json      # if Content-Type: application/json
    username = data.get("username")
    return jsonify(message=f"Welcome {username}")
```

---
## 🧩 Query Parameters
```python
@app.route('/search')
def search():
    term = request.args.get('q', '')
    return jsonify(result=f"Searching for {term}")

```

Example:
```sql
GET /search?q=flask
```

---
## 🧱 Templates (HTML Rendering)

Folder structure:
```
app.py
templates/
 └── index.html
```
```python
from flask import render_template

@app.route('/')
def home():
    return render_template('index.html', title='Welcome!')
```

index.html
```html
<!doctype html>
<html>
  <head><title>{{ title }}</title></head>
  <body><h1>Hello from Flask!</h1></body>
</html>
```

---
## 🗃️ Static Files

Folder structure:
```pgsql
static/
 └── style.css
```

Access via:
```
/static/style.css
```

---
## ⚠️ Error Handling
```python
@app.errorhandler(404)
def not_found(e):
    return jsonify(error="Not Found"), 404

@app.errorhandler(500)
def server_error(e):
    return jsonify(error="Internal Server Error"), 500
```

---
## 🧰 Redirects & URLs
```python
from flask import redirect, url_for

@app.route('/old')
def old():
    return redirect(url_for('new'))

@app.route('/new')
def new():
    return "This is the new endpoint!"
```

---
## 🧑‍💻 Debugging Tips

- app.debug = True enables auto-reload & detailed tracebacks.
- Flask prints each route on startup — perfect for quick verification.

---
## 🧱 Minimal REST API Example
 ```python
from flask import Flask, request, jsonify

app = Flask(__name__)

users = []

@app.route('/users', methods=['GET', 'POST'])
def manage_users():
    if request.method == 'POST':
        user = request.json
        users.append(user)
        return jsonify(user), 201
    return jsonify(users)
```

---
## 🧩 Useful Imports Recap
```python
from flask import Flask, request, jsonify, render_template, redirect, url_for
```