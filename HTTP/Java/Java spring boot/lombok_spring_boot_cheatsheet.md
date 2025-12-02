# Lombok + Spring Boot Complete Cheat Sheet

## 🚀 Overview
Lombok is a Java library that removes boilerplate by generating getters, setters, constructors, builders, loggers, and more **at compile time**.  
Spring Boot + Lombok = extremely clean and maintainable code.

This cheat sheet includes **real Spring Boot examples**.

---

# 1. 🔧 Core Lombok Annotations

## @Getter / @Setter
Generates getters and setters.

```java
@Getter
@Setter
public class UserDTO {
    private String username;
    private String email;
}
```

Apply on class or fields.

---

## @ToString
Generates a readable toString() method.

```java
@ToString
public class User {
    private Long id;
    private String name;
}
```

Exclude fields:
```java
@ToString(exclude = "password")
```

---

## @EqualsAndHashCode
Generates equals() and hashCode().

```java
@EqualsAndHashCode(of = "id")
public class User {
    private Long id;
    private String name;
}
```

⚠️ Important:  
**NEVER put @Data on JPA entities** (causes Hibernate issues).

---

# 2. 🏗 Constructors

```java
@NoArgsConstructor
@AllArgsConstructor
@RequiredArgsConstructor
public class User {
    private final String username;
    private String email;
}
```

@RequiredArgsConstructor generates constructor for all `final` fields.

---

# 3. 🧱 Builder Pattern

## @Builder
Perfect for DTOs, services, request/response models.

```java
@Builder
@Getter
public class RegisterRequest {
    private String username;
    private String email;
    private String password;
}
```

Usage:
```java
RegisterRequest req = RegisterRequest.builder()
    .username("adel")
    .email("adel@example.com")
    .password("secret")
    .build();
```

### @Builder.Default
```java
@Builder
public class Player {
    @Builder.Default
    private int xp = 0;
}
```

---

# 4. 🗄 Lombok + JPA Entities

### Clean and correct Spring Boot JPA Entity:

```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Table(name = "books")
public class BookEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;

    private String author;

    @Builder.Default
    private boolean published = false;
}
```

✔ Safe  
✔ Clean  
✔ Hibernate-friendly

---

# 5. 🧠 Lombok in Spring Services

@RequiredArgsConstructor removes @Autowired noise.

```java
@Service
@RequiredArgsConstructor
@Slf4j
public class BookService {

    private final BookRepository repo;

    public BookEntity create(BookEntity book) {
        log.info("Creating book {}", book.getTitle());
        return repo.save(book);
    }
}
```

---

# 6. 🌐 Lombok in REST Controllers

```java
@RestController
@RequiredArgsConstructor
@RequestMapping("/api/books")
@Slf4j
public class BookController {

    private final BookService service;

    @PostMapping
    public ResponseEntity<?> create(@RequestBody CreateBookDTO dto) {
        log.info("POST /api/books {}", dto);
        return ResponseEntity.ok(service.create(dto.toEntity()));
    }
}
```

---

# 7. 📦 DTO Patterns with Lombok

```java
@Getter
@Setter
@Builder
public class CreateBookDTO {
    private String title;
    private String author;

    public BookEntity toEntity() {
        return BookEntity.builder()
            .title(title)
            .author(author)
            .build();
    }
}
```

---

# 8. 📜 Logging with @Slf4j

```java
@Slf4j
public class AuthService {
    public void login(String user) {
        log.info("User {} logged in", user);
    }
}
```

---

# 9. 🛡 Null Validation

```java
public void save(@NonNull String email) {
    // Lombok throws NullPointerException if email is null
}
```

---

# 10. 🧊 Immutable Classes

### @Value = immutable + all-args constructor + getters + equals/hashCode

```java
@Value
@Builder
public class UserResponse {
    Long id;
    String username;
}
```

---

# 11. 🧹 @Cleanup (Auto-close resources)

```java
@Cleanup
FileInputStream in = new FileInputStream("file.txt");
```

Closes automatically.

---

# 12. ⚠ @SneakyThrows

Use carefully — hides checked exceptions.

```java
@SneakyThrows
public void test() {
    throw new IOException("Oops");
}
```

---

# 🎉 End
This cheat sheet covers everything needed for **real-world Spring Boot development** with Lombok.
