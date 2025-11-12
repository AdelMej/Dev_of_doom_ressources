# 🔐 Spring Boot JWT Token Creation Cheat Sheet

## 🚀 Overview

This cheat sheet covers **JWT generation** in Spring Boot using the `io.jsonwebtoken` (jjwt) library.  
You’ll learn how to generate JWT tokens with expiration and optional custom claims.

---

## 🧩 Step 1 — Add Dependencies (Maven)

Add these inside your `pom.xml`:

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.11.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId> <!-- for JSON parser -->
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
```

---

## 🧠 Step 2 — Create the `JwtService`

📁 `src/main/java/com/damini/authapi/security/JwtService.java`

```java
package com.damini.authapi.security;

import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.security.Keys;
import org.springframework.stereotype.Service;

import java.security.Key;
import java.util.Date;
import java.util.Map;

@Service
public class JwtService {

    // Ideally load from environment variable or config file
    private final Key key = Keys.secretKeyFor(SignatureAlgorithm.HS256);

    public String generateToken(String username, Map<String, Object> extraClaims) {
        long expiration = 1000 * 60 * 60; // 1 hour
        return Jwts.builder()
                .setClaims(extraClaims)
                .setSubject(username)
                .setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + expiration))
                .signWith(key)
                .compact();
    }

    public String generateToken(String username) {
        return generateToken(username, Map.of());
    }
}
```

✅ Uses HS256 signing algorithm  
✅ Supports custom claims (roles, permissions, etc.)  
✅ Token expires in 1 hour by default  

---

## ⚙️ Step 3 — Create a Controller to Test It

📁 `src/main/java/com/damini/authapi/controllers/AuthController.java`

```java
package com.damini.authapi.controllers;

import com.damini.authapi.security.JwtService;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/auth")
public class AuthController {

    private final JwtService jwtService;

    public AuthController(JwtService jwtService) {
        this.jwtService = jwtService;
    }

    @PostMapping("/token")
    public String getToken(@RequestParam String username) {
        return jwtService.generateToken(username);
    }
}
```

---

## 🧪 Step 4 — Test Your Token

Run the app:
```bash
./mvnw spring-boot:run
```

Call the endpoint:
```bash
curl -X POST "http://localhost:8080/api/auth/token?username=damini"
```

Output example:
```
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJkYW1pbmkiLCJleHAiOjE3MjkxMzY0MDB9.ZflYhCB5c8wC5YFw-YcZ8uwEwhA2dtA2x2R2A_rA_7k
```

---

## 🧩 Token Structure

A JWT has 3 parts:
```
xxxxx.yyyyy.zzzzz
```

| Part | Description |
|------|--------------|
| Header | Algorithm & token type |
| Payload | Claims (e.g. username, roles, exp) |
| Signature | Verifies authenticity |

---

## 🧠 Optional — Add Custom Claims

```java
Map<String, Object> claims = Map.of("role", "ADMIN", "verified", true);
String token = jwtService.generateToken("damini", claims);
```

Decoded payload (example):
```json
{
  "sub": "damini",
  "role": "ADMIN",
  "verified": true,
  "exp": 1729136400
}
```

---

## 🧱 Best Practices

| Practice | Description |
|-----------|--------------|
| 🔒 Store secret key securely | Use environment variables or config server |
| ⏱️ Short expiration | Prevents abuse of stolen tokens |
| 🧩 Add claims sparingly | Only include necessary info |
| 🧹 Regenerate key carefully | Changing the key invalidates all tokens |

---

## ✅ TL;DR Summary

| Task | Action |
|------|--------|
| Add jjwt dependencies | Include 3 JJWT packages |
| Create service | Use `Keys.secretKeyFor(SignatureAlgorithm.HS256)` |
| Generate token | Call `generateToken(username)` |
| Add claims | Pass a Map to `generateToken()` |
| Test endpoint | `/api/auth/token?username=damini` |

---

**Next Step →** [JWT Validation & Extraction Cheat Sheet (coming next)]()
