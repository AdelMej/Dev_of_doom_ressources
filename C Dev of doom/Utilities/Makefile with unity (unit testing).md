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
CFLAGS = -Wall -Wextra -Werror -pedantic -std=gnu89 -Iunity

SRC = $(wildcard *.c)
UNITY_SRC = unity/unity.c
TESTS = $(wildcard tests/test_*.c)

BUILD = build
MAIN_BIN = $(BUILD)/mylib
TEST_BINS = $(patsubst tests/%.c, $(BUILD)/%, $(TESTS))

.PHONY: all clean test run-tests run-%

all: $(MAIN_BIN) test

# Always build your main binary from *.c
$(MAIN_BIN): $(SRC) | $(BUILD)
	$(CC) $(CFLAGS) $^ -o $@

# Run tests if they exist
test:
	@if [ -z "$(TESTS)" ]; then \
		echo "⚠️  No test files found. Only built main binary."; \
	else \
		$(MAKE) run-tests; \
	fi

run-tests: $(TEST_BINS)
	@for bin in $(TEST_BINS); do \
		echo "Running $$bin..."; \
		if ./$$bin; then \
			echo "✅ $$bin passed"; \
		else \
			echo "❌ $$bin failed"; \
			exit 1; \
		fi; \
	done

# Build individual test binaries
$(BUILD)/%: tests/%.c $(SRC) $(UNITY_SRC) | $(BUILD)
	$(CC) $(CFLAGS) $^ -o $@

# Run a single test
run-%: $(BUILD)/%
	./$<

# Create build dir
$(BUILD):
	mkdir -p $(BUILD)

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