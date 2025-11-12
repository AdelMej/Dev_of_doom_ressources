# 🛡️ Spring Boot JWT Validator Cheat Sheet (Jakarta Edition)

## 🧩 Overview
This cheat sheet shows how to **validate JWT tokens** in Spring Boot 3+ using **Jakarta EE** imports. It includes signature verification, expiration checks, and claim extraction — all clean and production-safe.

---

## ⚙️ 1. Dependencies
Add JWT dependencies to your `pom.xml` (Maven):

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.12.5</version>
    <scope>runtime</scope>
</dependency>
```

---

## 🧠 2. JwtService (Token Validation Logic)

```java
import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import org.springframework.stereotype.Service;
import java.security.Key;
import java.util.Date;

@Service
public class JwtService {

    private final Key key;

    public JwtService() {
        // Replace with your own key from env var in production
        this.key = Keys.hmacShaKeyFor(System.getenv("JWT_SECRET").getBytes());
    }

    public boolean validateToken(String token) {
        try {
            Jwts.parser()
                .verifyWith(key)
                .build()
                .parseSignedClaims(token);
            return true;
        } catch (ExpiredJwtException e) {
            System.out.println("❌ Token expired: " + e.getMessage());
        } catch (MalformedJwtException e) {
            System.out.println("❌ Malformed token: " + e.getMessage());
        } catch (UnsupportedJwtException e) {
            System.out.println("❌ Unsupported token: " + e.getMessage());
        } catch (IllegalArgumentException e) {
            System.out.println("❌ Empty token: " + e.getMessage());
        } catch (SecurityException e) {
            System.out.println("❌ Invalid signature: " + e.getMessage());
        }
        return false;
    }

    public String extractUsername(String token) {
        try {
            return Jwts.parser()
                    .verifyWith(key)
                    .build()
                    .parseSignedClaims(token)
                    .getPayload()
                    .getSubject();
        } catch (JwtException e) {
            return null;
        }
    }

    public Date extractExpiration(String token) {
        return Jwts.parser()
                .verifyWith(key)
                .build()
                .parseSignedClaims(token)
                .getPayload()
                .getExpiration();
    }
}
```

---

## 🔄 3. Example Usage

```java
@RestController
@RequestMapping("/api/test")
public class TestController {

    private final JwtService jwtService;

    public TestController(JwtService jwtService) {
        this.jwtService = jwtService;
    }

    @GetMapping("/validate")
    public ResponseEntity<?> validateToken(@RequestHeader("Authorization") String authHeader) {
        String token = authHeader.replace("Bearer ", "");

        if (jwtService.validateToken(token)) {
            String username = jwtService.extractUsername(token);
            return ResponseEntity.ok("✅ Valid token for user: " + username);
        }
        return ResponseEntity.status(401).body("❌ Invalid or expired token");
    }
}
```

---

## 🧰 4. Exception Handling Best Practices
You can replace `System.out.println()` with a custom logger or `@ControllerAdvice` handler.

Example:
```java
catch (ExpiredJwtException e) {
    log.warn("Token expired for subject: {}", e.getClaims().getSubject());
    throw new ResponseStatusException(HttpStatus.UNAUTHORIZED, "Token expired");
}
```

---

## 🔐 5. Pro Tips
- Always use **strong HMAC-SHA256 or higher** algorithms.
- Keep secrets **in environment variables**, never in source.
- Use **short-lived access tokens** (5–15 min) and **refresh tokens** (days/weeks).
- Use **`jwtService.validateToken()`** in filters or interceptors to secure routes.

---

## 🧩 TL;DR Summary
| Method | Purpose | Returns |
|--------|----------|----------|
| `validateToken(token)` | Validates signature & expiration | `boolean` |
| `extractUsername(token)` | Extracts subject claim | `String` |
| `extractExpiration(token)` | Reads token expiry | `Date` |

✅ You now have a **production-ready JWT validator** that’s lightweight, Jakarta-compliant, and secure.