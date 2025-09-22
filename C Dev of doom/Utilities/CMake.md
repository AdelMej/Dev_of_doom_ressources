# 🔥 CMake Tutorial for C Developers
## 1. What is CMake?

CMake is a build system generator. It doesn’t compile code itself — it generates Makefiles, Ninja files, or Visual Studio projects, then you use those to build your program.

Advantages:

Cross-platform (Linux, Windows, macOS).

Handles dependencies cleanly.

Works great for both small projects and huge codebases.

Has built-in support for testing (CTest), packaging, and installation.

## 2. Basic Workflow

Create a CMakeLists.txt in your project.

Run cmake .. in a build/ folder.

Run make (or cmake --build .).

Optionally: run ctest to execute tests.

## 3. Minimal Example
Project structure:
```css
MyProject/
├── CMakeLists.txt
├── src/
│   └── main.c
```
`src/main.c`
```c
#include <stdio.h>
int main(void) {
    printf("Hello, CMake!\n");
    return 0;
}
```
CMakeLists.txt
```cmake
cmake_minimum_required(VERSION 3.10)
project(MyProject LANGUAGES C)

add_executable(myapp src/main.c)
```

Build & Run:
```bash
mkdir build && cd build
cmake ..
make
./myapp
```

## 4. Multiple Source Files

Add more .c files:
```css
src/
 ├── main.c
 ├── utils.c
 └── math.c
```

Update CMakeLists.txt:
```cmake
add_executable(myapp
    src/main.c
    src/utils.c
    src/math.c
)
```

Or use a variable:
```cmake
set(SOURCES
    src/main.c
    src/utils.c
    src/math.c
)
add_executable(myapp ${SOURCES})
```

## 5. Organizing with Headers
```css
include/
 ├── utils.h
 └── math.h
```

Tell CMake where headers live:
```cmake
target_include_directories(myapp PRIVATE include)
```

Now `#include` "utils.h" works.

## 6. Libraries

Let’s say you want to split your code into a library + an app.
```css
src/
 ├── utils.c
 ├── utils.h
 └── main.c
```

CMakeLists.txt
```cmake
add_library(utils src/utils.c)
target_include_directories(utils PUBLIC src)

add_executable(myapp src/main.c)
target_link_libraries(myapp PRIVATE utils)
```

utils is compiled separately.

myapp links against it.

## 7. Multiple Executables
```css
src/
 ├── main_app.c
 ├── main_tool.c
 └── utils.c
```

```cmake
add_executable(app src/main_app.c src/utils.c)
add_executable(tool src/main_tool.c src/utils.c)
```


Now you get two binaries: app and tool.

## 8. Using Wildcards (GLOB)

You can grab all files automatically:
```cmake
file(GLOB SOURCES "src/*.c")
add_executable(myapp ${SOURCES})
```

⚠️ Downside: If you add new files, you may need to re-run cmake ...
So best practice: use GLOB for tests or quick hacks, but list manually for main app code.

## 9. Adding Tests (Unity + CTest)

If you put Unity inside tests/unity/ and your test files in tests/:

### Unity framework
```cmake
add_library(Unity tests/unity/src/unity.c)
target_include_directories(Unity PUBLIC tests/unity/src)
```
### Find all test files
```cmake
file(GLOB TEST_SOURCES "tests/test_*.c")

enable_testing()

foreach(TEST_FILE ${TEST_SOURCES})
    get_filename_component(TEST_NAME ${TEST_FILE} NAME_WE)
    add_executable(${TEST_NAME} ${TEST_FILE} src/utils.c)
    target_link_libraries(${TEST_NAME} PRIVATE Unity)
    target_include_directories(${TEST_NAME} PRIVATE include)
    add_test(NAME ${TEST_NAME} COMMAND ${TEST_NAME})
endforeach()
```

Now:
```bash
ctest --output-on-failure
```

runs all Unity tests 🚀

## 10. Parallel Builds

Speed things up:
```bash
make -j$(nproc)
ctest -j$(nproc)
```

## 11. Installing (Optional)

You can make your project installable system-wide:
```cmake
install(TARGETS myapp DESTINATION bin)
install(FILES include/utils.h DESTINATION include)
```

Then:
```bash
make install
```

## 12. Best Practices

One CMakeLists.txt per directory.

Keep source, headers, and tests organized.

Use targets (add_library, add_executable) instead of global variables.

Use target_include_directories, target_link_libraries instead of global include_directories or link_libraries.

Always build in a build/ folder (never pollute source).