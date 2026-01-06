# JWT Fields Cheat Sheet
Practical, security-oriented, and aligned with a clean Actor-based auth model.

---

## 1. JWT Refresher

A JWT has **three parts**:

```
header.payload.signature
```

- Header: algorithm + key info
- Payload: claims (this document)
- Signature: integrity + authenticity

JWTs are:
- signed (JWS)
- immutable
- self-contained

---

## 2. Core Identity Claims (Always Relevant)

### `iss` — Issuer
**Who created the token**

Example:
```json
"iss": "https://auth.myapp"
```

Why it matters:
- Prevents accepting tokens from another auth system
- Mandatory in multi-service setups

✅ Strongly recommended

---

### `sub` — Subject
**Who the token represents**

Examples:
```json
"sub": "user_42"
"sub": "service.backup"
```

Used to:
- identify the actor
- link audits
- avoid user lookups

✅ Always required

---

### `exp` — Expiration Time
**When the token becomes invalid**

```json
"exp": 1710000000
```

Rules:
- Unix timestamp (seconds)
- Hard security boundary

❌ Tokens without `exp` are insecure

---

### `iat` — Issued At
**When the token was created**

```json
"iat": 1709995000
```

Used for:
- debugging
- audit timelines
- incident response

✅ Recommended

---

### `jti` — JWT ID
**Unique token identifier**

```json
"jti": "8b5f8a1e-2e91-4d61-9a63-4f82f8c9c3f1"
```

Used for:
- logout
- revocation
- token blacklisting

✅ Optional for demo projects  
✅ Required for real revocation

---

## 3. Authorization Claims (Your Architecture Sweet Spot)

### `aud` — Audience
**Which service(s) may accept this token**

Examples:
```json
"aud": "book-api"
"aud": ["book-api", "blog-api"]
```

Why it exists:
- Prevents token reuse across services
- Enables future service splitting

Security rule:
> API must reject tokens without its audience

---

### `roles`
**High-level authorization grouping**

```json
"roles": ["admin", "moderator"]
```

Rules:
- Roles do NOT grant access directly
- Roles map → permissions (in backend code)

---

### `permissions`
**Fine-grained access rights**

```json
"permissions": [
  "book.read",
  "book.write",
  "audit.view"
]
```

Best practices:
- Treat as a **set**
- Domain-level checks

Example:
```python
if not required_permissions <= actor.permissions:
    raise Forbidden
```

---

### `actor_type` (Custom but Powerful)
**What kind of entity is acting**

Examples:
```json
"actor_type": "user"
"actor_type": "system"
"actor_type": "service"
"actor_type": "external"
```

Used to:
- block invalid flows (`/me` for cron jobs)
- simplify auditing
- support non-user access cleanly

✅ Highly recommended

---

## 4. Optional / Advanced Claims

### `nbf` — Not Before
Token is invalid before a timestamp.

```json
"nbf": 1709996000
```

Rarely needed outside enterprise setups.

---

### `scope`
OAuth-style string permissions.

```json
"scope": "book:read book:write"
```

❌ Redundant if you already use permissions + roles

---

## 5. Claims You Should NOT Put in JWTs

❌ Passwords  
❌ Secrets  
❌ Mutable business data  
❌ Authorization rules  
❌ UI state  

JWTs are **snapshots**, not databases.

---

## 6. Minimal Recommended Payload (Demo Day Safe)

```json
{
  "iss": "auth.myapp",
  "sub": "user_123",
  "exp": 1710000000,
  "iat": 1709995000,
  "aud": ["blog-api"],
  "roles": ["admin"],
  "permissions": ["blog.write"],
  "actor_type": "user"
}
```

---

## 7. Actor Mental Model (Important)

JWT is NOT your domain object.

Flow:
1. Security layer validates JWT
2. Claims are mapped → `Actor`
3. Domain services trust the Actor
4. Authorization happens in-domain

> Services never decode JWTs.

---

## 8. Cleanup & Lifecycle Notes

- JWTs do not need cleanup
- Revocation = optional `jti` blacklist
- Expiration is your primary defense

For demo projects:
- short-lived access token
- refresh token rotation (optional)

---

## One-Line Summary

> JWT = validated identity + immutable authorization snapshot

GG no re.
