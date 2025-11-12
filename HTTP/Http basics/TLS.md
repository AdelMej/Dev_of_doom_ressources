## 🔐 **TLS Cheat Sheet (Transport Layer Security)**

---

### 🧱 **1. What TLS Does**

|Goal|Description|
|---|---|
|**Encryption**|Protects data so nobody can read it in transit|
|**Authentication**|Verifies the server’s (and optionally client’s) identity|
|**Integrity**|Detects tampering with data|
|**Forward Secrecy**|Prevents old sessions from being decrypted later, even if keys leak|

So TLS = **Secure Transport Layer for HTTP, SMTP, FTP, IMAP, etc.**  
HTTPS is just **HTTP + TLS** (on port 443 instead of 80).

---

### ⚙️ **2. TLS in the OSI Stack**

|Layer|Protocol|
|---|---|
|**7 – Application**|HTTP, SMTP, etc.|
|**6 – Presentation**|🔒 **TLS/SSL** (encryption, compression)|
|**5 – Session**|TLS session state, resumption|
|**4 – Transport**|TCP (HTTP/1, 2) or QUIC (HTTP/3)|

So TLS sits **between HTTP and TCP**, wrapping HTTP messages inside an encrypted tunnel.

---

### 🧩 **3. TLS Handshake Steps**

Here’s what happens before a secure HTTP request even begins 👇

#### 🔹 Step-by-step:

1. **Client Hello**
    
    - Client says: "Hey, I support TLS versions X–Y and these ciphers."
    - Sends random data + possible extensions (SNI, ALPN, etc.)

2. **Server Hello**
    
    - Chooses protocol + cipher suite
    - Sends **digital certificate** (proves identity)
    - Sends its own random data

3. **Key Exchange**
    
    - Client and server use public/private keys (often via **ECDHE**) 
    - Derive a **shared session key**

4. **Finished Messages**
    
    - Both sides verify handshake integrity
    - Switch to **encrypted mode**

✅ After this, all communication is encrypted.

---

### 🧠 **4. Key Concepts**

|Term|Description|
|---|---|
|**Cipher Suite**|Set of algorithms used (key exchange, encryption, hashing)|
|**Public/Private Keys**|Used for asymmetric encryption and identity proof|
|**Session Keys**|Temporary symmetric key for data encryption|
|**Certificate**|Digital ID proving the server’s authenticity|
|**CA (Certificate Authority)**|Trusted org that signs certificates|
|**SNI (Server Name Indication)**|Tells which hostname is requested (for shared IPs)|
|**ALPN (Application-Layer Protocol Negotiation)**|Negotiates HTTP/1.1 vs HTTP/2|
|**OCSP Stapling**|Optimized certificate revocation check|

---

### 🔑 **5. Common Cipher Suite Example**

`TLS_AES_256_GCM_SHA384`

|Part|Meaning|
|---|---|
|**TLS**|Protocol|
|**AES_256_GCM**|Encryption algorithm|
|**SHA384**|Hash for message integrity|

---

### 📜 **6. TLS Versions**

|Version|Status|Notes|
|---|---|---|
|**SSL 2.0 / 3.0**|❌ Deprecated|Insecure|
|**TLS 1.0 / 1.1**|❌ Deprecated|Outdated|
|**TLS 1.2**|✅ Still common|Secure and reliable|
|**TLS 1.3**|🟢 Current standard|Faster, safer, fewer round-trips|

💡 TLS 1.3 is what you want for all modern HTTPS connections.

---

### 🧰 **7. Inspecting TLS with curl & OpenSSL**

#### 🧪 Check a site’s certificate:
```bash
openssl s_client -connect example.com:443 -servername example.com
```
#### 🔍 Show certificate info:

```bash
openssl x509 -in cert.pem -text -noout
```

#### 🧾 Test TLS connection with curl:

```bash
curl -vI https://example.com
```

Look for:

```marksown
* TLSv1.3 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.3 / TLS_AES_256_GCM_SHA384
```

---

### 🧠 **8. TLS Certificates**

|Type|Description|
|---|---|
|**Self-signed**|Local testing; not trusted by browsers|
|**Domain Validated (DV)**|Basic domain ownership check|
|**Organization Validated (OV)**|Verifies org identity|
|**Extended Validation (EV)**|Strictest verification (green bar in old browsers)|
|**Wildcard**|Covers `*.example.com`|
|**SAN (Subject Alt Name)**|Multiple domains on one cert|

---

### 💾 **9. Where Certs Live (Linux)**

|File|Description|
|---|---|
|`/etc/ssl/certs/`|Trusted root certificates|
|`/etc/ssl/private/`|Private keys|
|`/etc/letsencrypt/live/`|Let’s Encrypt certs (if used)|

---

### 🛡️ **10. Best Practices for Servers**

✅ Always use **TLS 1.3**  
✅ Use **strong cipher suites** (`AES-GCM`, `CHACHA20`)  
✅ Enable **OCSP stapling** and **HSTS**  
✅ Use **Let’s Encrypt** for free certs  
✅ Renew certs automatically with `certbot`  
✅ Disable SSLv3, TLS 1.0, TLS 1.1  
✅ Redirect all HTTP → HTTPS

---

### 🧩 **11. TLS in Action (Request Flow)**

```pgsql
Client              Server
  |  ClientHello  ─────────▶
  |  ◀─────────  ServerHello + Certificate
  |  Key Exchange ─────────▶
  |  ◀─────────  Finished (Server)
  |  Finished (Client) ───▶
  |  [Encrypted Traffic 🔒]
```
After this, everything (HTTP headers, body, cookies, tokens) is fully encrypted.

---

### 🔥 **12. Quick Recap Table**

|Concept|Description|
|---|---|
|**TLS = Secure Transport Layer**|Protects any TCP-based protocol|
|**HTTPS = HTTP + TLS**|Secure web protocol|
|**Handshake**|Builds trust + session keys|
|**Cipher Suite**|Defines how data is secured|
|**TLS 1.3**|Modern, fast, secure|
|**Certificates**|Verify identity|
|**CA**|Signs certs you can trust|