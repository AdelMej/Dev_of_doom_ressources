## ⚙️ **REST API Methods Cheat Sheet (2025 Edition)**

### 🧩 **Core REST Methods**

|Method|Purpose|Request Body|Response Body|Idempotent|Safe|Typical Use|
|---|---|---|---|---|---|---|
|**GET**|Retrieve a resource|❌|✅|✅|✅|Read data|
|**POST**|Create a new resource|✅|✅|❌|❌|Create / submit data|
|**PUT**|Replace an existing resource|✅|✅|✅|❌|Full update|
|**PATCH**|Partially update a resource|✅|✅|❌|❌|Partial update|
|**DELETE**|Remove a resource|❌|✅|✅|❌|Delete data|
|**HEAD**|Get metadata only (no body)|❌|❌|✅|✅|Check if exists|
|**OPTIONS**|Get supported methods|❌|✅|✅|✅|CORS / discovery|

---

### 🧠 **Quick Definitions**

- **Idempotent:** Same request → same result every time.
- **Safe:** Doesn’t change server state (only reads).

---

### 🧱 **Typical REST Routes**

|Action|Method|Example Endpoint|
|---|---|---|
|List all users|`GET`|`/users`|
|Get one user|`GET`|`/users/42`|
|Create a user|`POST`|`/users`|
|Replace user|`PUT`|`/users/42`|
|Update email only|`PATCH`|`/users/42`|
|Delete user|`DELETE`|`/users/42`|

---

### 🧰 **curl Examples**

#### 🟢 **GET** – Read Data

```bash
curl -X GET https://api.example.com/users
```

Retrieve all users.  
Add headers if needed:

```bash
curl -H "Accept: application/json" https://api.example.com/users/1
```

---

#### 🔵 **POST** – Create Resource

```bash
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice", "email": "alice@example.com"}'
```

Creates a new user and usually returns `201 Created` with the new resource.

---

#### 🟣 **PUT** – Full Replace

```bash
curl -X PUT https://api.example.com/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name": "Alice Updated", "email": "alice@new.com"}'
```

Completely replaces user #1’s data.  
All unspecified fields are removed.

---

#### 🟠 **PATCH** – Partial Update

```bash
curl -X PATCH https://api.example.com/users/1 \
  -H "Content-Type: application/json" \
  -d '{"email": "new@example.com"}'
```

Updates only the provided fields.  
Response is often `200 OK` or `204 No Content`.

---

#### 🔴 **DELETE** – Remove Resource

```bash
curl -X DELETE https://api.example.com/users/1
```

Removes the user.  
Common responses: `204 No Content` or `404 Not Found` if it doesn’t exist.

---

#### ⚪ **HEAD** – Check Resource Exists

```bash
curl -I https://api.example.com/users/1
```

Same as `GET`, but only returns headers (no body).

---

#### 🟡 **OPTIONS** – Discover Server Capabilities

```bash
curl -X OPTIONS https://api.example.com/users -i
```

Server returns something like:

```pgsql
Allow: GET, POST, OPTIONS
Access-Control-Allow-Methods: GET, POST
```

---

### 🧠 **Status Codes by Method (Typical)**

|Method|Success Codes|Common Errors|
|---|---|---|
|`GET`|`200 OK`, `304 Not Modified`|`404 Not Found`|
|`POST`|`201 Created`, `202 Accepted`|`400 Bad Request`, `409 Conflict`|
|`PUT`|`200 OK`, `204 No Content`|`400`, `404`|
|`PATCH`|`200 OK`, `204 No Content`|`400`, `404`|
|`DELETE`|`204 No Content`|`404 Not Found`|

---

### 🧩 **Special Notes**

- `PUT` = full replacement
- `PATCH` = partial update
- `POST` = new resource (non-idempotent)
- `GET` = read-only
- `DELETE` = self-explanatory 😅
- `HEAD` + `OPTIONS` = meta / discovery tools

---

### 🧪 **Pro Tips**

- Combine with `-v` to debug headers:
```bash
   curl -v -X POST ...
   ```    
- Send multiple headers easily:
    
```bash
	curl -H "Accept: application/json" -H "Authorization: Bearer token" ...
	```

- Use `-i` to include response headers in output:
```bash
curl -i -X GET ...
```