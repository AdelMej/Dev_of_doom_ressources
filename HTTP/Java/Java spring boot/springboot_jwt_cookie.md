# 🍪 Spring Boot JWT + HttpOnly Cookies Cheat Sheet (Jakarta Edition)

## 🧩 Overview
This guide shows how to securely handle **refresh tokens** using **HttpOnly cookies** in Spring Boot 3+ (Jakarta).  
JWT access tokens stay short-lived (in memory), while refresh tokens are stored as cookies — hidden from JavaScript and protected against XSS.

---

## ⚙️ 1. Why HttpOnly Cookies
| Storage Method | Accessible by JS? | Secure Against XSS? | Notes |
|-----------------|------------------|---------------------|-------|
| LocalStorage    | ✅ Yes            | ❌ No               | Easy but insecure for tokens |
| HttpOnly Cookie | ❌ No             | ✅ Yes              | Best for refresh tokens |

---

## 🪄 2. Setting the Refresh Token Cookie (Login)
In your **AuthController**, when login succeeds, generate the refresh token and send it as an HttpOnly cookie.

```java
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/auth")
public class AuthController {

    private final JwtService jwtService;

    public AuthController(JwtService jwtService) {
        this.jwtService = jwtService;
    }

    @PostMapping("/login")
    public ResponseEntity<?> login(@RequestBody LoginRequest request, HttpServletResponse response) {
        // Authenticate user here...

        String accessToken = jwtService.generateAccessToken(request.getUsername());
        String refreshToken = jwtService.generateRefreshToken(request.getUsername());

        // Set refresh token as HttpOnly cookie
        var cookie = new jakarta.servlet.http.Cookie("refresh_token", refreshToken);
        cookie.setHttpOnly(true);
        cookie.setSecure(true); // true in production (requires HTTPS)
        cookie.setPath("/");
        cookie.setMaxAge(7 * 24 * 60 * 60); // 7 days
        cookie.setSameSite("Strict"); // Prevent CSRF

        response.addCookie(cookie);

        return ResponseEntity.ok(new AuthResponse(accessToken));
    }
}
```

---

## 🔄 3. Reading the Refresh Token Cookie (Refresh Route)
When the client requests a new access token:

```java
import jakarta.servlet.http.HttpServletRequest;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/auth")
public class TokenController {

    private final JwtService jwtService;

    public TokenController(JwtService jwtService) {
        this.jwtService = jwtService;
    }

    @PostMapping("/refresh")
    public ResponseEntity<?> refreshToken(HttpServletRequest request) {
        String refreshToken = null;

        if (request.getCookies() != null) {
            for (var cookie : request.getCookies()) {
                if ("refresh_token".equals(cookie.getName())) {
                    refreshToken = cookie.getValue();
                }
            }
        }

        if (refreshToken == null || !jwtService.validateToken(refreshToken)) {
            return ResponseEntity.status(401).body("Invalid refresh token");
        }

        String username = jwtService.extractUsername(refreshToken);
        String newAccessToken = jwtService.generateAccessToken(username);

        return ResponseEntity.ok(new AuthResponse(newAccessToken));
    }
}
```

---

## 🚪 4. Clearing the Cookie (Logout)

```java
@PostMapping("/logout")
public ResponseEntity<?> logout(HttpServletResponse response) {
    var cookie = new jakarta.servlet.http.Cookie("refresh_token", null);
    cookie.setHttpOnly(true);
    cookie.setSecure(true);
    cookie.setPath("/");
    cookie.setMaxAge(0); // delete
    response.addCookie(cookie);

    return ResponseEntity.ok("Logged out");
}
```

---

## 🧠 5. Security Tips
- Always set **`Secure=true`** in production (HTTPS).
- Use **`SameSite=Strict`** unless you have cross-domain needs.
- Store refresh tokens **hashed** in DB if you allow revocation.
- Access tokens remain in memory (frontend state).

---

## ⚙️ 6. Local Dev Tip (No HTTPS)
For local testing (e.g., `localhost:8080`), set:
```java
cookie.setSecure(false);
```
But **never deploy like that** to production 😅.

---

## 🧩 TL;DR Summary
| Action | Cookie? | Token Type | Where Stored |
|--------|----------|-------------|---------------|
| Login  | ✅ Set   | Refresh     | HttpOnly Cookie |
| Refresh | ✅ Read | Access      | JSON response |
| Logout | ❌ Clear | Refresh     | Cookie deleted |

---

✅ You now have **secure, HttpOnly-based refresh token flow** ready for real-world APIs.

