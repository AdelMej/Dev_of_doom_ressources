# 🐍 Python `http.server` Module Cheat Sheet

### 📦 Import Basics

```python
from http.server import HTTPServer, SimpleHTTPRequestHandler, CGIHTTPRequestHandler
```

---

## ⚙️ 1. Starting a Basic Server

### 🔹 Serve files from current directory

```python
from http.server import SimpleHTTPRequestHandler, HTTPServer

server = HTTPServer(('0.0.0.0', 8000), SimpleHTTPRequestHandler)
print("Serving on port 8000...")
server.serve_forever()
```

### 🔹 Serve files from a specific directory

```python
from http.server import SimpleHTTPRequestHandler, HTTPServer

class CustomHandler(SimpleHTTPRequestHandler):
    def __init__(self, *args, **kwargs):
        super().__init__(*args, directory="/path/to/files", **kwargs)

HTTPServer(('0.0.0.0', 8080), CustomHandler).serve_forever()
```

---

## 💬 2. Handling Requests (Custom Logic)

### 🔹 Override `do_GET()`

```python
from http.server import BaseHTTPRequestHandler, HTTPServer

class MyHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header("Content-type", "text/html")
        self.end_headers()
        self.wfile.write(b"<h1>Hello from custom GET!</h1>")

server = HTTPServer(("0.0.0.0", 8000), MyHandler)
server.serve_forever()
```

### 🔹 Handle `POST` requests

```python
class MyHandler(BaseHTTPRequestHandler):
    def do_POST(self):
        length = int(self.headers.get('Content-Length', 0))
        data = self.rfile.read(length).decode('utf-8')
        print("Received POST data:", data)
        self.send_response(200)
        self.end_headers()
        self.wfile.write(b"POST received!")
```

---

## 🧱 3. Common Methods (BaseHTTPRequestHandler)

|Method|Purpose|
|---|---|
|`do_GET(self)`|Handle HTTP GET requests|
|`do_POST(self)`|Handle HTTP POST requests|
|`do_HEAD(self)`|Handle HEAD requests|
|`send_response(code)`|Send status code (e.g. `200`, `404`)|
|`send_header(key, value)`|Add HTTP header|
|`end_headers()`|Finish header section|
|`wfile.write(data)`|Send response body (bytes only)|
|`rfile.read(size)`|Read raw request body|
|`log_message(fmt, *args)`|Log requests (override to customize output)|

---

## 🧰 4. Built-in Handler Classes

|Class|Description|
|---|---|
|`BaseHTTPRequestHandler`|Foundation for all handlers (must override `do_*`)|
|`SimpleHTTPRequestHandler`|Serves files and directory listings|
|`CGIHTTPRequestHandler`|Runs `.cgi` or `.py` scripts in `cgi-bin/` directories|

---

## ⚙️ 5. CGI Server Example

Run Python scripts as web pages:

```python
from http.server import CGIHTTPRequestHandler, HTTPServer

server = HTTPServer(("0.0.0.0", 8000), CGIHTTPRequestHandler)
print("CGI Server running on port 8000")
server.serve_forever()
```

Your scripts must be in a `cgi-bin/` directory and executable (`chmod +x script.py`).

Example `cgi-bin/hello.py`:

```python
#!/usr/bin/env python3
print("Content-Type: text/html\n")
print("<h1>Hello from CGI!</h1>")
```

---

## 🪵 6. Logging and Error Handling

### 🔹 Customize logging

```python
class SilentHandler(SimpleHTTPRequestHandler):
    def log_message(self, format, *args):
        pass  # Disable console logs
```

### 🔹 Handle errors manually

```python
class MyHandler(SimpleHTTPRequestHandler):
    def send_error(self, code, message=None):
        self.send_response(code)
        self.end_headers()
        self.wfile.write(f"Error {code}: {message}".encode())
```

---

## 🧠 7. Useful Attributes

|Attribute|Description|
|---|---|
|`self.path`|Request path (e.g. `/index.html`)|
|`self.headers`|HTTP headers dictionary|
|`self.client_address`|`(IP, port)` of client|
|`self.server`|Reference to the `HTTPServer` object|
|`self.wfile`|Output stream (binary write)|
|`self.rfile`|Input stream (binary read)|

---

## 🧵 8. Multithreading (for multiple clients)

```python
from http.server import ThreadingHTTPServer, SimpleHTTPRequestHandler

server = ThreadingHTTPServer(('0.0.0.0', 8000), SimpleHTTPRequestHandler)
print("Threaded server running on port 8000")
server.serve_forever()
```

---

## 🧹 9. Stopping the Server Gracefully

```python
try:
    server.serve_forever()
except KeyboardInterrupt:
    pass
finally:
    server.server_close()
    print("Server stopped.")
```

---

## 🧩 10. Quick Reference Table

| Feature              | Class/Function                    |
| -------------------- | --------------------------------- |
| Basic static server  | `SimpleHTTPRequestHandler`        |
| CGI script execution | `CGIHTTPRequestHandler`           |
| Custom logic         | Subclass `BaseHTTPRequestHandler` |
| Threaded server      | `ThreadingHTTPServer`             |
| Run forever          | `serve_forever()`                 |
| Stop server          | `server_close()`                  |