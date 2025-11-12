## 🧩 **JSON & MIME Types Cheat Sheet (2025 Edition)**

### 📦 **What is MIME Type?**

> MIME = **Multipurpose Internet Mail Extensions**  
> It tells the receiver _what kind of data_ is being sent.  
> In HTTP, it’s declared using the `Content-Type` header.

---

### 🧠 **Common MIME (Content-Type) Values**

|MIME Type|Description|Typical Use|
|---|---|---|
|`application/json`|JSON data|REST APIs, config data|
|`application/xml`|XML data|Legacy systems|
|`text/plain`|Raw text|Debug, logs|
|`text/html`|HTML content|Web pages|
|`application/javascript`|JS scripts|Web assets|
|`application/x-www-form-urlencoded`|URL-encoded form|HTML form submissions|
|`multipart/form-data`|File uploads|Images, documents|
|`application/octet-stream`|Binary file|File downloads, raw data|
|`image/png`, `image/jpeg`|Image content|Media APIs|
|`application/pdf`|PDF files|Downloads, docs|
|`text/css`|CSS stylesheet|Web apps|
|`text/csv`|CSV data|Data export/import|

---

### 🧾 **Request + Response Headers Example**

**Request**

```bash
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Accept: application/json
```
**Response**

```bash
HTTP/1.1 201 Created
Content-Type: application/json
```
---

### ⚙️ **JSON Basics**

JSON = **JavaScript Object Notation** — lightweight and human-readable.

#### ✅ Example Object

```json
{
  "id": 42,
  "name": "Alice",
  "email": "alice@example.com",
  "roles": ["admin", "editor"],
  "active": true
}
```
#### ✅ Example Array

```json
[
  { "id": 1, "task": "Setup server" },
  { "id": 2, "task": "Deploy API" }
]
```

---

### 💡 **curl Examples**

#### 🟢 **Send JSON (POST)**

```bash
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice","email":"alice@example.com"}'
```

#### 🟣 **Update Resource (PUT)**

```bash
curl -X PUT https://api.example.com/users/42 \
  -H "Content-Type: application/json" \
  -d '{"name":"Alice Updated"}'
```

#### 🔵 **Send Form Data**

```bash
curl -X POST https://api.example.com/login \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "username=alice&password=secret"
```

#### 🟠 **Upload a File**

```bash
curl -X POST https://api.example.com/upload \
  -F "file=@photo.png" \
  -F "description=Profile picture"
```

> `-F` automatically sets `Content-Type: multipart/form-data`.

---

### 📬 **Content Negotiation**

The client can request a specific response type via the `Accept` header.

|Header|Meaning|
|---|---|
|`Accept: application/json`|Ask for JSON response|
|`Accept: text/html`|Ask for HTML page|
|`Accept: */*`|Accept anything (default)|

Example:

```bash
curl -H "Accept: application/json" https://api.example.com/users/1
```

---

### 🧱 **Response Examples**

#### ✅ **JSON Response**

```json
{
  "status": "success",
  "data": { "id": 42, "name": "Alice" }
}
```

#### 📄 **HTML Response**

```html
<html><body><h1>User Created</h1></body></html>
```

#### 🧾 **Plain Text Response**

```
User created successfully.
```

---

### 🧠 **Special MIME Notes**

|MIME|Notes|
|---|---|
|`application/json`|Most modern APIs use this by default|
|`multipart/form-data`|Used for file uploads (`@filename`)|
|`application/x-www-form-urlencoded`|Simpler form submissions|
|`application/octet-stream`|Generic binary stream (e.g. `.exe`, `.bin`)|
|`text/*`|Always UTF-8 unless charset specified|

---

### ⚙️ **Pro Tip**

To **see both headers + content type** in a response:

```bash
curl -i https://api.example.com/test
```

To **only get the Content-Type**:

```bash
curl -sI https://api.example.com/test | grep Content-Type
```

---

### 🔐 **Security Tip**

If your API serves JSON:

`Content-Type: application/json; charset=utf-8 X-Content-Type-Options: nosniff`

This prevents browsers from misinterpreting JSON as HTML (important for security).

---

### 🧩 **Quick Reference Summary**

| Data Type   | MIME                                | Usage                  |
| ----------- | ----------------------------------- | ---------------------- |
| JSON        | `application/json`                  | REST APIs              |
| Form        | `application/x-www-form-urlencoded` | HTML form data         |
| File Upload | `multipart/form-data`               | Upload images/files    |
| Plain Text  | `text/plain`                        | Logs, messages         |
| HTML        | `text/html`                         | Web pages              |
| Binary      | `application/octet-stream`          | File download/transfer |