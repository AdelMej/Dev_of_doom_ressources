# JPA Relationships Cheat Sheet

This cheat sheet covers the main JPA relationships, their usage, and example code for clarity.

---

## **1. One-to-One (`@OneToOne`)**

- **Description:** One entity is associated with exactly one other entity.
    
- **Use Case:** User ↔ Profile, Employee ↔ ParkingSpot.
    
- **Owning Side:** The entity with the foreign key column.
    

**Example: User → Profile**

```java
@Entity
public class User {
    @Id @GeneratedValue
    private Long id;

    @OneToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "profile_id", referencedColumnName = "id")
    private Profile profile;
}

@Entity
public class Profile {
    @Id @GeneratedValue
    private Long id;
}
```

- `cascade = CascadeType.ALL` → propagate persist/remove operations.
    

---

## **2. One-to-Many (`@OneToMany`)**

- **Description:** One entity has many related entities.
- **Use Case:** User → Devices, Order → OrderItems.
- **Mapped By:** Specifies the field in the child entity that owns the relationship.

**Example: User → Devices**

```java
@Entity
public class User {
    @Id @GeneratedValue
    private UUID id;

    @OneToMany(mappedBy = "user", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Device> devices = new ArrayList<>();
}

@Entity
public class Device {
    @Id @GeneratedValue
    private UUID id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id", nullable = false)
    private User user;
}
```

- `orphanRemoval = true` → removing a device from the list deletes it from the DB.
    
- Always maintain **both sides** of the relationship.
    

---

## **3. Many-to-One (`@ManyToOne`)**

- **Description:** Many child entities belong to one parent entity.
- **Use Case:** Device → User, OrderItem → Order.

**Example: Device → User** (same as above)

```java
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "user_id", nullable = false)
private User user;
```

- Usually the **owning side** of a relationship (holds the foreign key).

---

## **4. Many-to-Many (`@ManyToMany`)**

- **Description:** Many entities relate to many entities.
- **Use Case:** Users ↔ Roles, Students ↔ Courses.
- **Join Table:** Required to store relationships.

**Example: Users ↔ Roles**

```java
@Entity
public class User {
    @Id @GeneratedValue
    private Long id;

    @ManyToMany
    @JoinTable(
        name = "user_roles",
        joinColumns = @JoinColumn(name = "user_id"),
        inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles = new HashSet<>();
}

@Entity
public class Role {
    @Id @GeneratedValue
    private Long id;

    @ManyToMany(mappedBy = "roles")
    private Set<User> users = new HashSet<>();
}
```

- `mappedBy` → defines the non-owning side of the relationship.
- Use `Set` to avoid duplicates.

---

## **5. Key Notes / Best Practices**

1. **Owning Side:** The entity with `@JoinColumn` owns the relationship; only it should be persisted.
2. **Bidirectional Relationships:** Always maintain both sides in code to avoid inconsistency.
3. **Fetch Type:**
    - `LAZY` → load only when needed (default for `@ManyToOne` / `@OneToMany`)
    - `EAGER` → load immediately (default for `@OneToOne` / `@ManyToMany`)

4. **Cascading:** Use `CascadeType.ALL` carefully; not always desired for deletes.
5. **Orphan Removal:** Deletes child entity if removed from collection.


---

This cheat sheet gives a **quick reference** for all JPA relationship types, their usage, and examples. Perfect for building entities like your `User` and `Device` for auth systems.