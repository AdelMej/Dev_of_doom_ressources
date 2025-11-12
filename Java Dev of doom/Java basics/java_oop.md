# 🧠 Java OOP Cheat Sheet

## 🧱 1. Classes and Objects

A **class** is a blueprint for objects; an **object** is an instance of a class.

```java
public class Car {
    String brand;
    int year;

    // Constructor
    public Car(String brand, int year) {
        this.brand = brand;
        this.year = year;
    }

    // Method
    public void drive() {
        System.out.println(brand + " is driving!");
    }
}

// Creating an object
Car car = new Car("Toyota", 2022);
car.drive();
```

---

## 🔒 2. Encapsulation

Encapsulation = data hiding + controlled access using getters and setters.

```java
public class User {
    private String username;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
```

| Modifier | Access Level |
|-----------|---------------|
| `public` | Everywhere |
| `protected` | Same package + subclasses |
| (no modifier) | Same package only |
| `private` | Within same class |

---

## 🧬 3. Inheritance

Inheritance allows a class to acquire properties of another.

```java
class Animal {
    void makeSound() {
        System.out.println("Some sound...");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Woof!");
    }
}
```

Use `super()` to call the parent constructor or methods.

---

## 🧩 4. Polymorphism

### Method Overloading (Compile-time)
Same method name, different parameters.

```java
class MathUtil {
    int add(int a, int b) { return a + b; }
    double add(double a, double b) { return a + b; }
}
```

### Method Overriding (Runtime)
Subclass redefines a superclass method.

```java
class Animal {
    void speak() { System.out.println("Generic animal sound"); }
}

class Cat extends Animal {
    @Override
    void speak() { System.out.println("Meow"); }
}
```

---

## ⚙️ 5. Abstraction

Abstraction hides implementation details using abstract classes or interfaces.

### Abstract Class
```java
abstract class Shape {
    abstract void draw();
}

class Circle extends Shape {
    void draw() { System.out.println("Drawing a circle"); }
}
```

### Interface
```java
interface Flyable {
    void fly();
}

class Bird implements Flyable {
    public void fly() {
        System.out.println("Bird is flying");
    }
}
```

---

## 💡 6. Best Practices

✅ Follow naming conventions (`CamelCase` for classes, `camelCase` for methods/variables).  
✅ Use `@Override` when overriding methods.  
✅ Prefer composition over inheritance when possible.  
✅ Keep fields private and expose behavior via methods.  
✅ Favor interfaces for extensibility.

---

## 🚫 7. Common Pitfalls

❌ Forgetting to initialize `super()` in subclasses with custom constructors.  
❌ Accessing private fields of another class directly.  
❌ Confusing overloading and overriding.  
❌ Forgetting to use `this` for field assignment when names overlap.  

---

**Next Steps:** Learn about `records`, `sealed classes`, and `enums` for modern OOP patterns in Java 17+.
