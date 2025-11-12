# 🧾 Java File I/O Cheat Sheet

## 📁 Basic File Operations (`java.io.File`)
```java
import java.io.File;

File file = new File("example.txt");

// Check existence
if (file.exists()) {
    System.out.println("Exists: " + file.getAbsolutePath());
}

// Create new file
file.createNewFile();

// Delete file
file.delete();
```

---

## ✍️ Writing to Files
```java
import java.io.FileWriter;
import java.io.IOException;

try (FileWriter writer = new FileWriter("output.txt")) {
    writer.write("Hello, World!");
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## 📖 Reading from Files
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

try (BufferedReader reader = new BufferedReader(new FileReader("output.txt"))) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
}
```

---

## ⚡ Modern File I/O (`java.nio.file`)
```java
import java.nio.file.*;
import java.io.IOException;

Path path = Paths.get("example.txt");
String content = Files.readString(path);
Files.writeString(path, "New content", StandardOpenOption.APPEND);
```

---

## 🧩 Reading All Lines
```java
List<String> lines = Files.readAllLines(Path.of("example.txt"));
lines.forEach(System.out::println);
```

---

## 📦 Object Serialization
```java
import java.io.*;

class User implements Serializable {
    String name;
    int age;
}

User u = new User();
u.name = "Alice";
u.age = 25;

// Serialize
try (ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("user.ser"))) {
    out.writeObject(u);
}

// Deserialize
try (ObjectInputStream in = new ObjectInputStream(new FileInputStream("user.ser"))) {
    User restored = (User) in.readObject();
}
```

---

## 🧰 Try-with-Resources
```java
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    System.out.println(br.readLine());
} catch (IOException e) {
    e.printStackTrace();
}
```

---

## 🪄 Handy Utilities
```java
Files.copy(Path.of("a.txt"), Path.of("b.txt"), StandardCopyOption.REPLACE_EXISTING);
Files.move(Path.of("a.txt"), Path.of("folder/a.txt"), StandardCopyOption.REPLACE_EXISTING);
Files.deleteIfExists(Path.of("old.txt"));
```
