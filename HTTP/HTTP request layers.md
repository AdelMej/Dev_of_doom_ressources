## 🌐 **HTTP Request Layer Cheat Sheet**

---

### 🧱 **1. OSI Model Context**

HTTP lives at the **Application Layer (Layer 7)** of the OSI model.  
But each HTTP request travels through all layers below it:

|Layer|Protocols / Concepts|What It Does|
|---|---|---|
|**7. Application**|HTTP, HTTPS, DNS, FTP, SMTP|Provides user-facing services (like web requests)|
|**6. Presentation**|TLS/SSL, Encryption, Compression|Secures or encodes data for transport|
|**5. Session**|TLS session, Cookies, Authentication|Manages connections and sessions|
|**4. Transport**|TCP (HTTP/1, HTTP/2), QUIC (HTTP/3), UDP|Handles data transfer and reliability|
|**3. Network**|IP, ICMP|Routes packets between devices|
|**2. Data Link**|Ethernet, Wi-Fi|Moves frames inside local networks|
|**1. Physical**|Cables, radio waves, fiber|Physical transmission of bits|

So when you do:

```bash
curl https://example.com
```
 
You’re actually going through **7 layers of protocols** to get a web page 🤯

---

### 📦 **2. HTTP Request Structure (Application Layer)**

A single HTTP request has **three main parts**:
```css
REQUEST LINE
HEADERS
BODY
```

#### 🧩 Example:

```pgsql
POST /api/login HTTP/1.1
Host: example.com
User-Agent: curl/8.0
Content-Type: application/json
Content-Length: 45
Authorization: Bearer <token>

{"username":"admin","password":"1234"}
```

|Part|Description|
|---|---|
|**Request Line**|Method + Path + Protocol version|
|**Headers**|Metadata (content type, auth, cache, etc.)|
|**Body**|Actual data (optional — usually in POST/PUT)|

---

### ⚙️ **3. HTTP Request Lifecycle**

1. **DNS Lookup** → Resolve `example.com` → IP address
2. **TCP or QUIC Connection** → Handshake with the server
3. **TLS Negotiation** _(if HTTPS)_ → Encrypt session
4. **HTTP Request Sent** → Method, headers, body
5. **Server Processes Request** → App logic runs
6. **HTTP Response Returned** → Status, headers, body
7. **Connection Closed or Reused** → Depends on `Connection: keep-aliv

---

### 🚀 **4. HTTP Versions Overview**

|Version|Transport|Key Features|
|---|---|---|
|**HTTP/1.1**|TCP|Persistent connections, chunked transfer|
|**HTTP/2**|TCP|Multiplexing, binary framing, header compression|
|**HTTP/3**|QUIC (UDP-based)|Low latency, connection migration, no head-of-line blocking|

---

### 🔐 **5. HTTPS Layer (Encryption)**

HTTPS = HTTP + TLS

- Uses **port 443** (default)
- Adds **TLS handshake** before data exchange
- Everything above Layer 4 becomes encrypted (headers + body)
- Certificates ensure **authenticity + integrity**

So:

 ```pgsql
HTTP   → cleartext, easy to inspect (port 80)
HTTPS  → encrypted, secure (port 443)
 ```
---

### 🧠 **6. Request Flow (Simplified)**

```scss
[Browser or curl]
   ↓
Application Layer (HTTP)
   ↓
Transport Layer (TCP or QUIC)
   ↓
Network Layer (IP)
   ↓
Data Link Layer (Ethernet/Wi-Fi)
   ↓
Physical Layer (Electric/Radio signals)
   ↓
[Server receives and responds]
```

---

### 🧩 **7. Quick Reference Table**

|Concept|Layer|Protocol/Example|Description|
|---|---|---|---|
|URL|Application|HTTP(S)|Resource identifier|
|Request Method|Application|GET, POST, PUT, DELETE|Defines operation type|
|Headers|Application|Content-Type, Authorization|Meta info about the request|
|TCP Port|Transport|80 / 443|Identifies service endpoint|
|Encryption|Presentation|TLS 1.3|Secures data|
|IP Address|Network|IPv4 / IPv6|Identifies machine|
|MAC Address|Data Link|Ethernet / Wi-Fi|Hardware address|
|Bits|Physical|Copper/Fiber/Wireless|Actual transmission|

---

### 🔥 **8. Curl Example with Layer Awareness**

`curl -v https://example.com/api/status`

You’ll see:

- DNS resolution (Layer 3)
- TCP/TLS handshake (Layers 4–6)
- HTTP headers and body (Layer 7)

So with `-v`, curl literally shows you **the full protocol stack in action**. 👀

---

✅ **TL;DR Summary**

| Layer           | Example in HTTP Request          |
| --------------- | -------------------------------- |
| 7. Application  | The actual HTTP message          |
| 6. Presentation | TLS encrypting that message      |
| 5. Session      | Connection/cookie management     |
| 4. Transport    | TCP/QUIC ensures delivery        |
| 3. Network      | IP routes packets to destination |
| 2. Data Link    | Ethernet/Wi-Fi frames data       |
| 1. Physical     | Sends the bits across hardware   |