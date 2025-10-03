# 🧑‍💻 C Cheat Sheet (Simple Core)
## 🔹 Program Structure
```c
#include <stdio.h>

int main(void) {
    printf("Hello, World!\n");
    return 0;
}
```
## 🔹 Variables & Constants
```c
int age = 20;
const double PI = 3.14;
```

## 🔹 Control Flow
```c
if (x > 0) { ... } else { ... }

for (int i=0; i<10; i++) { ... }

while (x > 0) { ... }

do { ... } while (x > 0);

switch (n) {
  case 1: ...; break;
  default: ...; break;
}
```

## 🔹 Functions
```c
int add(int a, int b) {
    return a + b;
}
```

## 🔹 Arrays & Strings
```c
int nums[3] = {1, 2, 3};
char str[] = "Hello";
```

## 🔹 Pointers
```c
int x = 10;
int *p = &x;
printf("%d\n", *p); // prints 10
```

##🔹 Structs
```c
struct Point {
    int x;
    int y;
};
struct Point p1 = {5, 10};
```