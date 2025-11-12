# ☕ Java Basics Cheat Sheet

A quick summary of core Java syntax and concepts for beginners.

---

## 🧩 1. Structure of a Java Program

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, Java!");
    }
}
```

---

## ⚙️ 2. Variables and Data Types

```java
int age = 25;
double price = 19.99;
char grade = 'A';
boolean isActive = true;
String name = "Alice";
```

---

## 🔁 3. Operators

```java
int a = 10, b = 5;

System.out.println(a + b);  // Addition
System.out.println(a - b);  // Subtraction
System.out.println(a * b);  // Multiplication
System.out.println(a / b);  // Division
System.out.println(a % b);  // Modulo
```

---

## 🔄 4. Control Flow

**If / Else**
```java
if (age >= 18) {
    System.out.println("Adult");
} else {
    System.out.println("Minor");
}
```

**Switch**
```java
switch (grade) {
    case 'A': System.out.println("Excellent"); break;
    case 'B': System.out.println("Good"); break;
    default: System.out.println("Try harder");
}
```

**Loops**
```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}

int x = 0;
while (x < 5) {
    System.out.println(x);
    x++;
}
```

---

## 📦 5. Arrays

```java
int[] nums = {1, 2, 3, 4, 5};
System.out.println(nums[0]); // 1

for (int n : nums) {
    System.out.println(n);
}
```

---

## 🧱 6. Methods

```java
public static int add(int a, int b) {
    return a + b;
}

public static void main(String[] args) {
    int result = add(5, 3);
    System.out.println(result);
}
```

---

## 🧠 7. Classes and Objects

```java
public class User {
    String name;
    int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void greet() {
        System.out.println("Hello, " + name);
    }
}

public class Main {
    public static void main(String[] args) {
        User user = new User("Alice", 22);
        user.greet();
    }
}
```

---

## 🧩 8. Inheritance

```java
class Animal {
    void speak() { System.out.println("Animal sound"); }
}

class Dog extends Animal {
    void speak() { System.out.println("Woof!"); }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.speak(); // Woof!
    }
}
```

---

## 💥 9. Exceptions

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Error: " + e.getMessage());
} finally {
    System.out.println("Done.");
}
```

---

## 📚 10. Common Utilities

```java
import java.util.*;

List<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");

for (String n : names) System.out.println(n);

Map<String, Integer> scores = new HashMap<>();
scores.put("Alice", 90);
scores.put("Bob", 85);
```

---

## 🕒 11. Date and Time

```java
import java.time.LocalDateTime;

LocalDateTime now = LocalDateTime.now();
System.out.println(now);
```

---

## 🧮 12. Input from User

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);
System.out.print("Enter your name: ");
String name = sc.nextLine();
System.out.println("Hello, " + name);
```

---

## 🧰 13. Tips

- File name = class name (with `public class`)
- Main method: `public static void main(String[] args)`
- Everything in Java must be inside a class
- Use `System.out.println()` for output
- End every statement with `;`
- Use camelCase for variables and methods

---

🧩 *Made for quick learning — covers 90% of what you’ll use daily.*
