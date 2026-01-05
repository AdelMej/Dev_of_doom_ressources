# 🔐 zxcvbn Cheat Sheet

## What zxcvbn is
**zxcvbn** is a password strength estimator that:
- detects common patterns
- estimates guessability
- outputs a strength score
- provides crack-time estimates

It is **not** a validator by itself — *you decide the policy*.

---

## What zxcvbn analyzes

### 🔎 Pattern detection
zxcvbn detects and penalizes:
- dictionary words ("password", "dragon")
- names and common words
- keyboard walks ("qwerty", "asdf")
- sequences ("abcd", "1234")
- dates ("2024", "01/01/2024")
- repeated characters ("aaaaaa")
- leetspeak ("p@ssw0rd")
- common substitutions

---

### 🧠 Guessability / entropy estimation
Instead of naive entropy math, zxcvbn estimates:
- number of guesses required
- time to crack (online / offline scenarios)

This models **real-world attacks**, not theoretical randomness.

---

## Output fields (important ones)

Typical output includes:
- `score` → integer from **0 to 4**
- `guesses`
- `guesses_log10`
- `crack_times_display`
- `feedback`
- `sequence` (detected patterns)

---

## Score meaning

| Score | Meaning     | Recommendation |
| ----- | ----------- | -------------- |
| 0     | very weak   | reject         |
| 1     | weak        | reject         |
| 2     | mediocre    | usually reject |
| 3     | strong      | accept         |
| 4     | very strong | accept         |

**Most systems require `score >= 3`.**

---

## Recommended baseline policy

Accept a password if:
- length ≥ 12
- `zxcvbn.score >= 3`
- not reused (optional)
- not breached (optional)

Keep it boring and enforce it in the **service layer**.

---

## Where zxcvbn belongs

- ❌ DTO layer
- ❌ client-only validation
- ✅ service / domain layer

Never rely on client-side checks for security.

---

## What NOT to do

- ❌ don't replace it with regex-only rules
- ❌ don't invent your own entropy formula
- ❌ don't force uppercase / symbol rules
- ❌ don't rely on score without length checks

---

## Typical service-layer flow (pseudo)

```
validateLength(password)

result = zxcvbn(password)
if result.score < 3:
    reject(result.feedback)

checkPasswordReuse(password)
hashAndStore(password)
```

---

## Custom dictionaries (optional)
You can provide context such as:
- username
- email
- application name

This strengthens detection against passwords like:
`john2024!`

---

## Performance notes
- fast enough for auth flows
- do not run in hot loops
- safe for registration / password change paths

---

## Security reality check
zxcvbn:
- ✅ improves password quality
- ❌ does not hash passwords
- ❌ does not prevent credential stuffing
- ❌ does not replace MFA

It is **one layer** in a defense-in-depth strategy.

---

## When NOT to use zxcvbn
- API keys
- machine-generated secrets
- service-to-service auth

Use cryptographically secure random generators instead.

---

## TL;DR

- zxcvbn = entropy + pattern detection
- require `score >= 3`
- service-layer enforcement
- length still matters
- never reinvent this
