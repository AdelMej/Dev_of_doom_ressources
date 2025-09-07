# 🧪 Makefile for Unity C Unit Tests

## 🗂️ Project Structure Assumed

project/
├── Makefile
├── src/
│ ├── mylib.c
│ └── mylib.h
├── tests/
│ ├── test_mylib.c
├── unity/
│ ├── unity.c
│ └── unity.h

---

## 🛠️ Makefile

```makefile
CC = gcc
CFLAGS = -Wall -Wextra -Iunity -Isrc

SRC = $(wildcard src/*.c)
TESTS = $(wildcard tests/test_*.c)
UNITY_SRC = unity/unity.c

# Targets will be build/test_mylib etc.
BUILD = build
BINARIES = $(patsubst tests/%.c, $(BUILD)/%, $(TESTS))

.PHONY: all clean test

# Build and run all tests
all: test

# Run tests
test: $(BINARIES)
	@for bin in $(BINARIES); do \
		echo "Running $$bin..."; \
		./$$bin; \
	done

# Build individual test binaries
$(BUILD)/%: tests/%.c $(SRC) $(UNITY_SRC) | $(BUILD)
	$(CC) $(CFLAGS) $^ -o $@

# Create build dir
$(BUILD):
	mkdir -p $(BUILD)

# Clean build and binaries
clean:
	rm -rf $(BUILD)
```

✅ How to Use
```bash
make        # Compiles and runs all tests
make clean  # Cleans build outputs
```

Each test file (like test_mylib.c) will be built and executed automatically.

💡 Notes
Your test files must be named test_*.c to be detected.

Unity must be included and compiled along with your sources.

Add more include paths to CFLAGS if needed.

🧪 Example Test File
```c
#include "unity.h"
#include "mylib.h"

void test_add_should_return_sum(void) {
    TEST_ASSERT_EQUAL_INT(4, add(2, 2));
}

int main(void) {
    UNITY_BEGIN();
    RUN_TEST(test_add_should_return_sum);
    return UNITY_END();
}
```