# 📦 C Data Types Cheat Sheet

---

## 🔢 Integer Types

| Type              | Typical Size* | Example               | Notes                            |
|-------------------|---------------|-----------------------|----------------------------------|
| `int`             | 4 bytes       | `int x = 10;`         | Most common whole number type    |
| `short`           | 2 bytes       | `short s = 5;`        | Smaller range                     |
| `long`            | 8 bytes       | `long l = 1000;`      | Larger range                      |
| `long long`       | 8 bytes       | `long long big;`      | Guaranteed ≥ 64 bits              |
| `unsigned int`    | 4 bytes       | `unsigned int u = 5;` | No negatives, doubles positive max|

---

## 🌊 Floating-Point Types

| Type          | Typical Size* | Example                 | Notes                                 |
|---------------|---------------|-------------------------|---------------------------------------|
| `float`       | 4 bytes       | `float f = 3.14;`       | Single precision (~6–7 digits)        |
| `double`      | 8 bytes       | `double d = 2.718;`     | Double precision (~15–16 digits)      |
| `long double` | 16 bytes†     | `long double ld = 1.0;` | Extended precision (implementation-dependent) |

---

## 🔤 Character Type

| Type   | Size   | Example          | Notes                          |
|--------|--------|------------------|--------------------------------|
| `char` | 1 byte | `char c = 'A';` | Stores single character (ASCII)|

---

## 🚫 Void Type

| Type   | Size | Example         | Notes                    |
|--------|------|-----------------|--------------------------|
| `void` | —    | `void func();`  | No value / no return type|

---

## 🏗️ Derived Types

| Type      | Example               | Notes                                  |
|-----------|-----------------------|----------------------------------------|
| Array     | `int arr[10];`        | Fixed-size sequence of elements         |
| Pointer   | `int *p = &x;`        | Stores address of a variable            |
| Struct    | `struct Point {int x; int y;};` | Groups multiple fields            |
| Enum      | `enum Color {RED, GREEN, BLUE};` | User-defined symbolic constants   |

---

\* Sizes are **typical on 64-bit systems**, but may vary by compiler/architecture.  
\† `long double` size is implementation-dependent (often 16 bytes on GCC/x86_64).  
