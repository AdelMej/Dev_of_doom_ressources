# 🔹 File I/O in C (Cheat Sheet)
## 1. Opening a file
```c
FILE *fp = fopen("file.txt", "r");   // open for reading
FILE *fp = fopen("file.txt", "w");   // open for writing (truncates)
FILE *fp = fopen("file.txt", "a");   // open for appending
```

Common modes:

- `"r"` → read
- `"w"`  → write (truncate)
- `"a"` → append
- `"r+"` → read/write
- `"w+"` → read/write (truncate)
- `"a+"` → read/write (append)
- Add `"b"` for binary:  `"rb"`, `"wb"`, etc.

---

## 2. Closing a file
```c
fclose(fp);
```

---
## 3. Writing to a file
```c
fprintf(fp, "Hello %s\n", "World");   // formatted text
fputs("Hello World\n", fp);           // string
fputc('A', fp);                       // single char
```

---
## 4. Reading from a file
```c
fscanf(fp, "%d", &x);                 // formatted read
fgets(buffer, sizeof buffer, fp);     // read line
c = fgetc(fp);                        // read char
```

---
## 5. Binary I/O
```c
fwrite(data, sizeof(int), count, fp);
fread(data, sizeof(int), count, fp);
```

---
## 6. Error handling
```c
if (fp == NULL) {
    perror("fopen");   // print system error message
    exit(1);
}
```

---
## 7. Positioning
```c
fseek(fp, 0, SEEK_SET);   // beginning
fseek(fp, 0, SEEK_END);   // end
long pos = ftell(fp);     // current position
rewind(fp);               // shortcut to start
```

---

💡 With these, you can:

- Write logs
- Read configs
- Process binary data (e.g., saving structs)
- Implement things like mini-databases