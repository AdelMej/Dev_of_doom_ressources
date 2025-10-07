## ⚙️ **Python `requests` Cheat Sheet**

### 🧠 **Import**

```python
import requests
```

---

### 🌐 **Basic GET Request**

```python
response = requests.get("https://api.example.com/users")
print(response.status_code)     # HTTP status code
print(response.text)            # Raw text
print(response.json())          # JSON (if valid)
```

---

### 📦 **Send Query Parameters**

```python
params = {"page": 2, "limit": 5}
response = requests.get("https://api.example.com/users", params=params)
print(response.url)  # Shows final URL with ?page=2&limit=5
```
---

### 📬 **POST JSON Data**

```python
data = {"name": "Alice", "email": "alice@example.com"}
response = requests.post("https://api.example.com/users", json=data)
print(response.status_code)
print(response.json())
```


> `json=data` automatically adds `Content-Type: application/json`

---

### 🧾 **POST Form Data**

```python
form = {"username": "alice", "password": "secret"}
response = requests.post("https://api.example.com/login", data=form)
```

---

### 🧱 **PUT / PATCH / DELETE**

```python
# Update (PUT)
requests.put("https://api.example.com/users/42", json={"name": "Alice Updated"})

# Partial update (PATCH)
requests.patch("https://api.example.com/users/42", json={"email": "new@mail.com"})

# Delete
requests.delete("https://api.example.com/users/42")
```
---

### 🧩 **Custom Headers**

```python
headers = {
    "Authorization": "Bearer mytoken123",
    "Accept": "application/json"
}
response = requests.get("https://api.example.com/profile", headers=headers)
```
---

### 🧠 **Handle JSON Response**

```python
if response.ok:
    data = response.json()
    print(data["username"])
else:
    print("Error:", response.status_code)
```

---

### 📤 **Upload a File**

```python
files = {"file": open("photo.png", "rb")}
response = requests.post("https://api.example.com/upload", files=files)
print(response.json())
```

---

### 📥 **Download a File**

```python
url = "https://example.com/file.zip"
response = requests.get(url)
open("file.zip", "wb").write(response.content)
```

---

### 🔁 **Session (Reuse Cookies, Headers, Auth)**

```python
session = requests.Session()
session.headers.update({"User-Agent": "MyApp/1.0"})
session.get("https://api.example.com/start")
session.post("https://api.example.com/login", data={"user": "admin"})
```
---

### ⏱️ **Timeouts & Errors**

```python
try:
    response = requests.get("https://api.example.com", timeout=5)
    response.raise_for_status()
except requests.exceptions.Timeout:
    print("Request timed out!")
except requests.exceptions.HTTPError as e:
    print("HTTP error:", e)
```

---

### 🔐 **Authentication**

#### Basic Auth

```python
from requests.auth import HTTPBasicAuth
response = requests.get("https://api.example.com/secure", auth=HTTPBasicAuth("user", "pass"))
```

#### Token Auth

```python
headers = {"Authorization": "Bearer <token>"}
response = requests.get("https://api.example.com/secure", headers=headers)
```

---

### 🧾 **Inspect Response**

|Attribute|Description|
|---|---|
|`response.status_code`|HTTP code|
|`response.headers`|Dict of response headers|
|`response.text`|Body as text|
|`response.content`|Raw bytes|
|`response.json()`|Parsed JSON|
|`response.ok`|True if 200–399|
|`response.elapsed`|Response time|

---

### 🧰 **Useful Flags**

|Feature|Example|
|---|---|
|Timeout|`timeout=5`|
|Stream large files|`stream=True`|
|Disable SSL verify|`verify=False` _(only for testing!)_|
|Redirect control|`allow_redirects=False`|

---

### 🧠 **Example: Realistic API Flow**

```python
import requests

BASE = "https://api.example.com"
headers = {"Authorization": "Bearer token123"}

# 1. Fetch user
user = requests.get(f"{BASE}/users/42", headers=headers).json()

# 2. Update something
update = {"active": False}
r = requests.patch(f"{BASE}/users/42", json=update, headers=headers)

# 3. Verify
print(r.status_code, r.json())
```

---

### 🧩 **Quick Summary**

|Operation|Method|Usage|
|---|---|---|
|Fetch data|`get()`|Retrieve|
|Send data|`post()`|Create|
|Update all|`put()`|Replace|
|Update partial|`patch()`|Modify|
|Delete|`delete()`|Remove|
|Upload file|`files=`|Send binary|
|JSON body|`json=`|Send structured data|
|Custom headers|`headers=`|Auth, CORS, etc.|

---

### ⚙️ **Pro Tip**

You can test your own backend routes locally:

```bash
python3 -m http.server 8080
```

Then hit it with:

```python
requests.get("http://localhost:8080")
```