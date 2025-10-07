## 📜 **HTTP Headers Cheat Sheet (2025 Edition)**

### 🧾 **1. Common Request Headers**

Headers your client (browser, script, or curl) sends to the server.

|Header|Purpose|Example|
|---|---|---|
|`Host`|Target hostname (mandatory in HTTP/1.1)|`Host: api.example.com`|
|`User-Agent`|Identifies client software|`User-Agent: curl/8.6.0`|
|`Accept`|Specifies acceptable content types|`Accept: application/json`|
|`Accept-Language`|Preferred language(s)|`Accept-Language: en-US,en;q=0.9`|
|`Accept-Encoding`|Supported compression methods|`Accept-Encoding: gzip, br`|
|`Content-Type`|Type of body being sent|`Content-Type: application/json`|
|`Content-Length`|Size of request body (bytes)|`Content-Length: 245`|
|`Referer`|Page that originated the request|`Referer: https://mysite.com/login`|
|`Origin`|Origin of request (used in CORS)|`Origin: https://frontend.app`|
|`Authorization`|Authentication credentials|`Authorization: Bearer <token>`|
|`Cookie`|Sends session cookies to server|`Cookie: session=abc123`|
|`Cache-Control`|Caching behavior for request|`Cache-Control: no-cache`|

---

### 💬 **2. Common Response Headers**

Headers the server sends back to describe the response.

|Header|Purpose|Example|
|---|---|---|
|`Content-Type`|Type of returned content|`Content-Type: application/json`|
|`Content-Length`|Size in bytes|`Content-Length: 1024`|
|`Server`|Web server software|`Server: nginx/1.24.0`|
|`Date`|Response timestamp|`Date: Mon, 06 Oct 2025 20:42:00 GMT`|
|`Set-Cookie`|Sends cookies to client|`Set-Cookie: token=xyz; HttpOnly; Secure`|
|`Location`|Redirect target|`Location: /login`|
|`Connection`|Control connection behavior|`Connection: keep-alive`|
|`Transfer-Encoding`|Data transfer format|`Transfer-Encoding: chunked`|
|`Content-Encoding`|Compression type used|`Content-Encoding: gzip`|

---

### 🔒 **3. Authentication Headers**

Used for login, tokens, or API key validation.

|Header|Purpose|Example|
|---|---|---|
|`Authorization`|Auth credentials|`Authorization: Basic dXNlcjpzZWNyZXQ=`|
|`Proxy-Authorization`|Auth for proxy servers|`Proxy-Authorization: Basic dXNlcjpzZWNyZXQ=`|
|`WWW-Authenticate`|Server challenges client|`WWW-Authenticate: Basic realm="Restricted"`|
|`Authentication-Info`|Auth metadata|`Authentication-Info: nextnonce="xyz"`|

---

### ⚙️ **4. Caching & Optimization**

Control how responses are stored or revalidated.

|Header|Purpose|Example|
|---|---|---|
|`Cache-Control`|Main caching policy|`Cache-Control: max-age=3600, must-revalidate`|
|`Expires`|Expiration date/time|`Expires: Tue, 07 Oct 2025 20:00:00 GMT`|
|`ETag`|Unique version identifier|`ETag: "abc123"`|
|`If-None-Match`|Client validation check|`If-None-Match: "abc123"`|
|`Last-Modified`|Timestamp of resource update|`Last-Modified: Mon, 06 Oct 2025 18:00:00 GMT`|
|`Vary`|Determines cache variance|`Vary: Accept-Encoding`|

---

### 🌍 **5. CORS (Cross-Origin Resource Sharing)**

Used when browsers request data from another domain.

|Header|Direction|Purpose|Example|
|---|---|---|---|
|`Origin`|Request|Specifies the caller’s domain|`Origin: https://frontend.app`|
|`Access-Control-Allow-Origin`|Response|Allowed origins|`Access-Control-Allow-Origin: *`|
|`Access-Control-Allow-Methods`|Response|Allowed HTTP methods|`Access-Control-Allow-Methods: GET, POST`|
|`Access-Control-Allow-Headers`|Response|Allowed custom headers|`Access-Control-Allow-Headers: Content-Type, Authorization`|
|`Access-Control-Allow-Credentials`|Response|Allow credentials (cookies)|`Access-Control-Allow-Credentials: true`|
|`Access-Control-Max-Age`|Response|Cache preflight time (seconds)|`Access-Control-Max-Age: 86400`|

---

### 🧰 **6. Security Headers**

Help protect users and browsers.

|Header|Purpose|Example|
|---|---|---|
|`Strict-Transport-Security`|Force HTTPS|`Strict-Transport-Security: max-age=31536000; includeSubDomains`|
|`X-Content-Type-Options`|Prevent MIME sniffing|`X-Content-Type-Options: nosniff`|
|`X-Frame-Options`|Prevent clickjacking|`X-Frame-Options: DENY`|
|`Content-Security-Policy`|Whitelist allowed sources|`Content-Security-Policy: default-src 'self'`|
|`Referrer-Policy`|Control Referer header behavior|`Referrer-Policy: no-referrer-when-downgrade`|
|`Permissions-Policy`|Control browser features|`Permissions-Policy: geolocation=(), microphone=()`|

---

### 🧩 **7. Developer & Debugging Headers**

Useful when testing APIs or performance.

|Header|Purpose|Example|
|---|---|---|
|`X-Request-ID`|Track a request across services|`X-Request-ID: 8f7a2a1b`|
|`X-RateLimit-Limit`|Max requests allowed|`X-RateLimit-Limit: 1000`|
|`X-RateLimit-Remaining`|Requests left|`X-RateLimit-Remaining: 997`|
|`Retry-After`|Wait time after throttling|`Retry-After: 30`|
|`X-Debug-Info`|Custom debug data|`X-Debug-Info: trace=42`|

---

### 💡 **Examples with `curl`**

**Inspect Headers Only**

```bash
curl -I https://api.example.com
```

**Send Custom Headers**

```bash
curl -H "Authorization: Bearer mytoken" \
     -H "Accept: application/json" \
     https://api.example.com/data
```

**View Request + Response Headers (Verbose)**

```bash
curl -v https://api.example.com/test
```

---

### 🔍 **Pro Tip**

You can pipe curl output through `grep` to quickly extract specific headers:

```bash
curl -I https://api.example.com | grep Content-Type
```