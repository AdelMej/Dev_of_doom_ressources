# UGA Authentication Architecture

This document describes the **UGA-based authentication system**: a deliberately minimal, dumb, and therefore secure architecture focused on simplicity, predictability, and low attack surface.

---

## 1. Core Principles

### **1. UGA Rule #1 — Track Nothing You Don’t Need**
Only store:
- `user_id`
- `device_id` (UUID)
- `refresh_token`
- `expires_at`

No browser info, no IP, no OS, no metadata.

---

## 2. Tokens

### **Access Token (JWT)**
- Lifetime: **5 minutes**
- Contains minimal claims:
  - `sub`: user_id
  - `exp`: expiration
  - `iat`: issuance
- Signed with the currently active key.
- Self-contained, stateless, used only by backend services.

### **Refresh Token**
- Stored as HTTP-only, SameSite cookies.
- Bound to a `device_id`.
- Rotated on every refresh.
- Stored server-side in the `device_sessions` table.

---

## 3. Device Session Table

```
device_id | user_id | refresh_token | expires_at
```

### Behavior
- Every refresh **creates a new row** and deletes the old one.
- No updates, ever.
- Access token never stored.
- Device info never stored.

---

## 4. Key Rotation (JWT Signing Keys)

### Structure
```
kid | key | active
```

### Process
1. Generate a new key.
2. Mark new key as `active = true`.
3. Old key remains for 5 minutes to let existing JWTs expire.
4. Cron deletes inactive keys older than 10 minutes.

Minimal, predictable, UGA.

---

## 5. How Authentication Works

### **Step 1 — Login**
1. User logs in.
2. Auth API:
   - Creates `device_id` (UUID).
   - Creates refresh token.
   - Stores refresh token + device row.
   - Sends refresh token in HTTP-only cookie.
   - Returns 5-minute JWT to frontend.

---

### **Step 2 — Using a Service**
1. Frontend sends request to a service **with JWT in Authorization header**.
2. Service validates JWT using current JWK.
3. No cookies sent to service.
4. Service never interacts with refresh tokens.

---

### **Step 3 — Refreshing the JWT**
1. Frontend calls `/refresh` on the Auth API.
2. Browser automatically sends cookies.
3. Auth API:
   - Finds matching device by UUID+refresh.
   - Deletes the old row.
   - Creates new refresh token row.
   - Sets new refresh HTTP-only cookie.
   - Issues new JWT.

Frontend never touches cookies.
Services never touch cookies.

---

## 6. Cleanup Cron
One cron job handles everything:
- Delete expired refresh tokens.
- Delete keys older than ~10 minutes.
- Delete device sessions inactive for 3 months.

Simple, one place to maintain.

---

## 7. Benefits of UGA Architecture
- Minimal attack surface.
- No complex state.
- No update queries.
- No fingerprinting.
- No overthinking.
- Works across any number of microservices.
- Frontend only handles JWT.
- Security is derived from simplicity.

---

If you want, I can add:
- diagrams
- sequence flows
- code examples (Node, Python, Go—your pick)
- the DB schema in SQL syntax

