# 🚨 Spring Boot Exception Handling — Complete Cheat Sheet

A practical reference covering exception types, handlers, patterns, and best practices for Spring Boot applications.

## 🔹 1. Exception Types in Spring Boot

### Runtime vs. Checked Exceptions
- **RuntimeException**: Unchecked; ideal for most business errors.
- **Checked Exception**: Must be declared/caught; rarely used in Spring Boot.

### Custom Exceptions
```java
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

## 🔹 2. @ControllerAdvice / @RestControllerAdvice
Global exception handling across the app.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ApiError> handleNotFound(ResourceNotFoundException ex) {
        ApiError error = new ApiError(
            HttpStatus.NOT_FOUND.value(),
            ex.getMessage(),
            LocalDateTime.now()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ApiError> handleGeneric(Exception ex) {
        ApiError error = new ApiError(
            HttpStatus.INTERNAL_SERVER_ERROR.value(),
            "An unexpected error occurred",
            LocalDateTime.now()
        );
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}
```

## 🔹 3. ApiError Model
```java
public class ApiError {
    private int status;
    private String message;
    private LocalDateTime timestamp;

    public ApiError(int status, String message, LocalDateTime timestamp) {
        this.status = status;
        this.message = message;
        this.timestamp = timestamp;
    }
}
```

## 🔹 4. Best Practices
- Use an `exception` package.
- Create custom RuntimeExceptions.
- Global handler for defaults.
- Consistent JSON errors.
- Correct HTTP status codes.

## 🔹 5. HTTP Status Mapping

| Exception | Status |
|----------|--------|
| ResourceNotFoundException | 404 |
| InvalidInputException | 400 |
| UnauthorizedException | 401 |
| ForbiddenException | 403 |
| ConflictException | 409 |
| ExternalServiceException | 502 |
| Generic Exception | 500 |

## 🔹 6. Validation Handling
```java
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<ApiError> handleValidation(MethodArgumentNotValidException ex) {
    String message = ex.getBindingResult()
                       .getFieldErrors()
                       .stream()
                       .map(err -> err.getField() + ": " + err.getDefaultMessage())
                       .collect(Collectors.joining(", "));
    ApiError error = new ApiError(400, message, LocalDateTime.now());
    return ResponseEntity.badRequest().body(error);
}
```

## 🔹 7. Throwing Exceptions
```java
public User getUser(Long id) {
    return userRepository.findById(id)
        .orElseThrow(() -> new ResourceNotFoundException("User " + id + " not found"));
}
```

## 🔹 8. Testing Exception Handlers
```java
@Test
void whenUserNotFound_thenReturns404() throws Exception {
    when(service.getUser(99L)).thenThrow(new ResourceNotFoundException("Not found"));

    mockMvc.perform(get("/users/99"))
           .andExpect(status().isNotFound())
           .andExpect(jsonPath("$.message").value("Not found"));
}
```

## 🔹 9. Summary
- Exceptions in `exception/` package  
- Custom RuntimeExceptions  
- Global handler for defaults  
- Additional handlers for modules if needed  
- Consistent structured API errors  
