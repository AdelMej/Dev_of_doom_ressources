# JPA Entity Cheat Sheet – IDs, Columns & Audit Timestamps

This cheat sheet summarizes how to define entities with primary keys, columns, and audit timestamps in JPA/Hibernate.

---

## 1. **Primary Key Strategies**

|Strategy / Type|Example|Notes|
|---|---|---|
|`@Id @GeneratedValue(strategy = GenerationType.IDENTITY)`|`Long id;`|Auto-increment numeric ID, fast, simple, predictable. Good for internal tables.|
|`@Id @GeneratedValue` (UUID)|`UUID id;`|Hibernate 6+ generates random UUID v4 automatically. Non-predictable, secure, good for exposed resources.|
|`@Id @GenericGenerator` (deprecated)|UUID generator for Hibernate <6|Older Spring Boot/Hibernate versions. Avoid in new projects.|

**Tips:**

- Use `Long` for internal tables like users.
- Use `UUID` for devices, sessions, or public/exposed IDs.
- `@GeneratedValue` is enough for UUID in Hibernate 6+.

---

## 2. **Common Column Options**

|Annotation / Attribute|Usage Example|Purpose|
|---|---|---|
|`@Column(nullable = false)`|`private String email;`|Cannot be null in DB|
|`@Column(unique = true)`|`private String email;`|Must be unique|
|`@Column(updatable = false)`|`private LocalDateTime createdAt;`|Cannot be updated after insert|
|`@Column(length = 255)`|`private String email;`|Limits column size (DB constraint)|
|`@Column(columnDefinition = "TEXT")`|`private String longText;`|Custom column type|

---

## 3. **Audit Timestamps**

```java
@Column(nullable = false, updatable = false)
private LocalDateTime createdAt;

@Column(nullable = false)
private LocalDateTime lastModified;

@PrePersist
protected void onCreate() {
    createdAt = LocalDateTime.now();
    lastModified = createdAt;
}

@PreUpdate
protected void onUpdate() {
    lastModified = LocalDateTime.now();
}
```

- `createdAt` → set once on insert
- `lastModified` → updated on every update
- Lifecycle hooks `@PrePersist` and `@PreUpdate` automate this

---

## 4. **Entity Example – User**

```java
@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue
    @Column(updatable = false, nullable = false)
    private UUID id;

    @Column(nullable = false, unique = true, length = 255)
    private String email;

    @Column(nullable = false)
    private String passwordHash;

    @Column(nullable = false, updatable = false)
    private LocalDateTime createdAt;

    @Column(nullable = false)
    private LocalDateTime lastModified;

    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        lastModified = createdAt;
    }

    @PreUpdate
    protected void onUpdate() {
        lastModified = LocalDateTime.now();
    }
}
```

---

## 5. **Entity Example – Device**

```java
@Entity
@Table(name = "devices")
public class Device {

    @Id
    @GeneratedValue
    @Column(updatable = false, nullable = false)
    private UUID id;

    @Column(nullable = false, unique = true)
    private String deviceUuid;

    @Column(nullable = false)
    private String refreshTokenHash;

    @Column(nullable = false, updatable = false)
    private LocalDateTime createdAt;

    @Column(nullable = false)
    private LocalDateTime lastModified;

    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        lastModified = createdAt;
    }

    @PreUpdate
    protected void onUpdate() {
        lastModified = LocalDateTime.now();
    }
}
```

---

## 6. **Key Notes**

- **Use `UUID` for secure, unguessable IDs**, `Long` for simple internal IDs.
- **`@GeneratedValue`** is only needed on primary keys; UUID strategy is automatic in Hibernate 6+.
- **Column attributes** help enforce DB constraints (`nullable`, `unique`, `length`).
- **Audit timestamps** and `@PrePersist/@PreUpdate` keep track of entity creation/modification.
- Keep **lifecycle methods simple**; avoid business logic there.

---

This cheat sheet gives a **complete reference for entity ID types, columns, and auditing** for modern JPA/Hibernate projects.