# 21 — JWT Access + Refresh Token Architecture (FastAPI)

This cheat sheet explains a **clean, production-ready JWT design** using FastAPI,
with proper separation between HTTP, dependencies, and services.

---

## 1. Token Types (Never Use One Token)

### Access Token
- Short-lived (5–15 min)
- Sent on every request
- Stored in memory or Authorization header

### Refresh Token
- Long-lived (days/weeks)
- Used ONLY to get new access tokens
- Stored securely (HTTP-only cookie or DB)

If you only use one token → security smell.

---

## 2. High-Level Flow

1. User logs in
2. Access token issued
3. Refresh token issued
4. Access token expires
5. Client calls `/auth/refresh`
6. New access token returned
7. Refresh token rotated (optional)

---

## 3. Where JWT Logic Lives (Critical)

| Layer | Responsibility |
|----|--------------|
| Router | HTTP input/output |
| Dependency | Token extraction + validation |
| Service | Auth business rules |
| Repository | Token persistence (if any) |

JWT validation ≠ business logic → dependency.

---

## 4. Token Creation (Service Layer)

```python
def create_access_token(user_id: int, scopes: list[str]) -> str:
    payload = {
        "sub": str(user_id),
        "scopes": scopes,
        "exp": datetime.utcnow() + timedelta(minutes=15),
    }
    return jwt.encode(payload, SECRET_KEY, algorithm="HS256")
```

Same idea for refresh token, longer expiration.

---

## 5. Token Extraction Dependency

```python
from fastapi import Depends, HTTPException
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials

security = HTTPBearer()

def get_token(
    credentials: HTTPAuthorizationCredentials = Depends(security),
) -> str:
    return credentials.credentials
```

This is **pure HTTP-layer logic**.

---

## 6. JWT Validation Dependency

```python
def get_current_user(
    token: str = Depends(get_token),
):
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
    except JWTError:
        raise HTTPException(status_code=401, detail="Invalid token")

    return payload["sub"]
```

Stops request BEFORE service is called.

---

## 7. Scope-Based Authorization

```python
def require_scope(scope: str):
    def checker(
        token: str = Depends(get_token),
    ):
        payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
        if scope not in payload.get("scopes", []):
            raise HTTPException(status_code=403, detail="Forbidden")
        return payload
    return checker
```

Usage:
```python
@router.post("/admin")
def admin_action(
    user = Depends(require_scope("admin")),
):
    ...
```

---

## 8. Refresh Token Endpoint

```python
@router.post("/refresh")
def refresh_token(
    refresh_token: str = Cookie(...),
):
    # validate refresh token
    # issue new access token
    return {"access_token": new_token}
```

Refresh token:
- NEVER passed as Authorization header
- NEVER used on normal endpoints

---

## 9. Refresh Token Storage Strategies

### Option A — Stateless
- JWT only
- Simpler
- Harder to revoke

### Option B — DB-backed (recommended)
- Store hash of refresh token
- Allows revocation & rotation
- Required for serious apps

---

## 10. Token Rotation (Recommended)

Every refresh:
- Invalidate old refresh token
- Issue a new one
- Store new hash

Prevents replay attacks.

---

## 11. Logout Implementation

Logout = invalidate refresh token in DB.

Access token:
- Just let it expire

Never try to "delete" access tokens.

---

## 12. Common Mistakes

❌ JWT in middleware  
❌ Long-lived access tokens  
❌ Refresh token in localStorage  
❌ Business logic in dependencies  
❌ One token to rule them all  

---

## 13. Spring Boot Mapping

| FastAPI | Spring Boot |
|------|-------------|
| Dependency | Security Filter |
| HTTPBearer | OncePerRequestFilter |
| require_scope | @PreAuthorize |
| Service | @Service |

Same ideas, explicit wiring.

---

## Final Mental Model

Access token = permission  
Refresh token = renewal  

Dependencies = gatekeepers  
Services = decision makers  

If JWT touches DB → service  
If JWT touches HTTP → dependency  

