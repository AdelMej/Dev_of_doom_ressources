
# 🧠 Flask + SQLAlchemy Complete Cheat Sheet

---

## ⚙️ Project Setup (Best Practice)

**Folder structure:**
```
project/
├── app/
│   ├── __init__.py
│   ├── extensions.py
│   ├── models/
│   │   ├── __init__.py
│   │   └── user.py
│   ├── routes/
│   │   ├── __init__.py
│   │   └── user_routes.py
│   └── config.py
├── migrations/
├── run.py
└── requirements.txt
```

---

## 🧩 app/extensions.py
```python
from flask_sqlalchemy import SQLAlchemy
from flask_migrate import Migrate

db = SQLAlchemy()
migrate = Migrate()
```

---

## ⚙️ app/config.py
```python
import os

class Config:
    SQLALCHEMY_DATABASE_URI = os.getenv("DATABASE_URL", "sqlite:///app.db")
    SQLALCHEMY_TRACK_MODIFICATIONS = False
    SECRET_KEY = os.getenv("SECRET_KEY", "super-secret-key")
```

---

## 🚀 app/__init__.py
```python
from flask import Flask
from app.extensions import db, migrate
from app.config import Config

def create_app():
    app = Flask(__name__)
    app.config.from_object(Config)

    db.init_app(app)
    migrate.init_app(app, db)

    from app.routes.user_routes import user_bp
    app.register_blueprint(user_bp, url_prefix="/api/users")

    return app
```

---

## 🧱 app/models/user.py
```python
from app.extensions import db
from datetime import datetime

class User(db.Model):
    __tablename__ = "users"

    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), unique=True, nullable=False)
    email = db.Column(db.String(120), unique=True, nullable=False)
    password = db.Column(db.String(255), nullable=False)
    is_admin = db.Column(db.Boolean, default=False)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)

    def __repr__(self):
        return f"<User {self.username}>"

    def to_dict(self):
        return {
            "id": self.id,
            "username": self.username,
            "email": self.email,
            "is_admin": self.is_admin,
            "created_at": self.created_at.isoformat(),
        }
```

---

## 🧭 app/routes/user_routes.py
```python
from flask import Blueprint, jsonify, request
from app.extensions import db
from app.models.user import User

user_bp = Blueprint("user_bp", __name__)

@user_bp.route("/", methods=["GET"])
def get_users():
    users = User.query.all()
    return jsonify([u.to_dict() for u in users])

@user_bp.route("/", methods=["POST"])
def create_user():
    data = request.get_json()
    user = User(
        username=data["username"],
        email=data["email"],
        password=data["password"],
    )
    db.session.add(user)
    db.session.commit()
    return jsonify(user.to_dict()), 201

@user_bp.route("/<int:user_id>", methods=["PUT"])
def update_user(user_id):
    user = User.query.get_or_404(user_id)
    data = request.get_json()
    user.username = data.get("username", user.username)
    db.session.commit()
    return jsonify(user.to_dict())

@user_bp.route("/<int:user_id>", methods=["DELETE"])
def delete_user(user_id):
    user = User.query.get_or_404(user_id)
    db.session.delete(user)
    db.session.commit()
    return jsonify({"message": "User deleted"})
```

---

## 🧠 Common SQLAlchemy Operations

### ➕ Add / Commit
```python
user = User(username="bob", email="bob@mail.com", password="pass")
db.session.add(user)
db.session.commit()
```

### 🔍 Queries
```python
User.query.all()
User.query.first()
User.query.get(1)
User.query.filter_by(username="bob").first()
User.query.filter(User.email.like("%mail.com")).all()
```

### ✏️ Update
```python
user = User.query.get(1)
user.username = "robert"
db.session.commit()
```

### ❌ Delete
```python
user = User.query.get(1)
db.session.delete(user)
db.session.commit()
```

---

## 🔗 Relationships

### One-to-Many
```python
class Post(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100))
    user_id = db.Column(db.Integer, db.ForeignKey("users.id"))
    user = db.relationship("User", backref="posts", lazy=True)
```

### Many-to-Many
```python
user_group = db.Table(
    "user_group",
    db.Column("user_id", db.Integer, db.ForeignKey("users.id")),
    db.Column("group_id", db.Integer, db.ForeignKey("groups.id"))
)

class Group(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(50))
    users = db.relationship("User", secondary=user_group, backref="groups")
```

---

## 🧪 Context Tips
```python
from app import create_app
from app.extensions import db
app = create_app()

with app.app_context():
    db.create_all()
```

---

## ⚡ Debugging & Utilities
| Action | Command |
|--------|----------|
| Drop all tables | `db.drop_all()` |
| Recreate all tables | `db.create_all()` |
| Count rows | `User.query.count()` |
| Order by | `User.query.order_by(User.username.desc()).all()` |
| Pagination | `User.query.paginate(page=1, per_page=10, error_out=False)` |

---

## 🧰 Flask-Migrate (Alembic)
```bash
flask db init
flask db migrate -m "Initial migration"
flask db upgrade
```

---

## 🔐 Bonus: Bcrypt Integration
**app/extensions.py**
```python
from flask_bcrypt import Bcrypt
bcrypt = Bcrypt()
```

**In User model:**
```python
from app.extensions import bcrypt

def set_password(self, password):
    self.password = bcrypt.generate_password_hash(password).decode('utf-8')

def check_password(self, password):
    return bcrypt.check_password_hash(self.password, password)
```
