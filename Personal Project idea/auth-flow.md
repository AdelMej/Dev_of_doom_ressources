# 🔐 Centralized Authentication Flow (JWT + Refresh Tokens)

## 🧩 Overview
This document describes the full authentication lifecycle for the centralized **DaMini Auth API**.  
It uses short-lived **JWT access tokens** and rotating **refresh tokens** stored in secure cookies.

---

## ⚙️ 1. Login Flow

### 1️⃣ User logs in
```http
POST /auth/login
{
  "email": "user@example.com",
  "password": "hunter2"
}
```

**Auth API actions:**
- Verify credentials
- Generate:
  - `access_token` (valid ~15 minutes)
  - `refresh_token` (valid ~7 days)
- Store hashed `refresh_token` in DB

**Response:**
```json
{
  "access_token": "<JWT>",
  "expires_in": 900
}
```
**Cookie:**
```
Set-Cookie: refresh_token=<value>; HttpOnly; Secure; SameSite=Strict; Path=/auth/refresh
```

---

## 🔁 2. Token Refresh Flow

When the JWT expires, the app calls:
```http
POST /auth/refresh
```
The browser automatically includes the refresh cookie.

### Backend validation:
1. Extract `refresh_token` from cookie  
2. Verify it in the database (hash check)  
3. Ensure not expired or revoked  

### If valid:
- Issue new `access_token`
- Rotate `refresh_token`
- Update cookie + DB entry

**Response:**
```json
{
  "access_token": "<new JWT>",
  "expires_in": 900
}
```

**New cookie:**
```
Set-Cookie: refresh_token=<new_value>; HttpOnly; Secure; SameSite=Strict; Path=/auth/refresh
```

---

## ⚔️ 3. Invalid Refresh Token

If validation fails:
```json
{
  "error": "invalid_refresh_token",
  "message": "Session expired. Please log in again."
}
```

**Frontend action:**
```js
if (response.status === 401 || response.error === "invalid_refresh_token") {
  window.location.href = "https://auth.example.com/login?redirect_uri=" + window.location.href;
}
```

➡️ Redirects user back to login for new session creation.

---

## 🧠 4. Token Validation Route

Your apps can validate tokens directly:
```http
GET /auth/validate
Authorization: Bearer <JWT>
```

**Response (valid):**
```json
{
  "valid": true,
  "user": {
    "id": "42",
    "email": "user@example.com",
    "roles": ["USER"]
  }
}
```

**Response (invalid):**
```json
{
  "valid": false,
  "reason": "Token expired or invalid"
}
```

---

## 🧩 5. Logout Flow

```http
POST /auth/logout
```

**Actions:**
- Delete the refresh token entry from DB  
- Clear the cookie  
- JWT naturally expires on its own  

**Response:**
```json
{ "success": true }
```

---

## ⏱️ 6. Token Lifetimes Summary

| Token | Lifetime | Storage | Rotation |
|--------|-----------|----------|-----------|
| Access Token | ~15 min | Client memory | No |
| Refresh Token | ~7–14 days | HttpOnly cookie | Yes (each use) |
| Max Session | ~30 days | N/A | User re-login required |

---

## ✅ 7. Security Guidelines

| Rule | Reason |
|------|--------|
| Use **HttpOnly**, **Secure**, **SameSite=Strict** cookies | Prevent XSS & CSRF |
| Hash refresh tokens in DB | Protect against DB leaks |
| Rotate refresh tokens on use | Detect reuse (session hijacking) |
| Use short-lived JWTs | Minimize risk exposure |
| Require re-login after max session age | Ensure periodic credential renewal |

---

## 🧭 Summary Flow

1. `/auth/login` → generate JWT + refresh cookie  
2. `/auth/refresh` → rotate refresh + renew JWT  
3. `/auth/validate` → verify active JWT  
4. `/auth/logout` → revoke refresh token  
5. `invalid_refresh_token` → redirect to `/login`

---

> 💡 The entire system is stateless for JWTs and session-aware for refresh tokens, giving you **speed**, **security**, and **full centralization**.
