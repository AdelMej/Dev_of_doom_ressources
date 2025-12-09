# JavaScript Abstract Classes Cheat Sheet

JavaScript does not have built‑in abstract classes like Java or C#, but you can simulate them using inheritance, constructor checks, and required‑override methods.

---

## ⭐ What Is an Abstract Class?

An abstract class is a class that:
- **Cannot be instantiated directly**
- **Defines an API for subclasses**
- **May include abstract methods that must be implemented**

Useful for modeling shared behavior and enforcing structure across subclasses.

---

## 🚫 Prevent Direct Instantiation

Use a constructor check:

```js
class Animal {
  constructor() {
    if (new.target === Animal) {
      throw new Error("Animal is abstract and cannot be instantiated directly.");
    }
  }
}
```

Trying to instantiate:

```js
new Animal(); // ❌ Error
```

Subclassing works:

```js
class Dog extends Animal {}
new Dog(); // ✔
```

---

## 📢 Abstract Methods

Define a method that throws if not overridden:

```js
class Animal {
  makeSound() {
    throw new Error("makeSound() must be implemented by subclass.");
  }
}
```

Subclass must implement it:

```js
class Dog extends Animal {
  makeSound() {
    return "Woof!";
  }
}
```

---

## 📚 Full Example

```js
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error("Shape is abstract.");
    }
  }

  area() {
    throw new Error("Method 'area()' must be implemented.");
  }

  perimeter() {
    throw new Error("Method 'perimeter()' must be implemented.");
  }
}

class Circle extends Shape {
  constructor(r) {
    super();
    this.r = r;
  }

  area() {
    return Math.PI * this.r ** 2;
  }

  perimeter() {
    return 2 * Math.PI * this.r;
  }
}
```

---

## ✨ Why Use Abstract Classes?

- Enforce consistent API across subclasses  
- Share common logic  
- Improve structure and readability  
- Useful in OOP: animals, shapes, models, services, etc.

---

## 🧠 Abstract Class with Static Methods

```js
class Service {
  constructor() {
    if (new.target === Service) {
      throw new Error("Service is abstract.");
    }
  }

  static validateConfig() {
    throw new Error("Subclasses must implement validateConfig().");
  }
}

class MailService extends Service {
  static validateConfig() {
    return true;
  }
}
```

---

## 🛡 Best Practices

- ✔ Always check `new.target` in the constructor  
- ✔ Throw errors in abstract methods  
- ✔ Keep abstract classes focused  
- ✔ Avoid deep inheritance chains  
- ✔ Prefer composition if inheritance feels forced  

---

Enjoy this reference for building clean, expressive JS class hierarchies! 🚀
