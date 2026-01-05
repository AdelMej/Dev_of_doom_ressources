# Auditing Cheat Sheet (Backend Mental Model)

This document defines **what to audit, why to audit it, and how to design auditing systems** without bloating your architecture.
Auditing is **not logging**. Auditing is a **security and accountability invariant**.

---

## 1. What Auditing Is (Invariant)

Auditing answers **exactly three questions**:

- **Who** did something
- **What** happened
- **When** it happened

If an action **changes system state**, it **must be auditable**.

If you cannot answer these three questions **immutably**, the system is broken.

---

## 2. Audit vs Log vs Metrics

| Concern | Purpose                   | Mutable | Performance Critical |
| ------- | ------------------------- | ------- | -------------------- |
| Logs    | Debugging & ops           | Yes     | Medium               |
| Audits  | Accountability & security | **No**  | **No**               |
| Metrics | Monitoring & trends       | Yes     | Yes                  |

**Never mix audits with logs.**

---

## 3. What MUST Be Audited

### Always audit:
- `POST` (create)
- `PUT` (replace)
- `PATCH` (modify)
- `DELETE` (remove)
- Permission changes
- Role grants / revocations
- Auth-related state changes
- Configuration changes
- Admin / system actions

### Usually NOT audited:
- `GET` requests (unless data is highly sensitive)
- Health checks
- Static asset access

---

## 4. Audit Failure Policy (Hard Rule)

> **If auditing fails, the business action must fail.**

No audit = no mutation.

This makes auditing a **fail-safe**, not a best-effort feature.

---

## 5. Actor Model (Critical Concept)

An **Actor** is **not always a User**.

Actors can be:
- Human users
- External services
- Cron jobs
- System processes
- CLI tools

### Actor Identity Fields (Stable):
- `issuer` (who issued the identity)
- `external_id` (stable ID from issuer)
- `type` (human, service, system, job)

Actors are **deduplicated** by `(issuer, external_id)`.

---

## 6. Actor Table Invariants

Actors:
- Are **created lazily**
- Are **reused across audits**
- May change metadata (name, role labels)
- **Must never be deleted**

Actors exist **only because they acted**.

---

## 7. Audit Table Invariants

Audit records:
- Are **append-only**
- Are **immutable**
- Cannot be updated or deleted
- Must reference a valid actor
- Must include timestamp

If an audit row changes, the system is compromised.

---

## 8. Minimal Audit Record Schema

Required fields:

- `id`
- `actor_id`
- `action`
- `resource_type`
- `resource_id`
- `timestamp`

Optional but useful:
- `before_state` (optional snapshot)
- `after_state` (optional snapshot)
- `request_id / trace_id`
- `source_ip`
- `reason`

---

## 9. Actor Resolution Flow (Invariant)

1. Receive request
2. Resolve actor identity (token, system, job)
3. Lookup actor by `(issuer, external_id)`
4. If not found → create actor
5. Write audit entry with internal `actor_id`
6. Commit transaction

**Audit and business mutation should share a transaction boundary**.

---

## 10. Database Rules

### Recommended:
- Separate **audit database**
- Strict permissions (write-only from app)
- No cascade deletes
- Strong foreign keys

### PostgreSQL:
- Supports unique constraints on `(issuer, external_id)`
- Supports immutability via permissions & triggers

### SQLite (dev only):
- Acceptable for local dev
- Do NOT rely on it for enforcing all invariants

---

## 11. Why Auditing Is Non-Negotiable

Audits are your:
- Legal safety net
- Security forensic tool
- Post-incident truth source
- Internal accountability system

Logs can lie.
Audits must not.

---

## 12. One-Line Rule

> **Logs explain what broke. Audits prove what happened.**

---

End of document.
