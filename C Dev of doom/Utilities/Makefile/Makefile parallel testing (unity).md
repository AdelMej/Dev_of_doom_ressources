# 🧪 Unity C Unit Testing Setup with Parallel Execution

This guide walks you through setting up [Unity](https://github.com/ThrowTheSwitch/Unity) for C unit testing with support for **parallel test execution** using `xargs`.

---

## ✅ Prerequisites

### 🔧 Tools you need:
- `make` (build system)
- `gcc` (or any C compiler)
- `git`
- `xargs` (already on most Linux distros)
- `Unity` (unit testing framework)

---

## 📦 Installation

### 1. Clone Unity:
```bash
git clone https://github.com/ThrowTheSwitch/Unity.git
```

You only need the src/ folder from Unity for basic usage:

```bash
mkdir -p test/vendor
cp -r Unity/src test/vendor/unity
```
### 📁 Folder Structure
```css
project-root/
├── src/
│   ├── main.c
│   └── ...
├── test/
│   ├── test_math.c
│   ├── test_io.c
│   └── vendor/
│       └── unity/
│           └── unity.c, unity.h
├── build/
├── Makefile
```

### ⚙️ Makefile (with Parallel Test Execution)
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -O0 -g
UNITY_DIR = test/vendor/unity
SRC = $(wildcard src/*.c)
TESTS = $(wildcard test/test_*.c)
BUILD_DIR = build
BINARIES = $(patsubst test/%.c, $(BUILD_DIR)/%, $(TESTS))

.PHONY: all clean test

all: test

# Build each test as an independent executable
$(BUILD_DIR)/%: test/%.c $(UNITY_DIR)/unity.c $(SRC)
	@mkdir -p $(BUILD_DIR)
	$(CC) $(CFLAGS) $< $(UNITY_DIR)/unity.c $(SRC) -o $@

test: $(BINARIES)
	@echo "🧪 Running unit tests in parallel..."
	@find $(BUILD_DIR) -type f -executable | xargs -n1 -P$(shell nproc) -I{} sh -c '{}'

clean:
	rm -rf $(BUILD_DIR)
```

### 🧪 Sample Test File: test/test_math.c
```c
#include "unity.h"

// Example function to test
int add(int a, int b) {
    return a + b;
}

void setUp(void) {}
void tearDown(void) {}

void test_addition(void) {
    TEST_ASSERT_EQUAL_INT(5, add(2, 3));
}

int main(void) {
    UNITY_BEGIN();
    RUN_TEST(test_addition);
    return UNITY_END();
}
```

### 🚀 Running the Tests
```bash
make
```

This:
- Compiles each test_\*.c into build/test_\*
- Executes them in parallel

Outputs test results per binary

### 🧼 Cleaning up
```bash
make clean
```

### 💡 Tips :

- Split tests logically (e.g., test_string.c, test_math.c) for better parallelism.
- Make sure test binaries are independent and don't share file state.
- Use -O0 in CFLAGS to avoid compiler optimizations during testing.

You can redirect logs like:
```bash
make test > test_output.log 2>&1
```