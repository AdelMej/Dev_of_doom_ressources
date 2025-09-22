# 🧱 Makefile for Multi-File C Projects (with src/, include/, and build/)

This setup assumes the following project structure:

project/
├── Makefile
├── src/
│ ├── main.c
│ ├── utils.c
├── include/
│ ├── utils.h
├── build/

---

## 🛠️ Makefile

```makefile
# Compiler and flags
CC = gcc
CFLAGS = -Wall -Wextra -Werror -g -Iinclude

# Project folders
SRC_DIR = src
INC_DIR = include
OBJ_DIR = build
BIN = myprog

# All source files
SRCS = $(wildcard $(SRC_DIR)/*.c)

# All object files (in build/ folder)
OBJS = $(patsubst $(SRC_DIR)/%.c, $(OBJ_DIR)/%.o, $(SRCS))

# Default target
all: $(BIN)

# Link all objects into final binary
$(BIN): $(OBJS)
	$(CC) $(CFLAGS) -o $@ $^

# Compile each .c into build/.o
$(OBJ_DIR)/%.o: $(SRC_DIR)/%.c | $(OBJ_DIR)
	$(CC) $(CFLAGS) -c $< -o $@

# Ensure build/ exists
$(OBJ_DIR):
	mkdir -p $(OBJ_DIR)

# Clean rule
clean:
	rm -rf $(OBJ_DIR) $(BIN)

.PHONY: all clean
```

🧪 How to Use
```bash
make        # Compiles the program
make clean  # Removes build/ and binary
```

🧠 Notes
$(wildcard src/*.c) gets all your source files.

$(patsubst ...) transforms paths from src/file.c → build/file.o.

$< = first prerequisite (source file)

$@ = target (e.g. build/file.o)

$^ = all prerequisites (all .o files when linking)

✅ Benefits
✅ Keeps object files out of the main folder
✅ Easy to scale with more source files
✅ Avoids recompiling everything on every change
✅ Works well with tools like valgrind, gdb, and version control

🧩 Bonus: Add More Folders
Want src/core/, src/utils/, etc.? Replace:

```makefile
SRCS = $(wildcard $(SRC_DIR)/*.c)
```

With:
```makefile
SRCS = $(shell find $(SRC_DIR) -name "*.c")
```