# 🔡 C Character Utilities Cheat Sheet (`ctype.h`)

---

## 🔢 Classification Functions

| Function     | Example         | Meaning                              |
|--------------|-----------------|--------------------------------------|
| `isalnum()`  | `isalnum(c)`    | Is alphanumeric (letter or digit)?   |
| `isalpha()`  | `isalpha(c)`    | Is alphabetic (A–Z, a–z)?            |
| `isdigit()`  | `isdigit(c)`    | Is decimal digit (0–9)?              |
| `isxdigit()` | `isxdigit(c)`   | Is hex digit (0–9, A–F, a–f)?        |
| `islower()`  | `islower(c)`    | Is lowercase letter?                 |
| `isupper()`  | `isupper(c)`    | Is uppercase letter?                 |
| `isspace()`  | `isspace(c)`    | Is whitespace (`' '`, `\t`, `\n`, …)?|
| `isblank()`  | `isblank(c)`    | Is blank (`' '`, `\t`)?              |
| `ispunct()`  | `ispunct(c)`    | Is punctuation character?            |
| `isprint()`  | `isprint(c)`    | Is printable (incl. space)?          |
| `isgraph()`  | `isgraph(c)`    | Is visible (printable except space)? |

---

## 🔄 Conversion Functions

| Function     | Example         | Meaning                         |
|--------------|-----------------|---------------------------------|
| `tolower()`  | `tolower(c)`    | Convert to lowercase (if letter)|
| `toupper()`  | `toupper(c)`    | Convert to uppercase (if letter)|

---

## ⚠️ Gotchas

- All functions take an `int` argument, but the value must be either:
  - `EOF`  
  - or a valid `unsigned char` value  
- Passing a plain `char` (signed by default on many systems) can cause **undefined behavior** if `char` is negative.  
  👉 Always cast: `isalpha((unsigned char)c)`.  
