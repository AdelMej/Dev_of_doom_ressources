# 📌 Pointer Cheat Sheet (C Edition)

---

## 1. **What is a Pointer?**

A variable that **stores a memory address**.

```c
int x = 10; int *p = &x;   // p holds the address of x`
```

- `&x` → “address of x”
- `*p` → “value at the address p points to” (dereference)

---

## 2. **Declaring Pointers**

```c
int *p;    // pointer to int 
char *c;   // pointer to char 
void *v;   // generic pointer (can point to anything)
```

---

## 3. **Dereferencing**

```c
int x = 42; 
int *p = &x; 
printf("%d", *p);   // prints 42 
*p = 99;            // changes x to 99
```


---

## 4. **Pointers & Arrays**

👉 Arrays “decay” into pointers.

```c
int arr[3] = {10, 20, 30}; 
int *p = arr;       // same as &arr[0]  
printf("%d", *(p+1)); // 20
```

---

## 5. **Pointer Arithmetic**

Works in units of the pointed type.

```c
int arr[3] = {1,2,3};
int *p = arr;  
p++;    // now points to arr[1]
```

---

## 6. **Pointer to Pointer**

```c
int x = 5; 
int *p = &x; 
int **pp = &p;  
printf("%d", **pp);  // 5
```

---

## 7. **NULL Pointer**

```c
int *p = NULL; 
if (p == NULL) {
	printf("No address yet!");
}
```

---

## 8. **Dynamic Memory & Pointers**

```c
int *p = malloc(sizeof(int) * 5);  // allocate array of 5 ints 
p[0] = 42; 
free(p);                           // always free!`
```

---

## 9. **Function Pointers**

👉 Store function addresses.

```c
int add(int a, int b) { 
	return a+b; 
}  

int (*f)(int,int) = add; 

printf("%d", f(2,3)); // 5`
```

---

## 10. **Common Pitfalls ⚠️**

- **Dangling pointer** → pointer to freed memory.
- **Uninitialized pointer** → points to garbage.
- **Wrong pointer arithmetic** → segfault city 🏙️.
- **Pointer type mismatch** → undefined behavior.

---

### 🔑 TL;DR

- `&` = “address of”
- `*` = “value at address”
- Arrays ⇔ pointers (mostly)
- Watch out for NULL, free your stuff, don’t deref garbage.