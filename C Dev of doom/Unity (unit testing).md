# Unity Testing Framework Cheat Sheet

Unity is a lightweight C unit testing framework designed for simplicity and ease of use.

---

## Installation

1. Download Unity from the [official GitHub repo](https://github.com/ThrowTheSwitch/Unity).

2. Copy the `src/unity.c` and `src/unity.h` files into your project folder (e.g., `tests/`).

---

## Basic Setup

Create a test file, e.g., `test_example.c`:

```c
#include "unity.h"

void setUp(void) {
    // Runs before each test
}

void tearDown(void) {
    // Runs after each test
}

void test_Addition(void) {
    TEST_ASSERT_EQUAL_INT(4, 2 + 2);
}

int main(void) {
    UNITY_BEGIN();
    RUN_TEST(test_Addition);
    return UNITY_END();
}
```

Common Assertions
Assertion	Description
TEST_ASSERT(condition)	Passes if condition is true.
TEST_ASSERT_TRUE(condition)	Passes if condition is true.
TEST_ASSERT_FALSE(condition)	Passes if condition is false.
TEST_ASSERT_EQUAL_INT(expected, actual)	Compares two integers for equality.
TEST_ASSERT_EQUAL_STRING(expected, actual)	Compares two strings.
TEST_ASSERT_NULL(pointer)	Passes if pointer is NULL.
TEST_ASSERT_NOT_NULL(pointer)	Passes if pointer is not NULL.

Running Tests
Compile your test file with Unity source:

```bash
gcc -o test_example test_example.c unity.c
./test_example
```

Test Output Example
```bash
test_example.c:10:test_Addition:PASS
-----------------------
1 Tests 0 Failures 0 Ignored
OK
```
Organizing Multiple Tests

You can have multiple test functions:
```c
void test_Subtraction(void) {
    TEST_ASSERT_EQUAL_INT(0, 2 - 2);
}

int main(void) {
    UNITY_BEGIN();
    RUN_TEST(test_Addition);
    RUN_TEST(test_Subtraction);
    return UNITY_END();
}
```
Advanced Features

Use setUp() and tearDown() to prepare and clean resources.

Use TEST_ASSERT_FLOAT_WITHIN(delta, expected, actual) for floating point comparisons.

You can group tests into suites by organizing in different files.

Useful Links
[Unity GitHub Repository](https://github.com/ThrowTheSwitch/Unity)
[Unity Documentation](https://github.com/ThrowTheSwitch/Unity/blob/master/docs/UnityAssertionsReference.md)
