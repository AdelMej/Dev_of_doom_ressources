# String Length Cheat Sheet (API / Domain Design)

This document lists **common string length limits**, **why they exist**, and **when to use them**.
These are **industry defaults**, not hard rules.

---

## 🔐 Authentication & User Data

### Username
- **Min:** 3
- **Max:** 30–32
- **Why:** Readability, URLs, logs, UI constraints

Use when:
- Public identifiers
- Login handles
- Mentions / URLs

```text
3–30  → Standard
3–32  → Slightly safer for future-proofing
```

---

### Password (raw input)
- **Min:** 8
- **Recommended Min:** 10–12
- **Max:** 64–128

Why max exists:
- Prevent DoS
- Hashing algorithm limits

```text
8–128 → Very common
```

⚠ Never store raw passwords.

---

### Email
- **Max:** 254 (RFC standard)

Use:
- Login
- Notifications

```text
EmailStr (Pydantic) already enforces this
```

---

### First / Last Name
- **Min:** 1
- **Max:** 50–100

Why:
- International names
- Hyphenated / compound names

```text
1–50  → Common
1–100 → International-safe
```

---

## 👤 Profile / Identity

### Display Name
- **Min:** 1
- **Max:** 50

Use:
- UI labels
- Chat apps

---

### Bio / About
- **Max:** 500–1000

Use:
- Profile descriptions

---

## 📚 Content (Books, Articles, Posts)

### Title
- **Min:** 1
- **Max:** 255

Why:
- SQL VARCHAR(255)
- Index-friendly

---

### Subtitle
- **Max:** 255

---

### Author Name
- **Min:** 1
- **Max:** 255

---

### Description / Summary
- **Max:** 500–2000

Use:
- Product descriptions
- Book summaries

---

### Long Content (Articles, Posts)
- **Max:** 5k–20k
- Usually stored as TEXT / LONGTEXT

```text
Do NOT over-constrain here
```

---

## 📦 Commerce

### Product Name
- **Max:** 255

---

### SKU / Identifier
- **Max:** 32–64

---

### Coupon Code
- **Max:** 16–32

---

## 📞 Contact Information

### Phone Number
- **Max:** 15 (E.164 standard)

```text
+123456789012345
```

---

### Address Line
- **Max:** 255

---

### City
- **Max:** 100

---

### Postal Code
- **Max:** 20

---

## 🔑 Security / Tokens

### JWT Token
- **Max:** 1024–4096

Use:
- Headers
- Cookies

---

### API Key
- **Max:** 64–128

---

## 🧩 Identifiers

### UUID (string form)
- **Length:** 36

```text
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

---

### Slug (URL-safe)
- **Min:** 1
- **Max:** 100

```text
[a-z0-9-]
```

---

## 🧠 Rules of Thumb

- **255** → Default safe string limit
- **32** → Identifiers
- **64–128** → Secrets / tokens
- **1000+** → Free-form text
- DTOs = guardrails
- Domain = truth

---

## ✅ Recommendation

Use DTO limits to:
- Protect API
- Prevent abuse
- Help frontend validation

Enforce **business rules** in the domain.

---

Happy building 🚀
