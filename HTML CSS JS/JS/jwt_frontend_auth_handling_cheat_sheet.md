# JWT Tokens — Frontend & Minimal Backend Handling (Cheat Sheet)

> Full, practical guide for handling JWTs from the browser. Includes login flow, storage options, sending tokens, refresh flow, logout, expiry checks, error handling, and security notes. Drop into your frontend project or keep as a sticky reference.

---

## Quick summary

- **Access token**: short-lived JWT used to authenticate API requests (`Authorization: Bearer <token>`).
- **Refresh token**: long-lived secret used to get new access tokens. Should be stored more securely (ideally httpOnly cookie).
- **Best practice**: store access token in memory or in a JS-accessible store for single-page apps. Store refresh token in an `httpOnly`, `Secure`, `SameSite` cookie if possible.

---

## 1. Minimal frontend flow (no refresh tokens)

### Login (receive access token)
```js
async function login(email, password) {
  const res = await fetch('/api/v1/auth/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ email, password })
  });

  const data = await res.json();
  if (!res.ok) throw new Error(data.error || 'Login failed');

  // store token (see storage options below)
  localStorage.setItem('access_token', data.access_token);
}
```

### API request wrapper (attach token)
```js
function apiRequest(url, { method = 'GET', body, headers = {} } = {}) {
  const token = localStorage.getItem('access_token');
  if (!token) return Promise.reject(new Error('No token found'));

  const hdr = {
    ...headers,
    'Authorization': `Bearer ${token}`,
    'Content-Type': 'application/json'
  };

  return fetch(url, {
    method,
    headers: hdr,
    body: body ? JSON.stringify(body) : undefined
  });
}
```

### Logout
```js
function logout() {
  localStorage.removeItem('access_token');
  // optionally redirect
  window.location.href = '/login.html';
}
```

---

## 2. Token presence & expiry checks

### `!!` trick
```js
function hasToken() {
  return !!localStorage.getItem('access_token');
}
```

### Check expiry (if JWT `exp` present)
```js
function isTokenExpired(token) {
  try {
    const payload = JSON.parse(atob(token.split('.')[1]));
    return Date.now() >= payload.exp * 1000;
  } catch (e) {
    return true; // malformed or missing => treat as expired
  }
}

function isLoggedIn() {
  const token = localStorage.getItem('access_token');
  return token && !isTokenExpired(token);
}
```

**Note:** client-side expiry check is UX convenience only — server must always validate tokens.

---

## 3. Refresh tokens (recommended for real apps)

### Two main storage models

**A — Refresh token in httpOnly cookie (recommended)**
- Backend sets a cookie `Set-Cookie: refresh=<token>; HttpOnly; Secure; SameSite=Strict` on login.
- Access token is returned in response body (or set in another cookie).
- Frontend does **not** access refresh token directly; it calls `/auth/refresh` (server reads cookie and issues new access token).

**B — Refresh token in localStorage (simpler but less secure)**
- Store refresh token in `localStorage` and call `/auth/refresh` sending it in body.
- **Risk:** XSS can steal refresh tokens.

---

### Minimal Refresh flow (httpOnly cookie model)

1. `POST /auth/login` → server returns access token and sets httpOnly cookie `refresh`.
2. Frontend stores access token in memory/localStorage and uses it.
3. When access token is expired (or 401 received), call `POST /auth/refresh` (no body needed; cookie auto-sent).
4. Server validates refresh cookie, issues new access token (and optionally rotates refresh cookie).
5. Frontend replaces stored access token and retries the original request.

Example refresh call:
```js
async function refreshAccess() {
  const res = await fetch('/api/v1/auth/refresh', {
    method: 'POST',
    credentials: 'include' // IMPORTANT for cookies
  });
  const data = await res.json();
  if (!res.ok) throw new Error(data.error || 'Refresh failed');
  localStorage.setItem('access_token', data.access_token);
}
```

**Important:** call `fetch(..., { credentials: 'include' })` when cookies are used.

---

## 4. Automatic retry / refresh wrapper (frontend)

This wrapper will attempt a request, and if it sees a `401`, try to refresh and replay once.

```js
async function authFetch(url, opts = {}) {
  const token = localStorage.getItem('access_token');

  const headers = {
    ...(opts.headers || {}),
    'Content-Type': 'application/json',
    ...(token ? { 'Authorization': `Bearer ${token}` } : {})
  };

  let res = await fetch(url, { ...opts, headers, credentials: 'include' });

  if (res.status !== 401) return res;

  // try refresh once
  try {
    await refreshAccess(); // refresh sets new access token in storage
  } catch (err) {
    // refresh failed — force logout
    logout();
    throw err;
  }

  // retry original request with new token
  const newToken = localStorage.getItem('access_token');
  const retryHeaders = {
    ...(opts.headers || {}),
    'Content-Type': 'application/json',
    ...(newToken ? { 'Authorization': `Bearer ${newToken}` } : {})
  };

  return fetch(url, { ...opts, headers: retryHeaders, credentials: 'include' });
}
```

**Notes:**
- `credentials: 'include'` is needed if refresh token is in cookie.
- Keep retry attempts limited (1 retry) to avoid loops.

---

## 5. Backend considerations (minimal, important)

- **Always validate JWT signature** (library does it). Check `exp`, `iat`, `iss`, `aud` as appropriate.
- **Short-lived access tokens** (e.g. 5–15 mins). Long refresh tokens (days/weeks) if you use them.
- **Rotate refresh tokens** whenever used. Store refresh tokens (or their hashes) server-side for revocation.
- **On logout**, invalidate refresh token (delete DB record / set cookie expiration).
- **Don’t put secrets or large payloads inside JWTs**. Keep small claims only (sub/user_id, is_admin).
- **Use https** in production — never send tokens over plain HTTP.

### Minimal secure cookie flags
```
Set-Cookie: refresh=<token>; HttpOnly; Secure; SameSite=Strict; Path=/api/v1/auth/; Max-Age=604800
```

---

## 6. Where to store tokens (pros/cons)

- **httpOnly cookie (refresh token)**
  - Pros: protected from XSS
  - Cons: requires `credentials: 'include'`, CSRF considerations

- **localStorage (access token)**
  - Pros: easy, works with SPA
  - Cons: XSS can read it

- **in-memory (access token)**
  - Pros: safest from XSS, cleared on refresh/page close
  - Cons: lost after reload unless refreshed; must re-login or rely on refresh cookie

**Real-world combo:** access token in memory (or short-lived in localStorage), refresh token in httpOnly cookie.

---

## 7. CSRF and cookies

If you store refresh token in cookie, **protect the refresh endpoint** from CSRF:
- Use `SameSite=Strict` or `Lax` (prevents most CSRF) *and* require `Content-Type: application/json` and validate `Origin`/`Referer` if you want extra protection.
- Or use double-submit cookie / anti-CSRF token if necessary.

For typical modern SPAs, `SameSite=Lax` or `Strict` plus the endpoint being a `POST` with `Content-Type: application/json` is usually sufficient.

---

## 8. Example flows (short)

**Login**
- POST `/auth/login` with email+password
- server: validate credentials → issue access token + set refresh cookie
- client: store access token (localStorage/in-memory)

**Request protected resource**
- client: call `authFetch('/protected')` → `Authorization: Bearer <access>` header included
- server: validates header and returns resource

**Token expired during request**
- client receives 401 → calls `/auth/refresh` (cookie auto-sent) → server issues new access token and (optionally) rotates refresh cookie → client retries original request

**Logout**
- client: call `/auth/logout` → server invalidates refresh token and clears cookie → client clears local access token

---

## 9. Debugging tips
- Use `jwt.io` to inspect token payloads (exp, sub, claims).
- Watch network tab to verify `Authorization` headers and cookie `Set-Cookie` behavior.
- Confirm `credentials: 'include'` when using cookies.
- Log server-side when refresh happens and when tokens are rejected to find mismatches.

---

## 10. Minimal example backend pseudo-code (Flask-like)

```python
# login
@app.route('/auth/login', methods=['POST'])
def login():
    user = check_credentials()
    access = create_access_token(identity=user.id, additional_claims={...})
    refresh = create_refresh_token(identity=user.id)

    # Save hashed refresh in DB for revocation
    save_refresh_token(user.id, refresh)

    response = jsonify({'access_token': access})
    response.set_cookie('refresh', refresh, httponly=True, secure=True, samesite='Strict')
    return response

# refresh
@app.route('/auth/refresh', methods=['POST'])
def refresh():
    # read cookie
    refresh = request.cookies.get('refresh')
    if not validate_refresh(refresh):
        return jsonify({'error': 'invalid refresh'}), 401

    new_access = create_access_token(identity=get_user_from_refresh(refresh))
    # (optional) rotate refresh token and set cookie again
    return jsonify({'access_token': new_access})

# logout
@app.route('/auth/logout', methods=['POST'])
def logout():
    refresh = request.cookies.get('refresh')
    revoke_refresh_in_db(refresh)
    response = jsonify({'message': 'ok'})
    response.set_cookie('refresh', '', expires=0)
    return response
```

---

## 11. TL;DR checklist (copy-paste)

- [ ] Use `Authorization: Bearer <token>` header for protected endpoints
- [ ] Store access token short-lived; store refresh token in httpOnly cookie
- [ ] Use `credentials: 'include'` in fetch when cookies used
- [ ] Always validate signatures and `exp` on server
- [ ] Rotate and revoke refresh tokens server-side
- [ ] Use HTTPS
- [ ] Limit retries (1 refresh attempt per failing request)
- [ ] Protect refresh endpoint from CSRF (SameSite, Origin checks)

---

## Notes
This cheat sheet is intentionally practical and minimal. For a production system, treat tokens, secrets, and session logic as sensitive infrastructure and add logging/monitoring, rate-limiting, device/session tables, and strict audit trails.

---

Good luck — if you want a downloadable JS file (`auth.js`) that implements `login`, `logout`, `authFetch`, `isLoggedIn`, and refresh handling, I can generate it for you.

