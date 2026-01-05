# Auditing System – Mental Model & Invariants

This document captures the auditing architecture, rules, and invariants discussed during design.
It is intended as living documentation for future maintainers and as a reference for implementation.

---

## 1. Purpose of Auditing

Auditing exists to answer three immutable questions:

- Who performed an action?
- What action was performed?
- When did it happen?

Auditing is a failsafe system.
If auditing fails, the business operation must fail.

---

## 2. Core Principles (Invariants)

### Append-Only
- Audit records are never updated
- Audit records are never deleted

### Strong Consistency
- State-changing operations must be audited
- If audit write fails, the request fails

### Separation of Concerns
- Audit database is separate from production database
- Audit DB has stricter access rules

---

## 3. What Must Be Audited

POST, PUT, PATCH, DELETE are always audited.
GET is audited only for sensitive or regulated data.

---

## 4. Actor Model

An Actor represents who or what caused an action.

Actors may be:
- Users
- External services
- Internal services
- Cron jobs
- CLI tools
- System processes

Actor is a domain entity, not a user.

---

## 5. Actor Identity Invariant

Actors are uniquely identified by:

(type, external_id, issuer)

If this tuple already exists, it maps to the same actor.

---

## 6. Database Schema

Actor table:
- id
- type
- external_id
- issuer
- created_at

Unique constraint:
(type, external_id, issuer)

Audit table:
- id
- actor_id (FK)
- action
- resource
- resource_id
- metadata
- created_at

---

## 7. Actor Resolution Flow

1. Lookup actor by identity tuple
2. If exists, reuse actor id
3. If not, create actor
4. Insert audit referencing actor id

---

## 8. Actor Mutability

Identity fields never change.
If identity changes, create a new actor.
Metadata may evolve.

---

## 9. Database Guarantees

- Foreign key constraints
- Unique identity constraint
- NOT NULL on identity fields

---

## 10. SQLite vs PostgreSQL

SQLite is acceptable for development.
PostgreSQL is required for production guarantees.

Design for Postgres, tolerate SQLite.

---

## 11. Failure Semantics

No audit, no commit.
Auditing and business logic must succeed together.

---

## 12. Final Rule

No state change without an audit.
