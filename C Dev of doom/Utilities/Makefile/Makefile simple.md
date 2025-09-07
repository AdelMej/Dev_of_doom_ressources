# Makefile Tutorial: Basics and Usage

Makefiles automate building your projects by specifying how to compile and link your code.

---

## Basic structure

A Makefile contains **rules** in this format:

```makefile
target: dependencies
<TAB> commands to build the target
```

- target: file to generate (e.g., executable, object file)
- dependencies: files the target depends on (source files, headers, etc.)
- commands: shell commands to build the target (must start with a tab character)

Example: Simple C project
```makefile
# Compiler and flags
CC = gcc
CFLAGS = -Wall -Wextra -g

# Target executable
TARGET = myprog

# Source and object files
SRCS = $(wildcard *.c)
OBJS = $(SRCS:.c=.o)

# Default rule: build executable
$(TARGET): $(OBJS)
<TAB>$(CC) $(CFLAGS) -o $(TARGET) $(OBJS)

# Compile .c to .o
%.o: %.c
<TAB>$(CC) $(CFLAGS) -c $< -o $@

# Clean generated files
clean:
<TAB>rm -f $(OBJS) $(TARGET)
```

Explanation :
- $(CC) and $(CFLAGS) are variables for compiler and flags
-  $(SRCS) lists source files
-  $(OBJS) replaces .c extensions with .o (object files)
- The default rule builds the final executable from object files
- The pattern rule %.o: %.c compiles each .c file to .o

The clean rule removes build artifacts (run with make clean)

Common commands
make — builds the first target in the Makefile (usually the executable)

make clean — removes compiled files

make TARGET=otherprog — override variables from the command line

Tips
Always use tabs, not spaces, before commands!

Use variables for flexibility and easy flag changes

Use pattern rules (%.o: %.c) to avoid repeating compilation commands

Add .PHONY: clean above the clean rule to avoid filename conflicts