## 🧩 **Flask Config Cheat Sheet**

### 🏗️ 1. Structure

```
project/
│
├── app/
│   ├── __init__.py
│   ├── routes/
│   ├── models/
│   └── config.py
│
├── .env
└── run.py
```

---

### ⚙️ 2. `config.py`

```python
import os
from dotenv import load_dotenv

# Load .env file
load_dotenv()

class Config:
    # --- Core Flask ---
    DEBUG = os.getenv("DEBUG", "False").lower() == "true"
    TESTING = os.getenv("TESTING", "False").lower() == "true"
    SECRET_KEY = os.getenv("SECRET_KEY", "dev-secret-key")

    # --- Flask Server ---
    HOST = os.getenv("HOST", "127.0.0.1")
    PORT = int(os.getenv("PORT", 8000))

    # --- JWT or Auth ---
    JWT_SECRET_KEY = os.getenv("JWT_SECRET_KEY", SECRET_KEY)
    JWT_ALGORITHM = "HS256"
    JWT_EXPIRATION_MINUTES = int(os.getenv("JWT_EXPIRATION_MINUTES", 30))

    # --- Logging ---
    LOG_FILE = os.getenv("LOG_FILE", "app.log")
    LOG_LEVEL = os.getenv("LOG_LEVEL", "INFO")

    # --- System integration (your case) ---
    ALLOWED_GROUPS = os.getenv("ALLOWED_GROUPS", "sudo,wheel").split(",")

class DevConfig(Config):
    DEBUG = True
    LOG_LEVEL = "DEBUG"

class ProdConfig(Config):
    DEBUG = False
    LOG_LEVEL = "WARNING"
```

---

### 📜 3. `.env` example

```python
SECRET_KEY=my-ultra-secret-key
JWT_SECRET_KEY=another-secret-key
DEBUG=True
LOG_FILE=/var/log/dashboard.log
ALLOWED_GROUPS=sudo,wheel
```

---

### 🚀 4. `__init__.py`

```python
from flask import Flask
from app.config import Config

def create_app():
    app = Flask(__name__)
    app.config.from_object(Config)
    return app
```

---

### 🧠 5. Environment-Specific Usage

In `run.py`:

```python
from app import create_app
from app.config import DevConfig, ProdConfig
import os

env = os.getenv("FLASK_ENV", "development")

if env == "production":
    app = create_app()
    app.config.from_object(ProdConfig)
else:
    app = create_app()
    app.config.from_object(DevConfig)

if __name__ == "__main__":
    app.run(host=app.config["HOST"], port=app.config["PORT"])
```
---

### 🧾 6. Common Config Vars You Might Add Later

|Purpose|Variable|Example|
|---|---|---|
|DB connection|`SQLALCHEMY_DATABASE_URI`|`sqlite:///app.db`|
|CORS|`CORS_ORIGINS`|`https://myfrontend.com`|
|Rate limit|`RATELIMIT_DEFAULT`|`100 per hour`|
|Logging|`LOG_LEVEL`|`DEBUG` / `INFO` / `ERROR`|
|Uploads|`UPLOAD_FOLDER`|`/var/data/uploads`|
|Session lifetime|`PERMANENT_SESSION_LIFETIME`|`3600` (seconds)|

---

### 🧠 TL;DR — Rules of Flask Config

1. ✅ Keep secrets out of code — always in `.env`
2. 🧱 Keep default safe fallbacks (`dev-secret-key`, etc.)
3. 🌍 Use separate classes for `Dev`, `Prod`, `Test`
4. 💾 Load config once in `__init__.py`
5. 🚫 Never push `.env` or credentials to Git