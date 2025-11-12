# 🧩 Spring Boot I/O Validation Cheat Sheet (Jakarta Edition)

## ⚙️ 1. Dependency
Add Jakarta Bean Validation support to your project (usually included automatically, but add explicitly if needed):

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

---

## 📥 2. Input Validation with DTOs
Use validation annotations from **`jakarta.validation.constraints`** on your request DTOs.

### Example: User Registration DTO
```java
import jakarta.validation.constraints.*;

public class RegisterRequest {

    @NotBlank(message = "Username is required")
    @Size(min = 3, max = 20, message = "Username must be between 3 and 20 characters")
    private String username;

    @NotBlank(message = "Email is required")
    @Email(message = "Invalid email format")
    private String email;

    @NotBlank(message = "Password is required")
    @Size(min = 8, message = "Password must be at least 8 characters")
    private String password;

    // Getters and setters
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
}
```

### Controller Usage
```java
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import jakarta.validation.Valid;

@RestController
@RequestMapping("/api/auth")
public class RegisterController {

    @PostMapping("/register")
    public ResponseEntity<String> register(@Valid @RequestBody RegisterRequest request) {
        return ResponseEntity.ok("✅ Valid registration for: " + request.getUsername());
    }
}
```

Spring automatically validates `@Valid` annotated parameters and returns a `400 Bad Request` with details if invalid.

---

## 🧠 3. Custom Validation Example
You can define your own annotations for complex validation logic (like password strength).

### Step 1 — Create Custom Annotation
```java
import jakarta.validation.Constraint;
import jakarta.validation.Payload;
import java.lang.annotation.*;

@Target({ ElementType.FIELD })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = StrongPasswordValidator.class)
public @interface StrongPassword {
    String message() default "Password must contain letters, digits, and symbols";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};
}
```

### Step 2 — Create Validator Logic
```java
import jakarta.validation.ConstraintValidator;
import jakarta.validation.ConstraintValidatorContext;

public class StrongPasswordValidator implements ConstraintValidator<StrongPassword, String> {
    @Override
    public boolean isValid(String password, ConstraintValidatorContext context) {
        if (password == null) return false;
        return password.matches("^(?=.*[A-Za-z])(?=.*\\d)(?=.*[@$!%*?&])[A-Za-z\\d@$!%*?&]{8,}$");
    }
}
```

### Step 3 — Use It
```java
@StrongPassword
private String password;
```

---

## ⚠️ 4. Global Exception Handler for Validation Errors
To handle validation messages cleanly:

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.*;

import java.util.HashMap;
import java.util.Map;

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, String>> handleValidationErrors(MethodArgumentNotValidException ex) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getFieldErrors().forEach(error ->
                errors.put(error.getField(), error.getDefaultMessage()));
        return new ResponseEntity<>(errors, HttpStatus.BAD_REQUEST);
    }
}
```

---

## 📤 5. Output Validation / Sanitization (Optional)
Spring doesn’t automatically sanitize output. You can:
- Use **DTOs** instead of returning entities (avoid leaking internal data).
- Escape HTML in responses if user input is displayed back (e.g. Thymeleaf does this automatically).

Example:
```java
public record UserResponse(String username, String email) {}
```

---

## 🧩 TL;DR Summary
| Validation Type | Example | Notes |
|-----------------|----------|-------|
| `@NotBlank` | Non-empty string | Common for text fields |
| `@Email` | Valid email format | Built-in email check |
| `@Size(min, max)` | String length | Works on collections too |
| `@Pattern(regex)` | Regex validation | Great for usernames, phone numbers |
| Custom `@Constraint` | e.g. Strong password | For complex logic |

✅ You now have a **fully validated I/O flow** in Spring Boot using Jakarta Validation — clean, safe, and production-ready!