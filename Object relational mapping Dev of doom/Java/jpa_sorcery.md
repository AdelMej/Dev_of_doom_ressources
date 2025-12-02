# JPA Filtering & Pagination Sorcery 🧙‍♂️

Welcome to the dark arts of Spring Data JPA.\
100% real, 0% Lombok, 200% cursed magic.

------------------------------------------------------------------------

## 🔹 Basic Pagination (the sane part)

``` java
Page<User> page = userRepository.findAll(
    PageRequest.of(pageNumber, pageSize, Sort.by("createdAt").descending())
);
```

------------------------------------------------------------------------

## 🔹 Filtering + Pagination via `JpaSpecificationExecutor`

### 1. Add the interface

``` java
public interface UserRepository 
        extends JpaRepository<User, Long>, JpaSpecificationExecutor<User> {
}
```

### 2. Create a Specification

``` java
public class UserSpecs {
    public static Specification<User> hasRole(String role) {
        return (root, query, cb) ->
            role == null ? null : cb.equal(root.get("role"), role);
    }

    public static Specification<User> usernameContains(String text) {
        return (root, query, cb) ->
            text == null ? null : cb.like(cb.lower(root.get("username")), "%" + text.toLowerCase() + "%");
    }
}
```

### 3. Combine them

``` java
Specification<User> spec = Specification
        .where(UserSpecs.hasRole(role))
        .and(UserSpecs.usernameContains(text));
```

### 4. Apply pagination

``` java
Page<User> result = userRepository.findAll(spec,
    PageRequest.of(page, size, Sort.by("createdAt").descending()));
```

------------------------------------------------------------------------

## 🔹 Dynamic Filtering (AKA "why does this work")

``` java
Specification<User> spec = Specification.where(null);

if (role != null) 
    spec = spec.and(UserSpecs.hasRole(role));

if (text != null) 
    spec = spec.and(UserSpecs.usernameContains(text));
```

------------------------------------------------------------------------

## 🔹 Multiple Sort Fields

``` java
Sort sort = Sort.by(
    new Sort.Order(Sort.Direction.ASC, "lastName"),
    new Sort.Order(Sort.Direction.DESC, "createdAt")
);

Page<User> page = repo.findAll(spec, PageRequest.of(page, size, sort));
```

------------------------------------------------------------------------

## 🔹 Pagination + Filtering + Joins (the forbidden spell)

``` java
public static Specification<User> hasGroupName(String groupName) {
    return (root, query, cb) -> {
        Join<User, Group> g = root.join("group", JoinType.LEFT);
        return groupName == null ? null : cb.equal(g.get("name"), groupName);
    };
}
```

------------------------------------------------------------------------

## 🔥 Meme Intermission

**When your API call magically filters, paginates, sorts, joins, and
still runs in 12 ms:**

> "I have no idea what I'm doing but it works."

**Spring Boot devs looking at their 1k-line service class:**

> "It's not a bug, it's a feature-rich configuration pipeline."

**JPA after turning your SQL query into a quantum physics experiment:**

> "Trust me bro."

------------------------------------------------------------------------

## 🔹 Mundane but important: handling empty results

``` java
if (page.getTotalElements() == 0) {
    // return empty response or whatever
}
```

------------------------------------------------------------------------

## 🔹 Filtering with IN clauses

``` java
public static Specification<User> idIn(List<Long> ids) {
    return (root, query, cb) ->
        ids == null ? null : root.get("id").in(ids);
}
```

------------------------------------------------------------------------

## 🔹 Range filtering

``` java
public static Specification<User> createdBetween(LocalDateTime start, LocalDateTime end) {
    return (root, query, cb) ->
        start == null || end == null ? null :
        cb.between(root.get("createdAt"), start, end);
}
```

------------------------------------------------------------------------

## 🔥 Final Notes

✔ Specs are composable\
✔ Null = ignore filter\
✔ Pagination is always clean\
✔ Sorting stacks\
✔ Joins are where things get spicy

You're now certified in **JPA Sorcery Level 3**.\
Proceed carefully.
