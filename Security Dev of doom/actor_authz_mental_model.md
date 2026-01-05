# Actor-Centric Authentication & Authorization Cheat Sheet

## Core Idea

**Actor ≠ User**

An **Actor** is *anything* that can perform an action in the system:
- Human user (via JWT)
- CLI tool
- Cron job
- Internal service
- External integration

Authentication methods differ, but **authorization rules do not**.

---

## Why Actor Exists

Problems with using `UserEntity` everywhere:
- Cron jobs are not users
- Services are not users
- CLI tools are not users
- Auditing becomes inconsistent
- Services become HTTP-coupled

**Actor solves this by becoming the security boundary.**

---

## Actor Responsibilities

An Actor carries:
- `id` (internal, stable)
- `type` (user | service | system | external)
- `issuer`
- `permissions: Set[str]`
- `audiences: Set[str]`
- `metadata` (optional, non-authoritative)

Actor is:
- Immutable during request lifetime
- Created by the security layer
- Consumed by services

---

## Authentication vs Authorization

### Authentication (AuthN)
> *Who are you?*

Handled in the **security layer**:
- JWT
- API key
- Service token
- CLI credentials
- Internal calls

AuthN produces an **Actor**.

### Authorization (AuthZ)
> *What are you allowed to do?*

Handled in the **service layer**:
- Permission checks
- Audience checks
- Domain invariants

Services **do not care** how authentication happened.

---

## Where Things Live

### Security Layer
- Parse headers / context
- Detect auth method
- Validate credentials
- Build Actor

Example:
```python
actor = get_current_actor(request)
```

### Domain / Service Layer
- Enforce permissions
- Enforce invariants
- Perform auditing

Example:
```python
def create_book(actor: Actor, data):
    ensure_permissions(actor, {"book:create"})
```

---

## Permission Model

- Permissions are **strings**
- Stored as **sets**
- No duplicates
- Order-independent

Example:
```python
required <= actor.permissions
```

This:
- Prevents conflicts
- Avoids O(n) scans
- Makes intent explicit

---

## Roles vs Permissions

- **Roles** are assignment mechanisms
- **Permissions** are enforcement mechanisms

Flow:
```
Roles → Permissions → Actor → Service Check
```

Roles should never be checked directly inside services.

---

## Audience (`aud`) Usage

Audience defines **where a token is valid**.

Rules:
- Audience is validated in security layer
- Audience is granted based on roles
- Services only check permissions

Audience ≠ permission.

---

## CLI / Cron / Internal Services

These:
- Do NOT need JWT login
- Do NOT need user flows
- DO need permissions

Security layer maps:
- CLI context → Actor
- Cron context → Actor
- Internal call → Actor

Same services. Same rules. Same audits.

---

## Auditing Integration

Every state-changing service:
- Receives Actor
- Emits audit event
- Audit is append-only

Audit answers:
- Who (Actor)
- What
- When
- Result

If audit fails → service fails.

---

## Why This Scales

- New auth methods require **zero service changes**
- Services become reusable (HTTP, CLI, jobs)
- Authorization rules are centralized
- Auditing is consistent
- Infra tooling becomes first-class

---

## Common Mistakes

❌ Checking roles inside controllers  
❌ Using UserEntity as security context  
❌ Letting frontend define permissions  
❌ Coupling services to JWT  
❌ Skipping auditing for system actions  

---

## Mental Model

> Authentication is *translation*  
> Authorization is *enforcement*  
> Actor is the *contract* between them

---

## Final Invariant

**If a service has no Actor, it must not mutate state.**
