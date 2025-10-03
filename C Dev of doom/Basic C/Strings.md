# 🔤 C String Utilities Cheat Sheet (`string.h`)

---

## 📏 Length & Copy

| Function    | Example                 | Meaning                                          |
| ----------- | ----------------------- | ------------------------------------------------ |
| `strlen()`  | `size_t n = strlen(s);` | Returns length of string (no `\0`)               |
| `strcpy()`  | `strcpy(dst, src);`     | Copies string (unsafe, no bounds check)          |
| `strncpy()` | `strncpy(dst, src, n);` | Copies up to `n` chars, may not `\0` terminate   |
| `strdup()`  | `char *p = strdup(s);`  | Allocates copy of string (non-standard C, POSIX) |

---

## 🔗 Concatenation

| Function     | Example                          | Meaning                                  |
|--------------|----------------------------------|------------------------------------------|
| `strcat()`   | `strcat(dst, src);`              | Appends string (unsafe, no bounds check) |
| `strncat()`  | `strncat(dst, src, n);`          | Appends up to `n` chars                  |

---

## 🔍 Comparison

| Function     | Example                          | Meaning                                  |
|--------------|----------------------------------|------------------------------------------|
| `strcmp()`   | `strcmp(a, b);`                  | Compare strings (<0, 0, >0)              |
| `strncmp()`  | `strncmp(a, b, n);`              | Compare first `n` chars                  |

---

## 🔎 Search

| Function     | Example                          | Meaning                                  |
|--------------|----------------------------------|------------------------------------------|
| `strchr()`   | `strchr(s, 'c');`                | Find first occurrence of char             |
| `strrchr()`  | `strrchr(s, 'c');`               | Find last occurrence of char              |
| `strstr()`   | `strstr(s, "sub");`              | Find substring                            |
| `strpbrk()`  | `strpbrk(s, "abc");`             | Find first match of any char in set       |
| `strspn()`   | `strspn(s, "abc");`              | Count prefix chars in set                 |
| `strcspn()`  | `strcspn(s, "abc");`             | Count prefix chars *not* in set           |

---

## ✂️ Tokenizing

| Function     | Example                          | Meaning                                  |
|--------------|----------------------------------|------------------------------------------|
| `strtok()`   | `strtok(s, " ,;");`              | Splits string into tokens (destructive)  |

---

## ⚠️ Gotchas

- `strcpy`, `strcat`, `strtok` are **unsafe** (buffer overflows).  
- Always prefer `strncpy`/`strncat`, but watch out: they may not null-terminate.  
- Safer alternatives exist in POSIX (`strdup`) and C11 Annex K (`strcpy_s`, etc.), but portability varies.  

 `strdup` is POSIX, not ISO C.  
