# PKCE (Proof Key for Code Exchange) — Cheat Sheet

## What PKCE Is
PKCE is an OAuth 2.0 extension designed to protect **public clients** (SPAs, mobile apps, desktop apps)
from **authorization code interception attacks**.

It removes the need for a client secret.

---

## When You NEED PKCE
Use PKCE if your client is:
- A browser SPA
- A mobile app
- A desktop app
- Any client where secrets **cannot be kept confidential**

PKCE is **mandatory** for OAuth 2.1 and modern OAuth providers.

---

## High-Level Flow

```
Client ──(1)──► Auth Server
Client ◄─(2)── Authorization Code
Client ──(3)──► Token Endpoint (+ code_verifier)
Client ◄─(4)── Access Token / ID Token
```

---

## Step-by-Step Flow

### 1. Generate Code Verifier
- High-entropy random string
- 43–128 characters
- URL-safe

Example:
```
dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```

---

### 2. Generate Code Challenge
Two methods:
- `S256` (REQUIRED)
- `plain` (DO NOT USE)

#### S256
```
code_challenge = BASE64URL(SHA256(code_verifier))
```

---

### 3. Authorization Request
```
GET /authorize?
  response_type=code
  &client_id=client_id
  &redirect_uri=...
  &code_challenge=...
  &code_challenge_method=S256
  &scope=...
```

Auth server stores:
- `code_challenge`
- `challenge_method`

---

### 4. Token Request
```
POST /token
  grant_type=authorization_code
  code=...
  redirect_uri=...
  client_id=...
  code_verifier=...
```

Auth server:
- hashes `code_verifier`
- compares to stored `code_challenge`
- issues tokens if match

---

## Security Guarantees

PKCE ensures:
- Authorization code is useless if intercepted
- Tokens can only be exchanged by the original client
- No reliance on client secrets

---

## What PKCE Does NOT Do
PKCE does NOT:
- Authenticate the user
- Authorize API access
- Replace JWT validation
- Replace HTTPS (HTTPS is mandatory)

---

## PKCE vs Client Secret

| Feature | Client Secret | PKCE |
|------|--------------|------|
| Public clients | ❌ | ✅ |
| Secret storage | Required | Not needed |
| Interception-safe | ❌ | ✅ |
| OAuth 2.1 compliant | ❌ | ✅ |

---

## PKCE + JWT
PKCE is used:
- **before** JWT exists
- during the authorization code exchange

JWT validation still requires:
- `iss`
- `aud`
- signature verification
- expiration checks

---

## Common Mistakes

❌ Using `plain` method  
❌ Reusing code_verifier  
❌ Storing code_verifier in localStorage long-term  
❌ Skipping HTTPS  
❌ Thinking PKCE replaces RBAC

---

## Minimal Mental Model

```
PKCE = "prove you started the login flow"
JWT  = "prove who you are & what you can access"
RBAC = "decide if you are allowed"
```

---

## TL;DR
- PKCE protects the auth code
- JWT protects identity & permissions
- RBAC protects resources
- Use all three
