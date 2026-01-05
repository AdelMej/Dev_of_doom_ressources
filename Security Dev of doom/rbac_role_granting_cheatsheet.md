# RBAC Role Granting Cheat Sheet

This cheat sheet documents a **simple, invariant-based RBAC system** using JWTs.
It is backend-agnostic and works especially well with FastAPI, Spring Boot, or any API-first architecture.

---

## Core Principles

- **Roles are static**
- **Permissions are additive**
- **Authorization is issued, not computed**
- **JWT is a transport, not a policy engine**
- **Rules live in code (invariants), not in the database**

---

## Terminology

- **Role**: A named responsibility (e.g. `USER`, `ADMIN`)
- **Permission**: A capability (e.g. `books:create`, `users:read`)
- **Audience (`aud`)**: Which API(s) the token is meant for
- **Issuer (`iss`)**: Who issued the token (auth service)

---

## Role → Permission Mapping (Invariant)

```python
ROLE_PERMISSIONS = {
    "USER": {
        "books:read",
        "books:borrow",
    },
    "ADMIN": {
        "books:create",
        "books:update",
        "books:delete",
        "users:read",
        "users:grant-role",
    },
}
```

Rules:
- No conditionals
- No DB calls
- No runtime logic

---

## Token Issuance Flow

1. User authenticates (login)
2. User roles are loaded
3. Permissions are **derived from roles**
4. JWT is issued with:
   - `sub` (user id)
   - `roles`
   - `permissions`
   - `aud`
   - `iss`
   - `exp`

Example payload:

```json
{
  "sub": "user-id",
  "roles": ["USER", "ADMIN"],
  "permissions": [
    "books:read",
    "books:create",
    "users:read"
  ],
  "aud": "books-api",
  "iss": "auth-api",
  "exp": 1735689600
}
```

---

## Why Permissions in JWT?

- No DB lookup per request
- APIs stay stateless
- Horizontal scaling is trivial
- Authorization is deterministic

---

## API-Side Authorization

Each route declares **required permission**.

Example (FastAPI):

```python
def require_permission(permission: str):
    def checker(user: AuthenticatedUser = Depends(get_current_user)):
        if permission not in user.permissions:
            raise ForbiddenError()
        return user
    return checker
```

Usage:

```python
@router.post(
    "/admin/books",
    dependencies=[Depends(require_permission("books:create"))]
)
async def create_book():
    ...
```

---

## Why This Scales

- Adding a role = mapping change
- Adding a permission = constant change
- No migration
- No policy tables
- No coupling between services

---

## When to Add `aud` and `iss`

### `iss`
Use when:
- Tokens are issued by a central auth service
- Multiple APIs validate the same token

### `aud`
Use when:
- Multiple APIs consume tokens
- You want strict boundary enforcement

Example:
- `aud = books-api`
- `aud = admin-api`

---

## What This Is NOT

- ❌ ABAC
- ❌ Policy engines
- ❌ Dynamic permissions
- ❌ Runtime authorization logic

---

## Mental Model

> **Auth answers: who are you?**  
> **Roles answer: what are you?**  
> **Permissions answer: what can you do?**

---

## Final Rule

If authorization logic requires:
- conditionals
- DB calls
- context objects

You are doing too much.

---

**Grant roles.  
Issue permissions.  
Validate invariants.  
GG no re.**
