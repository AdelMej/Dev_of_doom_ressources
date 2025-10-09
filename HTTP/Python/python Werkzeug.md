# 🧠 **Werkzeug Cheat Sheet**

---

## 🚀 **Setup**

```bash
pip install werkzeug
```

```python
from werkzeug.wrappers import Request, Response
from werkzeug.serving import run_simple
```

---

## 🏗️ **Basic WSGI App**

```python
from werkzeug.wrappers import Request, Response
from werkzeug.serving import run_simple

@Request.application
def app(request):
    return Response(f"Hello, {request.remote_addr}!")

if __name__ == "__main__":
    run_simple("localhost", 5000, app)
```

---

## 📬 **Request Object**

### Import:

```python
from werkzeug.wrappers import Request
```

### Common Attributes:

|Attribute|Description|
|---|---|
|`request.method`|HTTP method (GET, POST, etc.)|
|`request.path`|URL path|
|`request.args`|Query parameters (`ImmutableMultiDict`)|
|`request.form`|Form fields|
|`request.files`|Uploaded files|
|`request.json`|JSON body (if content-type is `application/json`)|
|`request.headers`|Request headers|
|`request.cookies`|Cookies|
|`request.remote_addr`|Client IP|
|`request.url`|Full URL|

---

## 📤 **Response Object**

### Import:

```python
from werkzeug.wrappers import Response
```
### Example:

```python
response = Response("OK", status=200, mimetype="text/plain")
response.headers["X-Custom-Header"] = "Value"
response.set_cookie("user", "Alice")
```

|Attribute|Description|
|---|---|
|`response.status_code`|HTTP status|
|`response.data`|Response body|
|`response.headers`|Header dictionary|
|`response.mimetype`|MIME type|
|`response.set_cookie(key, value)`|Set cookie|
|`response.delete_cookie(key)`|Delete cookie|

---

## 🔀 **Routing (werkzeug.routing)**

```python
from werkzeug.routing import Map, Rule
from werkzeug.exceptions import NotFound

url_map = Map([
    Rule("/", endpoint="index"),
    Rule("/user/<name>", endpoint="user")
])

urls = url_map.bind("localhost")

try:
    endpoint, args = urls.match("/")
except NotFound:
    pass
```

---

## ⚙️ **Utilities**

### URL Encoding / Decoding

```python
from werkzeug.urls import url_encode, url_decode

params = {"a": 1, "b": 2}
encoded = url_encode(params)      # a=1&b=2
decoded = url_decode(encoded)     # ImmutableMultiDict([('a', '1'), ('b', '2')])
```

### Secure Passwords

```python
from werkzeug.security import generate_password_hash, check_password_hash

hash = generate_password_hash("secret")
check_password_hash(hash, "secret")  # True
```
### File Sending

```python
from werkzeug.utils import send_file
return send_file("example.txt", as_attachment=True)
```
---

## 🧱 **Error Handling**

```python
from werkzeug.exceptions import NotFound, BadRequest

@Request.application
def app(request):
    if request.path != "/":
        raise NotFound("This page does not exist")
    return Response("Hello!", 200)
```

---

## 🧰 **Testing Utilities**

```python
from werkzeug.test import Client
from werkzeug.wrappers import Response

client = Client(app, Response)

resp = client.get("/")
print(resp.status_code, resp.data)
```

---

## 🔒 **Session Example**

Werkzeug doesn’t include sessions by default, but you can use signed cookies:

```python
from werkzeug.middleware.shared_data import SharedDataMiddleware
from werkzeug.middleware.proxy_fix import ProxyFix
```

Use these middlewares to serve static files or handle proxy headers properly.

---

## 🧩 **Mini Example: JSON API**

```python
from werkzeug.wrappers import Request, Response
from werkzeug.serving import run_simple
import json

@Request.application
def app(request):
    if request.method == "POST":
        data = request.get_json()
        return Response(json.dumps({"received": data}), mimetype="application/json")
    return Response("Send a POST request with JSON!", mimetype="text/plain")

run_simple("localhost", 8080, app)
```

---

## ⚡ **Development Server**

 ```bash
 python app.py
 ```

Then visit [http://localhost:8080](http://localhost:8080)