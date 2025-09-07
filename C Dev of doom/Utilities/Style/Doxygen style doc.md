# 📝 Doxygen-Style Documentation Guide for C
## 1. File Header

Provide a brief description of the file and its purpose.
```c
/**
 * matrix.h - Header file for Matrix library
 *
 * Provides functions for creating, manipulating, and freeing 2D matrices.
 */
```


## 2. Function Documentation

Document each function above its declaration or definition.
```c
/**
 * add - Adds two integers.
 * @a: First integer
 * @b: Second integer
 *
 * Return: Sum of a and b
 */
int add(int a, int b);
```

### Tips:

Start with a brief description.

List all parameters with @`<name>` or @param.

Describe the return value with @return or Return:

## 3. Struct Documentation

Document structs and each member.
```c
/**
 * struct Matrix - Represents a 2D matrix.
 * @rows: Number of rows.
 * @cols: Number of columns.
 * @data: Pointer to an array storing matrix elements.
 */
typedef struct Matrix {
    int rows;
    int cols;
    float *data;
} Matrix;
```


## 4. Enum Documentation
```c
/**
 * enum Direction - Direction constants for movement.
 * @UP: Up direction.
 * @DOWN: Down direction.
 * @LEFT: Left direction.
 * @RIGHT: Right direction.
 */
typedef enum Direction {
    UP,
    DOWN,
    LEFT,
    RIGHT
} Direction;
```


## 5. Macros and Defines
```c
/**
 * MAX_SIZE - Maximum size of the matrix.
 */
#define MAX_SIZE 100
```


## 6. Inline Comments

Keep inline comments short and only for complex logic.
```c
int x = y * 2; // Double y for scaling
```


## 7. Best Practices

Keep line width consistent (typically 80 chars).

Keep comments aligned with code indentation.

Be concise but descriptive.

Use /** ... */ for block comments over /* ... */ for functions/structs.