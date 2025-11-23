# Session Manager Library — Architecture Overview

This document describes the architecture, goals, and design decisions for the reusable Session Manager library intended for use across multiple Java/Spring applications.

## 🎯 Goal

Create a **fully modular**, **database‑agnostic**, **configurable**, and **reusable** session-management library that:

- Works with **any database** supported by JPA/Hibernate.
- Uses **Spring Boot auto-configuration** for clean integration.
- Provides **Session** and **SessionRepository** as abstractions.
- Allows **developers to plug their own DB** through JPA configuration.
- Provides **centralized configuration** using `application.yml`.
- Avoids spaghetti code and keeps responsibilities clean.

---

## 🧱 Architecture

### 1. **Session Entity (JPA)**
A simple JPA entity that represents a user session:

- `id` (UUID)
- `userId`
- `createdAt`
- `expiresAt`
- `ipAddress`
- `userAgent`
- `metadata` (optional JSON field)
- `isRevoked`

Because it’s JPA, the database is chosen by the **application**, not the library.

---

### 2. **Repository Layer**
The library defines:

```java
public interface SessionRepository extends JpaRepository<Session, UUID> {}
```

The DB implementation (PostgreSQL, MariaDB, SQLite, H2, etc.) is handled by Spring Boot depending on the app’s JPA configuration.

No spaghetti. No manual DB detection. Zero effort.

---

### 3. **SessionManager Service**

A service responsible for:

- Creating sessions
- Validating sessions
- Checking expiration
- Renewing sessions
- Revoking sessions
- Cleaning expired sessions (optional)

All values come from configuration:

```yaml
session:
  expiration: 30m
  cleanup-enabled: true
  cleanup-interval: 12h
```

---

### 4. **Auto-Configuration**

The library exposes:

```java
@Configuration
@EnableConfigurationProperties(SessionProperties.class)
public class SessionAutoConfiguration {

    @Bean
    public SessionManager sessionManager(SessionRepository repo,
                                         SessionProperties props) {
        return new SessionManager(repo, props);
    }
}
```

If the project includes this library + JPA, the SessionManager becomes available automatically.

---

## ⚙️ Configuration Example (application.yml)

```yaml
session:
  expiration: 45m
  cleanup-enabled: true
  cleanup-interval: 6h
spring:
  datasource:
    url: jdbc:postgresql://localhost/app
    username: postgres
    password: secret
  jpa:
    hibernate:
      ddl-auto: update
```

This lets the developer control everything without touching your library’s code.

---

## 🔌 Database Support

Because the library uses **pure JPA**:

- Add SQLite ⇒ works  
- Add MySQL ⇒ works  
- Add Postgres ⇒ works  
- Add H2 ⇒ works  

If someone wants to add a *new* database logic, they only implement the repository differently or override beans.

---

## 🧩 Extendability

Developers can:

- Override the `SessionRepository`.
- Override the `SessionManager` bean.
- Implement a custom expiration strategy.
- Add new fields to the Session entity.
- Plug a custom cache (Redis, Hazelcast, etc.) later if needed.

---

## 💡 Why This Architecture Is Peak

- No reinventing the wheel.
- Zero coupling to specific DB engines.
- Reusable across any future project.
- Pure Spring Boot conventions.
- Configuration-driven, clean, scalable.

This library becomes **plug & play session management** for any Spring app.

