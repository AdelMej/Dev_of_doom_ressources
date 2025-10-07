## 🌐 **HTTP / HTTPS / QUIC Cheat Sheet**

---

### ⚙️ 1. Overview

|Protocol|Layer|Transport|Encrypted|Default Port|Key Feature|
|---|---|---|---|---|---|
|**HTTP/1.1**|Application|TCP|❌ No|80|Plain-text web protocol|
|**HTTPS**|Application|TCP + TLS|✅ Yes|443|Secure HTTP using TLS|
|**HTTP/2**|Application|TCP + TLS (usually)|✅ Yes|443|Multiplexed requests|
|**HTTP/3 (QUIC)**|Application|**QUIC (UDP)**|✅ Yes|443|Multiplexed + 0-RTT + faster|

---

### 🧩 2. Protocol Stack View

```HTTP
HTTP/1.1:  Application Layer → TCP → IP
HTTPS:     Application Layer (HTTP) → TLS → TCP → IP
HTTP/3:    Application Layer (HTTP) → QUIC → UDP → IP
```

---

### 🌍 3. HTTP Basics

**Plain-text requests/responses:**

```HTTP
GET /index.html HTTP/1.1
Host: example.com
User-Agent: curl/8.0

HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 42
```

**Key properties:**

- Stateless — each request independent
- Methods: `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, `OPTIONS`
- Headers define metadata
- Bodies (for POST/PUT) carry data
- No encryption by default

**Typical use:**  
Local or internal APIs where encryption isn’t needed.

---

### 🔐 4. HTTPS (HTTP + TLS)

- Wraps HTTP traffic in **TLS** (Transport Layer Security).
- Encrypts everything after the TLS handshake.
- Provides:
    
    - **Confidentiality** (nobody can read the data)
    - **Integrity** (no tampering)
    - **Authentication** (via certificates)


**Handshake summary:**

1. Client → Server: "Hello" (supported ciphers, TLS version)
2. Server → Client: Certificate + chosen cipher
3. Keys exchanged (ECDHE usually)
4. Both sides derive shared symmetric key
5. Encrypted data flows over TCP

✅ Always use HTTPS for production.  
⚠️ Self-signed certificates are OK for local testing only.

---

### 🚀 5. HTTP/2 (over TLS)

Built to fix HTTP/1.1 inefficiencies:

- **Multiplexing:** many requests on one TCP connection
- **Header compression (HPACK)**
- **Server push** (optional)
- **Binary framing** (not plain text)
    

✅ Default for modern browsers and web servers  
⚠️ Still runs over TCP — suffers from _head-of-line blocking_ (if one TCP packet drops, others wait)

---

### ⚡ 6. QUIC / HTTP/3

> Next-gen transport replacing TCP for HTTP.

**Core idea:**

- QUIC = “Quick UDP Internet Connections”
- Runs over **UDP**, but implements:
    
    - Encryption (TLS 1.3 built-in)
    - Multiplexed streams
    - Connection migration (same session after IP change)
    - 0-RTT startup (no handshake delay)

**Benefits:**

- Faster connections (esp. on mobile)
- Resists head-of-line blocking
- Safer against packet loss or network switching

**Cons:**

- Harder to debug (encrypted at transport)
- Requires QUIC-aware firewalls/proxies
- Still newer in enterprise setups

**Enabled in:**

- Chrome, Firefox, Edge
- Nginx (with quiche), Caddy, Cloudflare, LiteSpeed, etc.

---

### 🔍 7. Ports & CLI Testing

| Protocol      | Command                                      | Example             |
| ------------- | -------------------------------------------- | ------------------- |
| HTTP          | `curl http://localhost:8080`                 | Plain test          |
| HTTPS         | `curl -v https://example.com`                | TLS + cert info     |
| HTTP/2        | `curl --http2 -I https://example.com`        | Force HTTP/2        |
| HTTP/3 (QUIC) | `curl --http3 -I https://example.com`        | Test QUIC support   |
| QUIC raw      | `nmap --script quic-info -p 443 example.com` | Detect QUIC support |

---

### 🧠 8. Quick Differences Summary

| Feature                       | HTTP/1.1 | HTTPS                  | HTTP/2    | HTTP/3 (QUIC) |
| ----------------------------- | -------- | ---------------------- | --------- | ------------- |
| Transport                     | TCP      | TCP + TLS              | TCP + TLS | UDP + QUIC    |
| Multiplexing                  | ❌        | ❌                      | ✅         | ✅             |
| Encryption                    | ❌        | ✅                      | ✅         | ✅             |
| Head-of-line blocking         | ✅        | ✅                      | ✅         | ❌             |
| Connection resume             | ❌        | ✅ (TLS session resume) | ✅         | ✅ (better)    |
| Built-in handshake encryption | ❌        | ✅                      | ✅         | ✅             |
| Default port                  | 80       | 443                    | 443       | 443           |
| Speed on lossy links          | Poor     | Poor                   | Better    | Excellent     |

---

### 🔧 9. Quick Troubleshooting Commands

```bash
# Check supported HTTP versions
curl -I -v https://example.com

# Check QUIC availability
curl -I --http3 https://example.com

# View TLS certificate details
openssl s_client -connect example.com:443 -servername example.com

# Measure response time
curl -w "Time: %{time_total}s\n" -o /dev/null -s https://example.com
```

---

### 💬 10. TL;DR Summary

|Layer|Protocol|Description|
|---|---|---|
|App|**HTTP/1.1**|Old, text-based, no encryption|
|App|**HTTPS**|HTTP + TLS (secure)|
|App|**HTTP/2**|Faster multiplexed HTTP over TCP|
|App|**HTTP/3**|HTTP over QUIC (UDP) — newest, fastest|