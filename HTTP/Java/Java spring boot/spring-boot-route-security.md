# 🔒 Spring Boot Route Security Cheat Sheet (JWT + Role-Based Access)

## 🚀 Overview
This guide covers **securing routes** in Spring Boot using:
- **JWT (JSON Web Tokens)** for authentication
- **Role-Based Access Control (RBAC)** for authorization
- **Spring Security 6+ (compatible with Java 21)**

---

## 🧩 Dependencies (add to `pom.xml`)

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

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
    <artifactId>jjwt-jackson</artifactId>
    <version>0.11.5</version>
    <scope>runtime</scope>
</dependency>
```

---

## 🧠 Security Flow

1. **User logs in** → `/api/auth/login`
2. Server validates credentials → generates JWT
3. Client stores JWT (usually in memory or cookie)
4. For protected routes → client sends JWT in header:
   ```
   Authorization: Bearer <token>
   ```
5. Server verifies token → allows or denies access

---

## 🧱 Example Structure

```
src/main/java/com/damini/authapi/
├─ controllers/
│  ├─ AuthController.java
│  └─ UserController.java
├─ security/
│  ├─ JwtAuthFilter.java
│  ├─ JwtService.java
│  └─ SecurityConfig.java
├─ services/
│  └─ UserService.java
└─ models/
   └─ User.java
```

---

## 🔑 Example `SecurityConfig.java`

```java
package com.damini.authapi.security;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    private final JwtAuthFilter jwtAuthFilter;

    public SecurityConfig(JwtAuthFilter jwtAuthFilter) {
        this.jwtAuthFilter = jwtAuthFilter;
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(csrf -> csrf.disable())
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/auth/**").permitAll()  // Public routes
                .requestMatchers("/api/admin/**").hasRole("ADMIN")  // Role-based access
                .anyRequest().authenticated()  // Everything else requires JWT
            )
            .addFilterBefore(jwtAuthFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
}
```

---

## 🔍 Example `JwtAuthFilter.java`

```java
package com.damini.authapi.security;

import jakarta.servlet.FilterChain;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.WebAuthenticationDetailsSource;
import org.springframework.stereotype.Component;
import org.springframework.web.filter.OncePerRequestFilter;
import java.io.IOException;

@Component
public class JwtAuthFilter extends OncePerRequestFilter {

    private final JwtService jwtService;

    public JwtAuthFilter(JwtService jwtService) {
        this.jwtService = jwtService;
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain)
            throws ServletException, IOException {

        String authHeader = request.getHeader("Authorization");
        if (authHeader == null || !authHeader.startsWith("Bearer ")) {
            filterChain.doFilter(request, response);
            return;
        }

        String token = authHeader.substring(7);
        String username = jwtService.extractUsername(token);

        if (username != null && SecurityContextHolder.getContext().getAuthentication() == null) {
            if (jwtService.isTokenValid(token)) {
                UsernamePasswordAuthenticationToken authToken =
                        new UsernamePasswordAuthenticationToken(username, null, jwtService.getAuthorities(token));
                authToken.setDetails(new WebAuthenticationDetailsSource().buildDetails(request));
                SecurityContextHolder.getContext().setAuthentication(authToken);
            }
        }

        filterChain.doFilter(request, response);
    }
}
```

---

## 🔐 Example `JwtService.java`

```java
package com.damini.authapi.security;

import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import org.springframework.stereotype.Service;
import java.security.Key;
import java.util.*;
import java.util.function.Function;

@Service
public class JwtService {

    private static final String SECRET_KEY = "supersecretkeysupersecretkey123456"; // Use env var in prod!

    private Key getSigningKey() {
        return Keys.hmacShaKeyFor(SECRET_KEY.getBytes());
    }

    public String extractUsername(String token) {
        return extractClaim(token, Claims::getSubject);
    }

    public <T> T extractClaim(String token, Function<Claims, T> claimsResolver) {
        final Claims claims = Jwts.parserBuilder()
                .setSigningKey(getSigningKey())
                .build()
                .parseClaimsJws(token)
                .getBody();
        return claimsResolver.apply(claims);
    }

    public String generateToken(String username, Collection<String> roles) {
        Map<String, Object> claims = new HashMap<>();
        claims.put("roles", roles);
        return Jwts.builder()
                .setClaims(claims)
                .setSubject(username)
                .setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + 1000 * 60 * 60 * 2)) // 2h
                .signWith(getSigningKey(), SignatureAlgorithm.HS256)
                .compact();
    }

    public boolean isTokenValid(String token) {
        try {
            Jwts.parserBuilder().setSigningKey(getSigningKey()).build().parseClaimsJws(token);
            return true;
        } catch (JwtException e) {
            return false;
        }
    }

    public List<String> getAuthorities(String token) {
        Claims claims = Jwts.parserBuilder().setSigningKey(getSigningKey()).build().parseClaimsJws(token).getBody();
        return (List<String>) claims.get("roles");
    }
}
```

---

## 🧱 Example Routes

```java
package com.damini.authapi.controllers;

import com.damini.authapi.security.JwtService;
import org.springframework.web.bind.annotation.*;
import java.util.List;
import java.util.Map;

@RestController
@RequestMapping("/api/auth")
public class AuthController {

    private final JwtService jwtService;

    public AuthController(JwtService jwtService) {
        this.jwtService = jwtService;
    }

    @PostMapping("/login")
    public Map<String, String> login(@RequestBody Map<String, String> req) {
        String username = req.get("username");
        String token = jwtService.generateToken(username, List.of("ROLE_USER"));
        return Map.of("token", token);
    }

    @GetMapping("/validate")
    public Map<String, Object> validate(@RequestHeader("Authorization") String header) {
        String token = header.substring(7);
        return Map.of("valid", jwtService.isTokenValid(token));
    }
}
```

---

## 🧠 Role-Based Access Example

```java
@RestController
@RequestMapping("/api/admin")
public class AdminController {

    @GetMapping("/dashboard")
    @PreAuthorize("hasRole('ADMIN')")
    public String adminDashboard() {
        return "Welcome, admin! 🧠";
    }
}
```

---

## 🧩 TL;DR

| Step | Description |
|------|-------------|
| `@SpringBootApplication` | Scans your components automatically |
| `SecurityConfig` | Defines which routes need auth or roles |
| `JwtAuthFilter` | Extracts + validates token from header |
| `JwtService` | Handles generation + validation |
| `@PreAuthorize` | Restricts access to certain roles |
| `@RestController` | Defines your API endpoints |

---

## 🧪 Test the Flow

```bash
# 1️⃣ Login
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "damini"}'

# Response → {"token": "<JWT>"}

# 2️⃣ Validate
curl http://localhost:8080/api/auth/validate \
  -H "Authorization: Bearer <JWT>"

# 3️⃣ Access protected route
curl http://localhost:8080/api/admin/dashboard \
  -H "Authorization: Bearer <JWT>"
```

---

**✅ Done!**
You now have:
- Automatic route registration  
- JWT-based authentication  
- Role-based access  
- Expandable structure for refresh tokens, revocation, etc.
