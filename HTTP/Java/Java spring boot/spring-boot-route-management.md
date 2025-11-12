# ☕ Spring Boot Route Creation & Management Cheat Sheet

## 🚀 Overview
Spring Boot automatically registers routes (endpoints) using **annotations**.  
No need for manual route registration — as long as your controllers are inside the same package (or a subpackage) as your main `@SpringBootApplication` class.

---

## 🧩 Basic Structure

```
src/
 └─ main/
     └─ java/
         └─ com/
             └─ damini/
                 └─ authapi/
                     ├─ DaMiniAuthApiApplication.java
                     └─ controllers/
                         └─ AuthController.java
```

- `DaMiniAuthApiApplication.java` → main entry point.
- `AuthController.java` → holds your routes.

---

## ⚙️ Main Application Class

```java
package com.damini.authapi;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DaMiniAuthApiApplication {
    public static void main(String[] args) {
        SpringApplication.run(DaMiniAuthApiApplication.class, args);
    }
}
```

✅ `@SpringBootApplication` tells Spring to **scan** all classes under `com.damini.authapi`  
✅ Any `@RestController` inside that package (or its subpackages) is **automatically registered**

---

## 🧭 Creating Routes

### 🧱 Step 1 — Create a Controller

```java
package com.damini.authapi.controllers;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/auth")
public class AuthController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello from Auth API 👋";
    }

    @PostMapping("/register")
    public String register(@RequestBody String body) {
        return "Register endpoint called with: " + body;
    }
}
```

### ✅ Resulting Endpoints
| HTTP Method | Path | Description |
|--------------|------|-------------|
| GET | `/api/auth/hello` | Simple test route |
| POST | `/api/auth/register` | Example route with request body |

---

## 🧠 Route Mapping Annotations

| Annotation | Description | Example |
|-------------|--------------|----------|
| `@GetMapping` | Handles HTTP GET | `@GetMapping("/users")` |
| `@PostMapping` | Handles HTTP POST | `@PostMapping("/login")` |
| `@PutMapping` | Handles HTTP PUT | `@PutMapping("/users/{id}")` |
| `@DeleteMapping` | Handles HTTP DELETE | `@DeleteMapping("/users/{id}")` |
| `@RequestMapping` | Base path or general-purpose mapping | `@RequestMapping("/api")` |

---

## 💬 Path Variables & Query Params

```java
@GetMapping("/users/{id}")
public String getUser(@PathVariable Long id) {
    return "User ID: " + id;
}

@GetMapping("/search")
public String search(@RequestParam String q) {
    return "Searching for: " + q;
}
```

✅ `/users/10` → `"User ID: 10"`  
✅ `/search?q=damini` → `"Searching for: damini"`

---

## 🧰 Returning JSON Objects

Spring automatically converts Java objects into JSON via Jackson.

```java
@GetMapping("/profile")
public Map<String, String> getProfile() {
    return Map.of("username", "damini", "role", "admin");
}
```

➡️ Returns:
```json
{
  "username": "damini",
  "role": "admin"
}
```

---

## 🧱 Folder Organization Best Practice

```
src/main/java/com/damini/authapi/
├─ controllers/   # Route definitions (AuthController, UserController, etc.)
├─ services/      # Business logic (AuthService, TokenService, etc.)
├─ repositories/  # Database access (UserRepository, TokenRepository, etc.)
├─ models/        # Entities and DTOs
└─ security/      # JWT, filters, config
```

---

## 🧩 TL;DR

| Task | Action |
|------|--------|
| Create controller | `@RestController` |
| Add route | `@GetMapping`, `@PostMapping`, etc. |
| Register route | Automatic (via `@SpringBootApplication` scan) |
| Group routes | Use `@RequestMapping("/api/...")` |
| Return JSON | Return a `Map`, `List`, or custom object |

---

## 🧪 Test Your API

Start the app:
```bash
./mvnw spring-boot:run
```

Test it:
```bash
curl http://localhost:8080/api/auth/hello
```

Output:
```
Hello from Auth API 👋
```

---

## 🧭 Quick Reference

| Command | Description |
|----------|-------------|
| `@RestController` | Marks a class as a REST endpoint |
| `@RequestMapping("/api")` | Sets base path for the controller |
| `@GetMapping`, `@PostMapping`, etc. | Define specific HTTP methods |
| `@PathVariable` | Extracts variable from URL path |
| `@RequestParam` | Extracts query parameter |
| `@RequestBody` | Reads JSON body from request |

---

**✅ Done!**  
That’s how route creation and management work in Spring Boot — no manual registration, just structure and annotations.
