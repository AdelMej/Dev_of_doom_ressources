# 🧱 Spring Boot + JPA ORM Cheat Sheet (Database‑Agnostic, Jakarta Edition)

This cheat sheet shows how to design a clean, database‑agnostic JPA layer for your Spring Boot app (works with H2, PostgreSQL, MariaDB, etc.). Focuses on entities, relationships, repositories, services, transactions, and common best practices.

---

## ✅ Key Dependencies
```xml
<!-- Spring Data JPA -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<!-- JDBC driver: include the one for your DB at runtime (H2, postgresql, mariadb) -->
<!-- Example for Postgres (runtime) -->
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <scope>runtime</scope>
</dependency>
```

> Keep DB-specific driver as a runtime dependency so your code stays DB-agnostic.

---

## 📁 Package Structure (recommended)
```
com.damini.authapi
├─ controllers
├─ services
├─ repositories
├─ models (or entities)
├─ dto
└─ config
```

---

## 🔹 Basic Entity Example (`User`)
```java
package com.damini.authapi.models;

import jakarta.persistence.*;
import java.util.HashSet;
import java.util.Set;

@Entity
@Table(name = "users", indexes = {@Index(columnList = "username"), @Index(columnList = "email")})
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true, length = 50)
    private String username;

    @Column(nullable = false)
    private String password; // store hashed password

    @Column(nullable = false, unique = true)
    private String email;

    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true, fetch = FetchType.LAZY)
    private Set<RefreshToken> refreshTokens = new HashSet<>();

    // constructors, getters, setters
}
```

---

## 🔹 Related Entity Example (`RefreshToken`)
```java
package com.damini.authapi.models;

import jakarta.persistence.*;
import java.time.Instant;

@Entity
@Table(name = "refresh_tokens")
public class RefreshToken {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, length = 512)
    private String tokenHash; // store hashed token

    @Column(nullable = false)
    private Instant expiryDate;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;

    // constructors, getters, setters
}
```

**Notes:** store `tokenHash` (BCrypt/SHA256) not raw token. Use `Instant` for dates.

---

## 🗂️ Repository Interfaces
```java
package com.damini.authapi.repositories;

import com.damini.authapi.models.User;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByUsername(String username);
    boolean existsByEmail(String email);
}
```

```java
package com.damini.authapi.repositories;

import com.damini.authapi.models.RefreshToken;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.Optional;

public interface RefreshTokenRepository extends JpaRepository<RefreshToken, Long> {
    Optional<RefreshToken> findByTokenHash(String tokenHash);
    void deleteAllByUserId(Long userId);
}
```

---

## 🛠️ Service Layer (business logic + transactions)
```java
package com.damini.authapi.services;

import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class AuthService {

    private final UserRepository userRepository;
    private final RefreshTokenRepository refreshTokenRepository;

    public AuthService(UserRepository userRepository, RefreshTokenRepository refreshTokenRepository) {
        this.userRepository = userRepository;
        this.refreshTokenRepository = refreshTokenRepository;
    }

    @Transactional
    public User registerUser(RegisterDto dto) {
        // validate, hash password, save user
    }

    @Transactional
    public void revokeUserTokens(Long userId) {
        refreshTokenRepository.deleteAllByUserId(userId);
    }
}
```

**Notes:** use `@Transactional` on methods that need atomic DB changes. Keep transactions short.

---

## 🔁 Relationship Tips
- **Owning side** controls FK (`@JoinColumn`). In `RefreshToken`, `@ManyToOne` is owning side.
- Use `orphanRemoval=true` + `CascadeType.ALL` on collections when child lifecycle follows parent.
- Prefer `FetchType.LAZY` for collections and `@ManyToOne` to avoid N+1 problems.
- Use DTOs to avoid returning entities (prevents accidental lazy-loading in views).

---

## 📐 Common Patterns
- **DTOs** for requests/responses: map entities → DTO in service layer (use MapStruct or manual mapping).
- **Projections** for lightweight queries: interfaces or `@Query` returning limited fields.
- **Pagination**: `Pageable` + `Page<T>` from `JpaRepository`.

---

## 🔎 Example: Custom Query & Projection
```java
public interface UserSummary {
    String getUsername();
    String getEmail();
}

public interface UserRepository extends JpaRepository<User, Long> {
    @Query("select u.username as username, u.email as email from User u where u.username like %:q%")
    List<UserSummary> searchSummary(String q);
}
```

---

## 🔐 Indexing & Constraints
- Add `@Index` on frequently filtered columns (username, email).
- Use `unique = true` on `@Column` for uniqueness constraints.
- Use DB migrations (Flyway/Liquibase) to apply schema changes in production.

---

## 🧪 Testing Tips
- Use `@DataJpaTest` for repository tests with embedded DB (H2).  
- Use `@Transactional` rollback behavior to isolate tests.

---

## 🔄 Migration to Production DB
- Use the same entities; change JDBC URL + driver and add Flyway migrations.  
- Avoid `ddl-auto: create` in prod — use `validate` or migrations.

---

## ⚡ Performance Tips
- Avoid eager collection fetches. Use `JOIN FETCH` in queries when you need to load relations.
- Batch inserts/updates with `spring.jpa.properties.hibernate.jdbc.batch_size`.
- Use second-level cache only after measuring (Hibernate L2 cache).  

---

## ✅ TL;DR
- Keep entities simple & DB-agnostic.  
- Prefer DTOs for external APIs.  
- Use `@Transactional` in services.  
- Hash tokens/passwords before storing.  
- Use migrations for production schema.  

---

If you want, I can now generate example `User` + `RefreshToken` CRUD endpoints and a small service implementation to pair with this cheat sheet.

