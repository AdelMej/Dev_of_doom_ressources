# 🧪 JUnit 5 & Testing Cheat Sheet (Full Edition)

## 🧠 Core JUnit 5

| Annotation | Description |
|-------------|--------------|
| `@Test` | Marks a method as a test |
| `@BeforeEach` / `@AfterEach` | Runs before/after each test |
| `@BeforeAll` / `@AfterAll` | Runs once per test class (static) |
| `@DisplayName("name")` | Gives a human-readable name |
| `@Disabled("reason")` | Skips test |

### 🧾 Assertions
- `assertEquals(expected, actual)`  
- `assertTrue(condition)` / `assertFalse(condition)`  
- `assertNull(obj)` / `assertNotNull(obj)`  
- `assertThrows(Exception.class, () -> {...})`  
- `assertAll("Group name", () -> {...}, () -> {...})`  

### ⚡ Parameterized Tests
```java
@ParameterizedTest
@ValueSource(strings = {"apple", "banana"})
void testFruits(String fruit) {
    assertNotNull(fruit);
}
```

---

## 🧩 Mockito

| Function | Description |
|-----------|--------------|
| `@Mock` | Create a mock instance |
| `@InjectMocks` | Inject mocks into class under test |
| `when(mock.method()).thenReturn(value)` | Define behavior |
| `verify(mock).method()` | Ensure interaction occurred |
| `reset(mock)` | Clear mock behavior |

### 🧠 Example
```java
when(service.getUser("John")).thenReturn(new User("John"));
verify(service, times(1)).getUser("John");
```

---

## 🌱 Spring Boot Testing

| Annotation | Purpose |
|-------------|----------|
| `@SpringBootTest` | Loads full context |
| `@WebMvcTest(Controller.class)` | Test MVC layer only |
| `@DataJpaTest` | Test repositories with embedded DB |
| `@MockBean` | Replace a bean with a mock |
| `@TestConfiguration` | Custom beans for tests |

### Example
```java
@WebMvcTest(AuthController.class)
class AuthControllerTest {
    @Autowired MockMvc mockMvc;
    @MockBean AuthService authService;
}
```

---

## 🧩 Database Testing

### 🧱 In-Memory DB (H2)
```java
@DataJpaTest
class UserRepositoryTest {
    @Autowired UserRepository repo;
}
```

**application-test.properties**
```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.jpa.hibernate.ddl-auto=create-drop
```

### 🐋 Testcontainers
```java
@Testcontainers
class RepositoryIntegrationTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15");

    @DynamicPropertySource
    static void registerPgProps(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }
}
```

---

## 🌐 API Testing

### Using MockMvc
```java
mockMvc.perform(post("/login")
    .content("{\"user\":\"admin\"}")
    .contentType(MediaType.APPLICATION_JSON))
    .andExpect(status().isOk())
    .andExpect(jsonPath("$.token").exists());
```

### Using TestRestTemplate
```java
ResponseEntity<String> response = restTemplate.getForEntity("/api/health", String.class);
assertEquals(HttpStatus.OK, response.getStatusCode());
```

---

## ⚙️ Best Practices

✅ Clear test naming convention  
✅ No shared mutable state  
✅ One assert per behavior (logical, not literal)  
✅ Keep test data small and focused  
✅ Mock external dependencies only  
✅ Integration tests for end-to-end validation  
