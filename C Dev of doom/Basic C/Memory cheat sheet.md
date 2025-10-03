# 🧠 C Memory Cheat Sheet

---

## 🔑 Memory Keywords

| Keyword    | Meaning                                                     |
| ---------- | ----------------------------------------------------------- |
| `auto`     | Default storage class (local vars)                          |
| `static`   | Persists value between function calls                       |
| `extern`   | Declares a variable defined elsewhere                       |
| `register` | Suggests storing var in CPU register                        |
| `const`    | Makes variable read-only                                    |
| `volatile` | Prevents compiler optimizations (var may change externally) |

---

## 📦 Dynamic Memory Allocation (`stdlib.h`)

| Function      | Example                           | Meaning                                        |
|---------------|-----------------------------------|------------------------------------------------|
| `malloc()`    | `int *p = malloc(10 * sizeof(int));` | Allocates memory (uninitialized)               |
| `calloc()`    | `int *p = calloc(10, sizeof(int));` | Allocates and zero-initializes memory          |
| `realloc()`   | `p = realloc(p, 20 * sizeof(int));` | Resizes previously allocated memory            |
| `free()`      | `free(p);`                        | Frees allocated memory                         |

---

## 📏 Memory Size

| Operator / Function | Example             | Meaning                            |
|---------------------|---------------------|------------------------------------|
| `sizeof`           | `sizeof(int)`       | Returns size in bytes of a type    |
| `sizeof var`       | `sizeof arr`        | Returns size of variable in bytes  |

---

## 🔗 Memory Functions (`string.h`)

| Function   | Example                             | Meaning                               |
|------------|-------------------------------------|---------------------------------------|
| `memcpy()` | `memcpy(dst, src, n);`             | Copies `n` bytes from src → dst       |
| `memmove()`| `memmove(dst, src, n);`            | Safe copy (handles overlap)           |
| `memset()` | `memset(buf, 0, n);`               | Sets block of memory to a value       |
| `memcmp()` | `memcmp(a, b, n);`                 | Compares `n` bytes (returns <0, 0, >0)|

---

## ⚠️ Common Pitfalls

- Always `free()` what you `malloc()`/`calloc()`/`realloc()`.  
- Using freed memory = **undefined behavior**.  
- Forgetting to free = **memory leak**.  
- `sizeof(pointer)` ≠ size of what it points to.  
