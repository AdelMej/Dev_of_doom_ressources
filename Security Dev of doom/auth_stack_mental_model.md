# Auth Stack Mental Model – From Login to Authorization

This cheat sheet explains **how all auth pieces fit together** without framework BS.

---

## 1. The Core Problem Auth Solves

Auth answers **three different questions**:

1. **Who are you?** → Authentication
2. **What can you do?** → Authorization
3. **Can I trust this token?** → Verification

Each layer solves ONE problem. Mixing them causes pain.

---

## 2. High-Level Stack Overview

```
[ User ]
   ↓
[ Frontend (SPA) ]
   ↓  (PKCE)
[ Auth API ]
   ↓  (JWT)
[ Protected APIs ]
```

---

## 3. Authentication Layer (Login)

### Goal
Prove the user is who they claim to be.

### Happens in
**Auth API only**

### Inputs
- Username / password
- OAuth provider
- MFA, etc.

### Output
- Refresh token (HTTP-only cookie)
- Access token (JWT)

❗ Authentication **never checks permissions**.

---

## 4. PKCE (Client Security Layer)

### Problem PKCE Solves
SPAs cannot store secrets securely.

### What PKCE Protects Against
- Authorization code interception
- Token theft

### Who Uses PKCE
- Browser apps
- Mobile apps

### Mental Model
> PKCE proves the client that started login is the same one finishing it.

PKCE ≠ Authorization  
PKCE ≠ Identity  
PKCE = **Client integrity**

---

## 5. Token Layer (JWT)

### What a JWT Is
A **signed claim container**, not a session.

### JWT Guarantees
- Integrity (not modified)
- Authenticity (issuer)

### JWT Does NOT Guarantee
- Permissions correctness
- Freshness (unless exp checked)

---

## 6. JWT Core Claims

| Claim | Purpose |
|------|--------|
| iss | Who issued the token |
| sub | User ID |
| exp | Expiration |
| aud | Who the token is for |
| roles / perms | Authorization data |

---

## 7. Issuer (`iss`)

### What It Means
> Who created and signed this token?

### Usage
- Every API checks `iss`
- Must match **Auth API identifier**

### Rule
❗ If `iss` is wrong → reject immediately

---

## 8. Audience (`aud`)

### What It Means
> Which service(s) is this token valid for?

### Why It Exists
Prevents token reuse across services.

### Mental Rule
> A service only accepts tokens meant for it.

### Common Patterns
- Single API → optional
- Multiple internal APIs → important
- Third-party APIs → mandatory

---

## 9. Authorization Layer (RBAC)

### Goal
Decide **what the user can access**.

### Happens In
- Auth API (token issuance)
- Protected APIs (enforcement)

### RBAC Model
```
User → Roles → Permissions → APIs
```

---

## 10. Role → Permission Mapping (Invariant)

Roles map to permissions **in code**, not DB tables.

Example:
```
ADMIN → [admin:read, admin:write]
USER  → [user:read]
```

Benefits:
- Predictable
- Testable
- No policy explosion

---

## 11. JWT as Authorization Carrier

JWT contains:
- roles
- permissions
- audience

Protected API checks:
1. Signature
2. Expiration
3. Issuer
4. Audience
5. Permission required for route

---

## 12. Route Protection Mental Model

```
Incoming request
  ↓
Verify JWT signature
  ↓
Check exp / iss / aud
  ↓
Check required permission
  ↓
Allow or deny
```

Each step fails fast.

---

## 13. Refresh Tokens

### Purpose
Get new access tokens without re-login.

### Stored As
- HTTP-only cookie

### Flow
```
Access token expired
  ↓
Call /refresh
  ↓
New access token issued
```

Frontend never touches refresh token.

---

## 14. SPA vs MPA Impact

### SPA
- JS memory persists
- Tokens cached in memory
- Fewer refresh calls

### MPA
- Page reload wipes memory
- Frequent /refresh calls
- Cookies required

SPA = smoother auth UX

---

## 15. When Things Go Wrong

| Problem | Cause |
|------|------|
| Token rejected | Wrong iss / aud |
| Infinite refresh | Bad refresh logic |
| Over-permission | Bad role design |
| Token abuse | Missing aud checks |

---

## 16. Golden Rules

- Auth API authenticates, not authorizes routes
- JWTs carry claims, APIs enforce them
- RBAC > ABAC for sanity
- aud defines **service boundaries**
- iss defines **trust boundary**

---

## 17. One-Sentence Mental Model

> PKCE secures the client, JWT secures the data, RBAC secures the actions.

---

## 18. Minimal Stack (Recommended)

- SPA + PKCE
- Central Auth API
- JWT with roles + aud
- RBAC in code
- HTTP-only refresh tokens

Clean. Scalable. Testable.

---

## 19. Final Thought

If auth feels complex:
You are probably mixing **authentication**, **authorization**, and **transport security**.

Separate them — everything clicks.

