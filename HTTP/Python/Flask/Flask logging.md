# 🪵 Flask & Python Logging Cheat Sheet

## 📘 Overview

Logging is how you record what's happening inside your Flask app — from debugging info to critical errors.

Flask uses Python’s built-in `logging` module under the hood.

---

## 🧠 Basic Logging

```python
import logging

logging.basicConfig(level=logging.INFO)
logging.info("App started")
logging.warning("Something looks odd")
logging.error("An error occurred")
```

### Common Log Levels
| Level | Function | Description |
|--------|-----------|-------------|
| DEBUG | `logging.debug()` | Detailed info for debugging |
| INFO | `logging.info()` | General runtime events |
| WARNING | `logging.warning()` | Something unexpected happened |
| ERROR | `logging.error()` | A serious problem occurred |
| CRITICAL | `logging.critical()` | Program may not continue |

---

## 🧩 Flask Logging Setup

### Example: `config.py`

```python
import logging
from logging.handlers import RotatingFileHandler
import os

class Config:
    LOG_FILE = "logs/app.log"
    LOG_LEVEL = logging.INFO

    @staticmethod
    def init_app(app):
        # Create logs folder if missing
        os.makedirs(os.path.dirname(Config.LOG_FILE), exist_ok=True)

        # Create a rotating file handler
        file_handler = RotatingFileHandler(
            Config.LOG_FILE, maxBytes=10240, backupCount=5
        )
        file_handler.setLevel(Config.LOG_LEVEL)
        formatter = logging.Formatter(
            "[%(asctime)s] %(levelname)s in %(module)s: %(message)s"
        )
        file_handler.setFormatter(formatter)

        # Attach to Flask app logger
        app.logger.addHandler(file_handler)
        app.logger.setLevel(Config.LOG_LEVEL)

        # Optional: silence HTTP request spam
        logging.getLogger("werkzeug").setLevel(logging.WARNING)
```

---

### Example: `app.py`

```python
from flask import Flask
from config import Config

app = Flask(__name__)
app.config.from_object(Config)

# Initialize logger
Config.init_app(app)

@app.route("/")
def index():
    app.logger.info("Index route accessed!")
    return "Hello Flask Logging!"
```

---

## 🧱 Multi-Environment Logging

### Example: `config.py`

```python
class BaseConfig:
    LOG_LEVEL = logging.INFO
    LOG_FILE = "logs/app.log"

    @staticmethod
    def init_app(app):
        pass


class DevelopmentConfig(BaseConfig):
    LOG_LEVEL = logging.DEBUG


class ProductionConfig(BaseConfig):
    LOG_LEVEL = logging.WARNING
```

### In `app.py`
```python
import os

env = os.getenv("FLASK_ENV", "development")
if env == "production":
    app.config.from_object("config.ProductionConfig")
else:
    app.config.from_object("config.DevelopmentConfig")

app.config["Config"].init_app(app)
```

---

## 🧩 Advanced Handlers

### Stream Handler (Console Only)
```python
console_handler = logging.StreamHandler()
console_handler.setFormatter(logging.Formatter("%(levelname)s: %(message)s"))
app.logger.addHandler(console_handler)
```

### File Handler (Persistent Log File)
```python
file_handler = logging.FileHandler("logs/app.log")
file_handler.setFormatter(logging.Formatter("%(asctime)s - %(levelname)s - %(message)s"))
app.logger.addHandler(file_handler)
```

### Rotating File Handler
```python
from logging.handlers import RotatingFileHandler

rot_handler = RotatingFileHandler("logs/app.log", maxBytes=10240, backupCount=5)
rot_handler.setFormatter(logging.Formatter("%(asctime)s - %(message)s"))
app.logger.addHandler(rot_handler)
```

---

## 🧩 Custom Logger

```python
logger = logging.getLogger("custom_logger")
logger.setLevel(logging.DEBUG)

handler = logging.FileHandler("logs/custom.log")
formatter = logging.Formatter("%(asctime)s [%(levelname)s] %(name)s: %(message)s")
handler.setFormatter(formatter)

logger.addHandler(handler)
logger.info("Custom logger ready!")
```

---

## 🧩 SQLAlchemy Query Logging (Flask + DB)

If you’re using SQLAlchemy with Flask, you can log **all executed SQL queries** easily:

### Example

```python
import logging

logging.basicConfig()
logging.getLogger("sqlalchemy.engine").setLevel(logging.INFO)
```

This prints all SQL queries to the console.

To log to a file instead:

```python
sql_logger = logging.getLogger("sqlalchemy.engine")
file_handler = logging.FileHandler("logs/sqlalchemy.log")
formatter = logging.Formatter("%(asctime)s - %(message)s")
file_handler.setFormatter(formatter)
sql_logger.addHandler(file_handler)
sql_logger.setLevel(logging.INFO)
```

You can also filter specific types of SQL logs:

```python
logging.getLogger("sqlalchemy.pool").setLevel(logging.WARNING)   # Connection pool info
logging.getLogger("sqlalchemy.dialects").setLevel(logging.ERROR) # Dialect warnings
```

✅ **Best practice:** Enable SQL logging only in development environments — it slows down production systems.

---

## 🧰 Formatting Reference

| Format Code | Description |
|--------------|-------------|
| `%(asctime)s` | Timestamp |
| `%(levelname)s` | Log level name |
| `%(message)s` | Log message |
| `%(module)s` | Module name |
| `%(name)s` | Logger name |
| `%(filename)s` | Filename |
| `%(lineno)d` | Line number |
| `%(threadName)s` | Thread name |

---

## 🚨 Logging Best Practices

1. Use `app.logger` for Flask apps.
2. Use different log levels meaningfully.
3. Use **RotatingFileHandler** in production.
4. Store logs outside project root.
5. Enable SQLAlchemy query logging only when debugging.
6. Keep formats consistent.

---

**Author:** ChatGPT  
**Version:** v1.1  
**Last Updated:** 2025-10-30
