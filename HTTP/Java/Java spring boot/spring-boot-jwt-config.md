# ⚙️ Spring Boot JWT Configuration Cheat Sheet

## 🚀 Overview

This guide shows how to **externalize JWT configuration** (secret, expiration, algorithm) using `application.yml` and a `JwtConfig` class.  
It replaces hardcoded values with a clean, flexible setup ready for production.

---

## 🧩 Step 1 — Add Configuration to `application.yml`

📁 `src/main/resources/application.yml`

```yaml
jwt:
  secret: "your-256-bit-secret-key-change-this"
  expiration: 3600000 # 1 hour in milliseconds
  algorithm: HS256
```

> 💡 You can later override these values via **environment variables** or **Kubernetes secrets**.

---

## 🧠 Step 2 — Create a `JwtConfig` Class

📁 `src/main/java/com/damini/authapi/config/JwtConfig.java`

```java
package com.damini.authapi.config;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;

@Configuration
@ConfigurationProperties(prefix = "jwt")
public class JwtConfig {
    private String secret;
    private long expiration;
    private String algorithm;

    public String getSecret() { return secret; }
    public void setSecret(String secret) { this.secret = secret; }

    public long getExpiration() { return expiration; }
    public void setExpiration(long expiration) { this.expiration = expiration; }

    public String getAlgorithm() { return algorithm; }
    public void setAlgorithm(String algorithm) { this.algorithm = algorithm; }
}
```

✅ Automatically binds properties under `jwt:` from `application.yml`  
✅ Can be injected anywhere in the project  

---

## 🔐 Step 3 — Update the `JwtService` to Use Config

📁 `src/main/java/com/damini/authapi/security/JwtService.java`

```java
package com.damini.authapi.security;

import com.damini.authapi.config.JwtConfig;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;
import io.jsonwebtoken.security.Keys;
import org.springframework.stereotype.Service;

import javax.crypto.spec.SecretKeySpec;
import java.security.Key;
import java.util.Date;
import java.util.Map;

@Service
public class JwtService {

    private final JwtConfig jwtConfig;

    public JwtService(JwtConfig jwtConfig) {
        this.jwtConfig = jwtConfig;
    }

    private Key getSigningKey() {
        return new SecretKeySpec(
            jwtConfig.getSecret().getBytes(),
            jwtConfig.getAlgorithm()
        );
    }

    public String generateToken(String username, Map<String, Object> claims) {
        return Jwts.builder()
                .setClaims(claims)
                .setSubject(username)
                .setIssuedAt(new Date(System.currentTimeMillis()))
                .setExpiration(new Date(System.currentTimeMillis() + jwtConfig.getExpiration()))
                .signWith(getSigningKey(), SignatureAlgorithm.forName(jwtConfig.getAlgorithm()))
                .compact();
    }

    public String generateToken(String username) {
        return generateToken(username, Map.of());
    }
}
```

✅ No more hardcoded keys or expiration times  
✅ Algorithm and key can be easily swapped  

---

## 🧪 Step 4 — Test

```bash
curl -X POST "http://localhost:8080/api/auth/token?username=damini"
```

Output (JWT Token):
```
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJkYW1pbmkiLCJleHAiOjE3MjkxMzY0MDB9.ZflYhCB5c8wC5YFw-YcZ8uwEwhA2dtA2x2R2A_rA_7k
```

---

## 🧰 Environment Variable Override (Optional)

You can override `application.yml` values by exporting variables:

```bash
export JWT_SECRET="super-secret-key"
export JWT_EXPIRATION=7200000
export JWT_ALGORITHM=HS512
```

Spring Boot automatically maps them.

---

## 🧱 Best Practices

| Practice | Description |
|-----------|--------------|
| 🔒 Use environment variables for secrets | Never commit real secrets in Git |
| 🔁 Rotate keys periodically | Improves security if a key leaks |
| ⏱️ Keep expiration short | Reduces exposure from stolen tokens |
| 🧩 Centralize config | Easier to maintain and test |

---

## ✅ TL;DR Summary

| Task | Action |
|------|--------|
| Define config | Add `jwt:` section in `application.yml` |
| Bind config | Use `@ConfigurationProperties(prefix = "jwt")` |
| Inject config | Pass `JwtConfig` into `JwtService` |
| Generate token | Use properties dynamically |
| Secure secrets | Load from environment variables |

---

**Next Step →** [JWT Validation & Verification Cheat Sheet (coming next)]()
