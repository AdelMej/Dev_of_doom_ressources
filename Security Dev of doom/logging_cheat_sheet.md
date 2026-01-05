# Logging Cheat Sheet (Backend / API Systems)

> **Invariant:** Logs exist to explain *anomalies*, not normal behavior.

---

## What to Log (At a Glance)

### 🔴 ERROR — Always log
Log when something **should not happen** or causes a failure.

**Examples**
- Unhandled exceptions
- Database unavailable
- External service timeout
- Corrupted state detected
- Audit write failure

**Must include**
- trace_id
- actor_id (if known)
- action / operation
- error type
- stack trace

---

### 🟡 WARNING — Log selectively
Log when a **rule or invariant is violated**, but the system continues safely.

**Examples**
- Permission denied (403)
- Rate limit exceeded (429)
- Invalid state transition
- Business rule rejection
- Deprecated API usage

**Purpose**
- Detect abuse
- Detect misconfiguration
- Detect UX or client issues

---

### 🟢 INFO — Log only system boundaries
Log **lifecycle or operational events**, not request flow.

**Allowed**
- Application startup/shutdown
- Configuration loaded
- Migrations applied
- Background job started/completed
- Dependency unavailable/recovered

**Forbidden**
- Request received
- Request succeeded
- Controller entered
- Service finished

---

### ⚫ DEBUG — Local only
Never rely on DEBUG in production.

**Use for**
- Local development
- Temporary investigation
- One-off troubleshooting

---

## What NOT to Log

❌ Successful requests  
❌ Validation success  
❌ Normal control flow  
❌ Every function call  
❌ Sensitive data (tokens, passwords, secrets)  

---

## Logs vs Audit vs Metrics

| Need               | Use             |
| ------------------ | --------------- |
| Who did what, when | Audit           |
| State changes      | Audit           |
| System health      | Metrics         |
| Request volume     | Metrics         |
| Latency            | Metrics         |
| What broke         | Logs            |
| Why it broke       | Logs + Trace ID |

---

## HTTP Status → Logging Level

| Status | Log Level           |
| ------ | ------------------- |
| 2xx    | None                |
| 3xx    | None                |
| 400    | None (client error) |
| 401    | WARNING             |
| 403    | WARNING             |
| 404    | None                |
| 409    | WARNING             |
| 422    | None                |
| 429    | WARNING             |
| 5xx    | ERROR               |

---

## Required Log Fields (Minimal)

```json
{
  "timestamp": "...",
  "level": "ERROR | WARNING | INFO",
  "trace_id": "uuid",
  "actor_id": "optional",
  "action": "string",
  "message": "short summary"
}
```

---

## Trace ID Rules

- Generated at request entry (middleware)
- Propagated through:
  - logs
  - errors
  - audit records
  - downstream calls
- One request = one trace_id

---

## Core Invariants

- Logs explain failures, not success
- No logs on happy paths
- Errors are loud, warnings are signals
- Audit is immutable and separate
- Metrics tell the health story
- Logs answer the 3am question

> **Ask before logging:**  
> *What question will this answer at 3am?*

If the answer is “none” → **don’t log it**.
