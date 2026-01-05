# python-jose (JOSE) Cheat Sheet

This cheat sheet focuses on **JWT usage with python-jose**, independent of any specific web framework.

---

## Installation

```bash
pip install python-jose[cryptography]
```

---

## Core Concepts

- **JWT**: JSON Web Token
- **JWS**: Signed token (most common)
- **JWE**: Encrypted token (rare for APIs)
- **Claims**: Data inside the token

JWT structure:
```
header.payload.signature
```

---

## Common Algorithms

| Algorithm | Type | Notes |
|---------|------|------|
| HS256 | HMAC | Symmetric (shared secret) |
| RS256 | RSA | Asymmetric (private/public key) |
| ES256 | ECDSA | Asymmetric, smaller keys |

Most APIs start with **HS256**.

---

## Creating a JWT

```python
from jose import jwt
from datetime import datetime, timedelta, timezone

SECRET_KEY = "super-secret"
ALGORITHM = "HS256"

def create_access_token(subject: str, expires_minutes: int = 15) -> str:
    expire = datetime.now(tz=timezone.utc) + timedelta(minutes=expires_minutes)

    payload = {
        "sub": subject,
        "exp": expire,
        "iat": datetime.now(tz=timezone.utc),
        "type": "access",
    }

    return jwt.encode(payload, SECRET_KEY, algorithm=ALGORITHM)
```

### Recommended Claims

| Claim | Meaning |
|------|--------|
| sub | Subject (user id / identifier) |
| exp | Expiration time |
| iat | Issued at |
| iss | Issuer |
| aud | Audience |

---

## Decoding & Validating a JWT

```python
from jose import jwt, JWTError

def decode_token(token: str) -> dict:
    try:
        payload = jwt.decode(
            token,
            SECRET_KEY,
            algorithms=[ALGORITHM],
        )
        return payload
    except JWTError:
        raise ValueError("Invalid or expired token")
```

What `jwt.decode()` validates automatically:
- Signature
- Algorithm
- Expiration (`exp`)
- Token format

---

## Extracting the Subject

```python
def get_subject(token: str) -> str:
    payload = decode_token(token)
    subject = payload.get("sub")
    if not subject:
        raise ValueError("Missing subject")
    return subject
```

---

## Token Expiration

```python
from datetime import datetime, timezone

exp = payload["exp"]
if datetime.fromtimestamp(exp, tz=timezone.utc) < datetime.now(tz=timezone.utc):
    raise ValueError("Token expired")
```

> ⚠️ python-jose already checks `exp`, manual checks are optional.

---

## Refresh Tokens (Concept)

- Access token: short-lived (minutes)
- Refresh token: long-lived (days)

```python
payload["type"] = "refresh"
```

Always validate the `type` claim when using refresh tokens.

---

## Asymmetric (RS256) Example

```python
PRIVATE_KEY = open("private.pem").read()
PUBLIC_KEY = open("public.pem").read()

token = jwt.encode(payload, PRIVATE_KEY, algorithm="RS256")

payload = jwt.decode(token, PUBLIC_KEY, algorithms=["RS256"])
```

---

## Common Errors

| Error | Cause |
|------|------|
| JWTError | Invalid token |
| ExpiredSignatureError | Token expired |
| JWKError | Key mismatch |
| JWTClaimsError | Invalid claims |

---

## Security Best Practices

- Never store sensitive data in JWT payload
- Keep tokens small
- Always set `exp`
- Rotate secrets in production
- Prefer RS256 for large systems
- Use HTTPS only

---

## What NOT to Do

❌ Put passwords in JWT  
❌ Trust client-side decoded tokens  
❌ Use long-lived access tokens  
❌ Mix JWT logic with domain logic  

---

## Mental Model

- JWT = **transport credential**
- Not a session
- Not a user
- Not domain logic

---

## Minimal Production Checklist

✔ SECRET_KEY loaded from env  
✔ Algorithm explicitly defined  
✔ Expiration enforced  
✔ Validation centralized  
✔ Infra-only logic  

---

## References

- RFC 7519 (JWT)
- python-jose documentation
