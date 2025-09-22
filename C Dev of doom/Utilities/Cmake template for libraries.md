```cmake
cmake_minimum_required(VERSION 3.15)
project(library_name C)

set(CMAKE_C_STANDARD 11)

# Include directories
include_directories(${PROJECT_SOURCE_DIR}/include)

# Source files
file(GLOB SOURCES "${PROJECT_SOURCE_DIR}/src/*.c")

# Build static library
add_library(${PROJECT_NAME} STATIC ${SOURCES})

# Optional: build executable for testing
add_executable(test_${PROJECT_NAME} tests/test_${PROJECT_NAME}.c)
target_link_libraries(test_${PROJECT_NAME} ${PROJECT_NAME})
```

**How to use:**  
- Replace `library_name` → the name of your library (`vector`, `matrix`, etc.).  
- Place `CMakeLists.txt` in the root of each library folder.  
- Keeps it **plug-and-play**, reusable for every new library you add. 