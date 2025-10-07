# 🐍 Python http.server Cheat Sheet
## 🧩 Basic Usage

Serve files from the current directory:
```python
python3 -m http.server
```

Default:
- Port: 8000
- Directory: current working directory

---
## ⚙️ Custom Port
```python
python3 -m http.server 8080
```

➡️ Serves on http://localhost:8080

---
## 📁 Serve a Specific Directory
```python
python3 -m http.server 8080 --directory /path/to/dir
```

➡️ Example:
```python
python3 -m http.server 9090 --directory ~/projects/site
```
🔒 Bind to a Specific Address
```python
python3 -m http.server 8080 --bind 127.0.0.1
```

➡️ Only accessible from localhost.
To allow LAN access:
```python
python3 -m http.server 8080 --bind 0.0.0.0
```

---
## 🧠 Module Import (Custom Server)

You can import it in a script for more control:
```python
from http.server import SimpleHTTPRequestHandler, HTTPServer

PORT = 8000
server = HTTPServer(("0.0.0.0", PORT), SimpleHTTPRequestHandler)
print(f"Serving on port {PORT}")
server.serve_forever()
```

---
## 🔧 Custom Handler Example

To log or modify behavior:
```python
from http.server import SimpleHTTPRequestHandler, HTTPServer

class MyHandler(SimpleHTTPRequestHandler):
    def do_GET(self):
        print("Client requested:", self.path)
        super().do_GET()

server = HTTPServer(("0.0.0.0", 8000), MyHandler)
server.serve_forever()
```

---
## 📜 Directory Listing & MIME Types

- Automatically generates an HTML directory index if no index.html exists.
- Uses built-in MIME type guessing based on file extension.

---
## 🔍 Useful Flags
|Flag|Description|
|---|---|
|`--directory DIR`|Serve files from a specific directory|
|`--bind ADDR`|Bind server to a specific address|
|`--cgi`|Enable CGI scripts in `cgi-bin/` or any `htbin/` dir|
Example (CGI enabled):
```python
python3 -m http.server 8000 --cgi
```

---
## 🧪 Testing Requests

You can test your running server with:
```python
curl http://localhost:8000
```

Or check headers:
```python
curl -I http://localhost:8000
```

🧹 Stop Server
```python
Press Ctrl + C to stop it.
```

---
## 🚀 Quick Tips

- Perfect for static file serving and testing frontends.
- Not for production — use something like nginx, gunicorn, or uvicorn for real apps.
- Works out of the box in Python 3.x (no install required).