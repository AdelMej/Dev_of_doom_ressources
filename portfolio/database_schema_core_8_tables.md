# Core Database Schema (8 Tables)

This document summarizes the **8 SQL tables** required to implement the project in a **clean, spec‑compliant, auditable, and future‑proof** way. No hallucinated features, no over‑engineering.

---

## 1. users
Stores all authenticated users of the system.

**Purpose**
- Central identity table
- Used by admins, coaches, and regular users

**Key fields**
- id (PK)
- email (unique)
- password_hash
- role (ADMIN | COACH | USER)
- created_at
- disabled_at (nullable)

---

## 2. invite_tokens
Handles scoped, secure user registration.

**Purpose**
- Single‑use, time‑limited invite links
- Avoids public signup
- Avoids manual admin account creation

**Key fields**
- id (PK)
- token (unique)
- role_to_assign
- expires_at
- consumed_at (nullable)
- created_by_admin_id (FK → users.id)

---

## 3. sessions
Represents coach sessions that users can subscribe to.

**Purpose**
- Core business object
- Drives subscriptions, payments, and credits

**Key fields**
- id (PK)
- coach_id (FK → users.id)
- title
- scheduled_at
- price
- status (PLANNED | CANCELLED | COMPLETED)

---

## 4. subscriptions
Links users to sessions.

**Purpose**
- Tracks who is subscribed to what
- Allows notifications and auditing

**Key fields**
- id (PK)
- user_id (FK → users.id)
- session_id (FK → sessions.id)
- subscribed_at
- cancelled_at (nullable)

---

## 5. payment_policies
Defines how much the company covers for sessions.

**Purpose**
- Non‑fixed pricing support
- Auditable business rules
- Changeable without breaking history

**Key fields**
- id (PK)
- company_share_percent (nullable)
- company_share_amount (nullable)
- active_from
- active_until (nullable)

---

## 6. payments
Stores actual user payments.

**Purpose**
- Records what the **user actually paid**
- Decoupled from company accounting

**Key fields**
- id (PK)
- user_id (FK → users.id)
- session_id (FK → sessions.id)
- policy_id (FK → payment_policies.id, nullable)
- amount_paid
- paid_at

---

## 7. credits
Tracks refundable user value.

**Purpose**
- Replaces refunds with credits
- Fully auditable
- Supports cancellation flows

**Key fields**
- id (PK)
- user_id (FK → users.id)
- source_payment_id (FK → payments.id)
- amount
- created_at
- consumed_at (nullable)

---

## 8. notifications
Tracks important system events.

**Purpose**
- Session cancellation alerts
- Transparency for users

**Key fields**
- id (PK)
- user_id (FK → users.id)
- type
- payload (JSON)
- sent_at

---

## Summary

- **8 tables total**
- Fully auditable
- No speculative features
- Spec‑compliant
- Industry‑grade architecture

This schema supports UML, API design, backend implementation, and future scaling without refactors.

