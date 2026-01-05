# Argon2 Cheat Sheet

## What is Argon2?

Argon2 is a modern password-hashing algorithm designed to resist GPU and
ASIC attacks. It is the **winner of the Password Hashing Competition
(PHC)**.

### Variants

-   **Argon2id** (recommended): Hybrid of Argon2i + Argon2d
-   Argon2i: Side-channel resistant
-   Argon2d: GPU-resistant but side-channel weaker

------------------------------------------------------------------------

## Installation

``` bash
pip install argon2-cffi
```

------------------------------------------------------------------------

## Basic Usage

``` python
from argon2 import PasswordHasher

ph = PasswordHasher()

hash = ph.hash("password")
ph.verify(hash, "password")
```

-   `hash()` → returns a **string**
-   `verify()` → raises exception if invalid

------------------------------------------------------------------------

## Verification Handling

``` python
from argon2.exceptions import VerifyMismatchError

try:
    ph.verify(hash, "password")
    print("OK")
except VerifyMismatchError:
    print("Invalid password")
```

------------------------------------------------------------------------

## Recommended Parameters (2025)

``` python
PasswordHasher(
    time_cost=3,
    memory_cost=65536,  # 64 MB
    parallelism=4,
    hash_len=32,
    salt_len=16,
)
```

-   Increase **memory_cost** first
-   Then increase **time_cost**
-   Keep parallelism ≤ CPU cores

------------------------------------------------------------------------

## Hash Format

Example:

    $argon2id$v=19$m=65536,t=3,p=4$<salt>$<hash>

Argon2 hashes are: - Self-describing - Versioned - Safe to store as TEXT

------------------------------------------------------------------------

## Rehash Detection (Important)

``` python
if ph.check_needs_rehash(hash):
    hash = ph.hash(password)
```

Use this after successful login to upgrade security automatically.

------------------------------------------------------------------------

## Storage Guidelines

-   Store hashes as **TEXT**
-   Never truncate hashes
-   Never compare hashes manually

------------------------------------------------------------------------

## Common Mistakes

❌ Using bcrypt-style APIs\
❌ Storing hashes as bytes\
❌ Catching generic Exception\
❌ Rolling your own salt\
❌ Hashing passwords twice

------------------------------------------------------------------------

## When to Use Argon2

✅ New applications\
✅ Security-focused systems\
✅ Long-lived projects

❌ Legacy bcrypt-only environments\
❌ Extremely low-memory systems

------------------------------------------------------------------------

## Comparison (Quick)

  Algorithm   Memory-Hard   GPU Resistant   Recommended
  ----------- ------------- --------------- -------------
  Argon2id    ✅            ✅              ⭐⭐⭐⭐⭐
  bcrypt      ❌            ⚠️              ⭐⭐⭐
  scrypt      ✅            ⚠️              ⭐⭐

------------------------------------------------------------------------

## References

-   RFC 9106
-   Password Hashing Competition
-   argon2-cffi documentation
