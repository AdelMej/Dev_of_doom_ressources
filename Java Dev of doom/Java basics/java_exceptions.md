# 🛑 Java Exceptions & Error Handling Cheat Sheet

Quick reference for Java exception handling (Java 17+).

---

## 🛠️ 1. Basics of Exceptions

**try / catch / finally**
```java
try {
    int x = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Always executes");
}
```

**throw vs throws**
```java
void riskyMethod() throws IOException { // declares checked exception
    if (true) throw new IOException("Something went wrong");
}

try {
    riskyMethod();
} catch (IOException e) {
    e.printStackTrace();
}
```

**Checked vs Unchecked**
- Checked: `IOException`, `SQLException` (must declare or catch)  
- Unchecked: `RuntimeException` subclasses like `NullPointerException`, `IllegalArgumentException`  

---

## ⚠️ 2. Common Exceptions

| Exception | When it occurs | Example |
|-----------|----------------|---------|
| NullPointerException | Accessing null object | `String s = null; s.length();` |
| IllegalArgumentException | Invalid arguments | `Integer.parseInt("abc");` |
| IndexOutOfBoundsException | Invalid index | `int[] arr = {1}; arr[5];` |
| IOException | I/O errors | `new FileReader("nofile.txt");` |
| FileNotFoundException | File not found | `new FileInputStream("nofile.txt");` |
| SQLException | DB errors | `Statement.executeQuery("bad SQL");` |

---

## ✨ 3. Custom Exceptions

**Extending Exception**
```java
class MyException extends Exception {
    public MyException(String msg) { super(msg); }
}
```

**Extending RuntimeException**
```java
class MyRuntimeException extends RuntimeException {
    public MyRuntimeException(String msg) { super(msg); }
}
```

**Usage**
```java
throw new MyException("Oops");
throw new MyRuntimeException("Oops runtime");
```

---

## 🔄 4. Exception Handling Patterns

**Multi-catch**
```java
try {
    riskyMethod();
} catch (IOException | SQLException e) {
    e.printStackTrace();
}
```

**Try-with-resources**
```java
try (BufferedReader br = Files.newBufferedReader(Path.of("file.txt"))) {
    System.out.println(br.readLine());
} catch (IOException e) {
    e.printStackTrace();
}
```

**Re-throwing / wrapping**
```java
try {
    riskyMethod();
} catch (IOException e) {
    throw new RuntimeException("Wrapped exception", e);
}
```

**Logging exceptions**
```java
Logger logger = Logger.getLogger("MyLogger");
try {
    riskyMethod();
} catch (Exception e) {
    logger.severe(e.toString());
}
```

---

## 💡 5. Best Practices

- Prefer unchecked exceptions for programming errors.  
- Use meaningful messages in exceptions.  
- Avoid empty `catch` blocks — always handle or log.  
- Don’t catch `Throwable` unless absolutely necessary.  
- Wrap exceptions when adding context before re-throwing.  
- Use try-with-resources for I/O to prevent leaks.  

---

*Made for quick reference — Java 17+ exception handling idioms and practical examples.*
