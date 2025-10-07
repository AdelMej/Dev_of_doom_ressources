## ⚙️ **HTTP Status Code Cheat Sheet**

HTTP responses always come with a **status code** that tells you what happened.  
They’re grouped by the **first digit**:

|Range|Meaning|
|---|---|
|**1xx**|Informational – request received, processing continues|
|**2xx**|Success – request handled correctly|
|**3xx**|Redirection – client must take more action|
|**4xx**|Client error – problem with request|
|**5xx**|Server error – problem on server side|

---

### 🔹 **1xx — Informational**

| Code                        | Meaning                                               |
| --------------------------- | ----------------------------------------------------- |
| **100** Continue            | Server got headers, client should send body           |
| **101** Switching Protocols | For WebSocket or HTTP upgrade                         |
| **102** Processing          | WebDAV / async ops                                    |
| **103** Early Hints         | Send headers before final response (used for preload) |

---

### ✅ **2xx — Success**

| Code                           | Meaning                                     |
| ------------------------------ | ------------------------------------------- |
| **200** OK                     | Generic success                             |
| **201** Created                | Resource created (POST/PUT)                 |
| **202** Accepted               | Request accepted but still processing       |
| **203** Non-Authoritative Info | Modified response (proxy/intermediate)      |
| **204** No Content             | Success, no body returned                   |
| **205** Reset Content          | Success, client should reset form/view      |
| **206** Partial Content        | Used for ranged requests (downloads/resume) |
| **207** Multi-Status           | WebDAV multi-resource response              |

---

### 🔁 **3xx — Redirection**

| Code                       | Meaning                                             |
| -------------------------- | --------------------------------------------------- |
| **300** Multiple Choices   | Several options available                           |
| **301** Moved Permanently  | Resource moved; update links                        |
| **302** Found              | Temporary redirect (often used for login redirects) |
| **303** See Other          | Redirect using GET (after POST)                     |
| **304** Not Modified       | Cached version still valid                          |
| **307** Temporary Redirect | Like 302, but preserves method                      |
| **308** Permanent Redirect | Like 301, but preserves method                      |

---

### ⚠️ **4xx — Client Errors**

| Code                                    | Meaning                                        |
| --------------------------------------- | ---------------------------------------------- |
| **400** Bad Request                     | Invalid syntax, missing params, malformed JSON |
| **401** Unauthorized                    | Missing or invalid auth credentials            |
| **403** Forbidden                       | Authenticated but not allowed                  |
| **404** Not Found                       | Resource doesn’t exist                         |
| **405** Method Not Allowed              | Wrong HTTP method (e.g., POST instead of GET)  |
| **406** Not Acceptable                  | Content type not acceptable                    |
| **407** Proxy Auth Required             | Must authenticate with proxy                   |
| **408** Request Timeout                 | Client took too long to send                   |
| **409** Conflict                        | Resource conflict (e.g., version mismatch)     |
| **410** Gone                            | Resource permanently removed                   |
| **411** Length Required                 | Missing `Content-Length` header                |
| **412** Precondition Failed             | `If-*` headers not met                         |
| **413** Payload Too Large               | Body exceeds server limit                      |
| **414** URI Too Long                    | URL too big                                    |
| **415** Unsupported Media Type          | Wrong `Content-Type`                           |
| **416** Range Not Satisfiable           | Invalid range in request                       |
| **417** Expectation Failed              | `Expect` header not fulfilled                  |
| **418** I'm a Teapot 🫖                 | RFC joke (Easter egg from 1998)                |
| **422** Unprocessable Entity            | Valid syntax, but invalid data (often in APIs) |
| **425** Too Early                       | Retry after more time                          |
| **426** Upgrade Required                | Needs HTTPS or another protocol                |
| **429** Too Many Requests               | Rate limit exceeded                            |
| **431** Request Header Fields Too Large | Headers too big                                |
| **451** Unavailable For Legal Reasons   | Blocked due to legal reasons (e.g., DMCA)      |

---

### 💥 **5xx — Server Errors**

|Code|Meaning|Notes|
|---|---|---|
|**500** Internal Server Error|Generic “something broke”||
|**501** Not Implemented|Endpoint/method not supported||
|**502** Bad Gateway|Proxy/gateway got invalid response from upstream||
|**503** Service Unavailable|Server overloaded or under maintenance||
|**504** Gateway Timeout|Upstream didn’t respond||
|**505** HTTP Version Not Supported|Unsupported HTTP version||
|**507** Insufficient Storage|Out of disk/memory (WebDAV)||
|**508** Loop Detected|Infinite loop detected in request processing||
|**510** Not Extended|Further extensions required||
|**511** Network Authentication Required|Client must authenticate for network (captive portals)||

---

### 🧠 **Common Combos You’ll Actually See**

|Situation|Typical Code|
|---|---|
|API success|**200 / 201**|
|Created a new resource|**201 Created**|
|Empty success (no response body)|**204 No Content**|
|Redirected after login|**302 Found**|
|Cached GET request|**304 Not Modified**|
|Bad JSON in request|**400 Bad Request**|
|Missing or bad token|**401 Unauthorized**|
|Auth ok but user not allowed|**403 Forbidden**|
|Resource missing|**404 Not Found**|
|Rate limit hit|**429 Too Many Requests**|
|Server crash or exception|**500 Internal Server Error**|
|Downstream microservice failed|**502 Bad Gateway / 504 Gateway Timeout**|
|Maintenance mode|**503 Service Unavailable**|

---

### 🧰 **Quick curl Testing Examples**

```bash
# Test a route and show status only
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8080/api/status

# Simulate a POST and see 201 Created
curl -i -X POST -d '{"name":"test"}' -H "Content-Type: application/json" http://localhost:8080/api/users

# Handle 404 cleanly in a script
if [ "$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8080/nothing)" = "404" ]; then
  echo "Resource not found!"
fi
```
---

### 🧾 **TL;DR Summary**

|Group|Meaning|Example|
|---|---|---|
|1xx|Info|“Got it, keep going.”|
|2xx|Success|“All good.”|
|3xx|Redirect|“Go somewhere else.”|
|4xx|Client error|“Your request was wrong.”|
|5xx|Server error|“Our server messed up.”|