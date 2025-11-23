# Full JWT Security Guide

_Comprehensive practical guide: Access + Refresh tokens, cookie integration, rotation, backend and frontend patterns, and security best practices._

---

## Overview
JWT-based authentication commonly uses two tokens:
- **Access token**: short-lived JWT used to authenticate API requests (`Authorization: Bearer <token>`). Short TTL (minutes).
- **Refresh token**: long-lived credential used to obtain new access tokens. Stored and treated more securely (often in an HTTP-only cookie).

Goal: keep the access token available for API calls while protecting the longer-lived refresh token from theft (XSS).

---

## Quick architecture patterns

### Recommended (modern, secure)
- Access token in **memory** (frontend JS variable)
- Refresh token in an **HttpOnly Secure SameSite cookie** (backend manages it)
- Frontend calls `/auth/refresh` when access expires; browser sends cookie automatically

### Simpler (developer-friendly, lower security)
- Access token in **localStorage** or in-memory
- Refresh token in **localStorage** (less secure; vulnerable to XSS)

---

## Backend (Flask + Flask-JWT-Extended) — Core snippets

### Configuration (cookies + secure defaults)
```py
app.config.update({
  "JWT_SECRET_KEY": "replace_with_real_secret",
  "JWT_TOKEN_LOCATION": ["cookies"],
  "JWT_ACCESS_COOKIE_NAME": "access_token",
  "JWT_REFRESH_COOKIE_NAME": "refresh_token",
  "JWT_COOKIE_SECURE": True,         # production: require HTTPS
  "JWT_COOKIE_HTTPONLY": True,
  "JWT_COOKIE_SAMESITE": "Lax",
  "JWT_COOKIE_CSRF_PROTECT": False   # optional; enable for extra safety
})
```

### Login (issue tokens + set cookies)
```py
from flask_jwt_extended import create_access_token, create_refresh_token, set_access_cookies, set_refresh_cookies

@app.post('/auth/login')
def login():
    # validate credentials
    access = create_access_token(identity=user.id, additional_claims={"is_admin": user.is_admin})
    refresh = create_refresh_token(identity=user.id)

    # optionally store refresh token (or its hash) server-side for revocation
    save_refresh_token(user.id, refresh)

    resp = jsonify({"msg": "logged in"})
    set_access_cookies(resp, access)
    set_refresh_cookies(resp, refresh)
    return resp, 200
```

### Refresh endpoint
```py
@jwt_required(refresh=True)
def refresh():
    identity = get_jwt_identity()
    new_access = create_access_token(identity=identity)

    # optional: rotate refresh token
    resp = jsonify({"access_token": new_access})
    set_access_cookies(resp, new_access)
    return resp, 200
```

### Logout (clear cookies)
```py
from flask_jwt_extended import unset_jwt_cookies

@app.post('/auth/logout')
def logout():
    resp = jsonify({"msg":"logged out"})
    unset_jwt_cookies(resp)
    # optional: revoke refresh server-side
    return resp, 200
```

---

## Refresh token rotation & revocation (best practice)

### Goals
- Prevent replay of stolen refresh tokens
- Allow server-side invalidation (logout, compromise)

### Simple approach (recommended):
1. **Store refresh token ID (or hash)** in DB on issuance.
2. On `/refresh` verify token against DB record.
3. When issuing a new refresh token, **revoke the old one** (delete DB entry) and store the new one.
4. On logout, delete the DB entry.

This way a refresh token presented twice (replay) is rejected.

---

## Cookie settings (recommended values)
```
Set-Cookie: refresh_token=<token>; HttpOnly; Secure; SameSite=Lax; Path=/; Max-Age=7d
Set-Cookie: access_token=<token>; HttpOnly; Secure; SameSite=Lax; Path=/; Max-Age=15m
```
Notes:
- `Secure` requires HTTPS. Use in production.
- `SameSite=Lax` or `Strict` reduces CSRF. Lax is practical for most SPAs.
- `HttpOnly` prevents JS access.

---

## Frontend patterns (JS)

### Minimal secure pattern (access-in-memory + refresh-cookie)
```js
// in-memory storage
let accessToken = null;

async function login(email, password) {
  const res = await fetch('/auth/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({email, password}),
    credentials: 'include' // important for cookies
  });
  const data = await res.json();
  accessToken = data.access_token || null;
}

async function refreshAccess() {
  const res = await fetch('/auth/refresh', { method: 'POST', credentials: 'include' });
  if (!res.ok) return false;
  const data = await res.json();
  accessToken = data.access_token;
  return true;
}

async function authFetch(url, opts = {}) {
  opts.credentials = 'include';
  opts.headers = opts.headers || {};
  if (accessToken) opts.headers['Authorization'] = `Bearer ${accessToken}`;

  let res = await fetch(url, opts);
  if (res.status !== 401) return res;

  // try refresh once
  const ok = await refreshAccess();
  if (!ok) {
    // not logged in anymore
    throw new Error('Session expired');
  }

  // retry original request
  opts.headers['Authorization'] = `Bearer ${accessToken}`;
  return fetch(url, opts);
}
```

### LocalStorage simpler pattern (less secure)
- Store access & refresh in `localStorage` and attach headers normally.
- Easier but XSS-vulnerable.

---

## CSRF considerations
- If you rely on cookies for refresh, **CSRF is a concern for the refresh endpoint** only.
- Mitigations:
  - `SameSite=Lax` or `Strict` (good default)
  - Require `Content-Type: application/json` and validate `Origin`/`Referer`
  - Enable `JWT_COOKIE_CSRF_PROTECT` in Flask-JWT-Extended and use double-submit tokens

---

## Error handling & UX
- On `401` from API calls, try a single refresh and retry the request. If that fails, redirect to login.
- Do not continuously retry - avoid loops.
- Provide graceful UI messages when session expires.

---

## Deployment & HTTPS
- Always use HTTPS in production to ensure `Secure` cookies are sent.
- Keep `JWT_SECRET_KEY` secret and rotate periodically if needed.

---

## Debugging tips
- Use `jwt.io` to inspect token payloads (exp, iat, claims).
- Watch `Set-Cookie` headers in network tab to ensure HttpOnly/Secure flags.
- Verify `credentials: 'include'` in fetch when cookies are involved.

---

## TL;DR checklist
- [ ] Short-lived access tokens
- [ ] Refresh token in HttpOnly Secure cookie
- [ ] Refresh endpoint reads cookie and issues new access tokens
- [ ] Implement refresh-token rotation + server-side revocation table
- [ ] Use HTTPS and SameSite cookies
- [ ] Frontend keeps access token in memory and calls refresh when needed

---

If you want, I can also generate a downloadable `auth.js` module implementing `login`, `logout`, `authFetch`, `isLoggedIn`, and `refreshAccess` for your project.

