# 📝 Variadic Functions in C — Cheat Sheet

---
## 1. Include the Header

All variadic handling macros live in:
```python
#include <stdarg.h>
```

---
## 2. Declare a Variadic Function
```c
int example(int fixed_arg, ...);
```

- ... means “variable number of arguments.”
- Usually you have at least one fixed argument to help you know how many follow.

---
## 3. Core Macros

- `va_list` → type for iterating arguments.
- `va_start(ap, last_fixed_arg)` → initialize ap.
- `va_arg(ap, type)` → get next argument, cast to type.
- `va_end(ap)` → clean up.

---
## 4. Basic Example
```c
#include <stdio.h>
#include <stdarg.h>

int sum(int count, ...) {
    va_list args;
    va_start(args, count);

    int total = 0;
    for (int i = 0; i < count; i++) {
        total += va_arg(args, int);  // each arg treated as int
    }

    va_end(args);
    return total;
}

int main(void) {
    printf("%d\n", sum(3, 1, 2, 3));   // 6
    printf("%d\n", sum(5, 10, 20, 30, 40, 50)); // 150
}
```

---
## 5. Type Safety Warning ⚠️

- The compiler doesn’t check the variadic arguments.
- Passing the wrong type = undefined behavior (💀).
- Example: If you expect an int but pass a double, it’ll break.

---
## 6. Common Patterns

- Use a count parameter (sum(count, ...)) → tells how many args.
- Or use a sentinel value (print_until(-1, ...)) → stop when special value appears.
```c
void print_until(int sentinel, ...) {
    va_list args;
    va_start(args, sentinel);
    int val;
    while ((val = va_arg(args, int)) != sentinel) {
        printf("%d ", val);
    }
    va_end(args);
}
```

Call:
```c
print_until(-1, 1, 2, 3, -1); // prints "1 2 3"
```

---
## 7. Real World: printf

- printf uses a format string to decide how many args & what type.
- That’s why "%d %f" is critical — it tells printf how to interpret the ....

---

🔑 TL;DR

- Use <stdarg.h>.
- va_list → holder, va_start → init, va_arg → fetch, va_end → cleanup.
- Decide how you’ll know when to stop (count or sentinel).
- Type mismatches = instant segfault city 🏙️.