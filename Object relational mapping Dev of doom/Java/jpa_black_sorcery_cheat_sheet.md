# 🧙‍♂️ THE ULTIMATE BLACK SORCERY JPA CHEAT SHEET

## 1. The Holy Trinity

-   **Lombok** -- removes boilerplate.
-   **JPA** -- maps objects to tables.
-   **Spring Data JPA** -- turns method names into SQL.

## 2. Entity Sorcery (Lombok + JPA)

``` java
@Entity
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private Integer pages;

    @ManyToOne
    private Author author;
}
```

## 3. Repository Black Magic

``` java
public interface BookRepository extends JpaRepository<Book, Long> {}
```

## 4. Method Name Spellbook

### Basic

-   findByName
-   findById

### AND/OR

-   findByNameAndPages
-   findByNameOrAuthor

### Comparison

-   findByPagesLessThan
-   findByPagesGreaterThan
-   findByPagesBetween
-   findByCreatedAtAfter

### LIKE

-   findByNameContaining
-   findByNameStartingWith
-   findByNameEndingWith
-   findByNameIgnoreCase

### Null checks

-   findByAuthorIsNull
-   findByAuthorIsNotNull

### Boolean

-   findByPublishedTrue
-   findByPublishedFalse

### Sorting

-   findByAuthorOrderByPagesAsc
-   findByPagesGreaterThanOrderByNameDesc

### Limits

-   findTop3ByOrderByCreatedAtDesc
-   findFirstByName
-   findFirst10ByAuthor

### Exists

-   existsByName
-   existsByAuthorEmail

### Count

-   countByPublishedTrue
-   countByNameStartingWith

## 5. Auto JOIN Witchcraft

``` java
findByAuthorName(String name)
findByAuthorEmailAndAuthorActiveTrue(String email)
```

## 6. Full Insane Example

``` java
List<Book> findTop10ByAuthorCountryIgnoreCaseAndPublishedTrueAndPagesBetweenOrderByCreatedAtDesc(
    String country,
    int minPages,
    int maxPages
);
```

## 7. Real Controller Example

``` java
@RestController
@RequiredArgsConstructor
public class BookRestController {

    private final BookRepository repo;

    @GetMapping("/books")
    public List<Book> getBooks() {
        return repo.findAll();
    }

    @GetMapping("/books/search")
    public List<Book> search(String name) {
        return repo.findByNameContainingIgnoreCase(name);
    }
}
```
