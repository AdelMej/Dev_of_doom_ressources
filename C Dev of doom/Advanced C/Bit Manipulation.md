# 🔹 Bit Manipulation Cheat Sheet (C)

## 1. Building Masks

```c
1U << n        // bit mask with only bit n set 
(1U << n) - 1  // mask with lowest n bits set 
~0U            // mask with all bits set (all 1s)
```

Examples:

- `1U << 3 = 00001000`
- `(1U << 5) - 1 = 00011111`

---

## 2. Single Bit Operations

```c
n |= (1U << k);    // set bit k to 1 
n &= ~(1U << k);   // clear bit k (set to 0) 
n ^= (1U << k);    // toggle bit k 
(n >> k) & 1U;     // test bit k (result = 0 or 1)
```

---

## 3. Multi-Bit Operations (using a mask)

```c
n |= mask;       // set all bits in mask 
n &= ~mask;      // clear all bits in mask 
n ^= mask;       // toggle all bits in mask 
n & mask;        // test if any bit in mask is set 
(n & mask) == mask;  // test if all bits in mask are set
```

Example:

`unsigned int mask = 0x0F;   // lower 4 bits (1111)`

---

## 4. Using Bitmasks as Flags (the fun part 🎉)

### Define flags

```c
#define FLAG_READ    (1U << 0)  // 0001 
#define FLAG_WRITE   (1U << 1)  // 0010 
#define FLAG_EXEC    (1U << 2)  // 0100 
#define FLAG_HIDDEN  (1U << 3)  // 1000
```

### Store them

```c
unsigned int permissions = 0;
```

### Set flags

```c
permissions |= FLAG_READ | FLAG_WRITE;   // enable READ + WRITE
```

### Clear a flag

```c
permissions &= ~FLAG_HIDDEN;             // disable HIDDEN
```

### Toggle a flag

```c
permissions ^= FLAG_EXEC;                // flip EXEC on/off`
```

### Test flags
```c
if (permissions & FLAG_READ) {
     // READ is enabled 
}

if ((permissions & (FLAG_READ | FLAG_WRITE)) == (FLAG_READ | FLAG_WRITE)) {
	// both READ and WRITE are enabled 
}
```

---

## 5. Real Example: Unix-like Permissions

```c
#define PERM_READ   (1U << 0)  // 001 
#define PERM_WRITE  (1U << 1)  // 010 
#define PERM_EXEC   (1U << 2)  // 100  

unsigned int file_perm = 0;  

// give read + write 
file_perm |= PERM_READ | PERM_WRITE;  

// remove write 
file_perm &= ~PERM_WRITE;  

// check if executable 
if (file_perm & PERM_EXEC) {     
	 // run it 
}
```

---

## 6. Quick Reference

- **Set** → `|=`
- **Clear** → `&= ~`
- **Toggle** → `^=`
- **Test** → `&`