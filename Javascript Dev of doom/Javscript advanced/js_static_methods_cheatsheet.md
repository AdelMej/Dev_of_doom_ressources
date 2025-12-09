# JavaScript Static Methods Cheat Sheet

## What Are Static Methods?

Static methods are functions defined **on the class itself**, not on instances of the class.  
They are called **without creating an object**, making them useful for utilities, factories, and helpers.

```js
class MathUtils {
  static add(a, b) {
    return a + b;
  }
}

MathUtils.add(2, 3); // 5
```

---

## Defining Static Methods

```js
class MyClass {
  static myMethod() {
    return "Hello";
  }
}
```

Key point:  
`MyClass.myMethod()` works — but `(new MyClass()).myMethod()` throws an error.

---

## Static Properties

```js
class Config {
  static version = "1.0.0";
}

console.log(Config.version); // "1.0.0"
```

---

## Use Cases for Static Methods

### 1. **Utility Functions**
```js
class Utils {
  static clamp(num, min, max) {
    return Math.min(Math.max(num, min), max);
  }
}

Utils.clamp(10, 0, 5); // 5
```

---

### 2. **Factory Methods**
```js
class User {
  constructor(name) {
    this.name = name;
  }

  static createGuest() {
    return new User("Guest");
  }
}

const guest = User.createGuest();
```

---

### 3. **Parsing and Validation**
```js
class NumberUtils {
  static isPositive(n) {
    return typeof n === "number" && n > 0;
  }
}
```

---

### 4. **Singleton Access**
```js
class DB {
  static instance = null;

  static getInstance() {
    if (!DB.instance) DB.instance = new DB();
    return DB.instance;
  }
}
```

---

## Static Blocks (Advanced)

ES2022 introduced **static initialization blocks**.

```js
class Example {
  static count;

  static {
    this.count = 0;
    console.log("Static block executed!");
  }
}
```

---

## Static vs Instance Method Comparison

| Feature | Static Method | Instance Method |
|--------|---------------|-----------------|
| Called on class | ✔ | ✖ |
| Called on object | ✖ | ✔ |
| Access `this` of instance | ✖ | ✔ |
| Access static properties | ✔ | ✔ |
| Use cases | utilities, factories | object behavior |

---

## Best Practices

- Use static methods for **logic that does not depend on instance data**.  
- Do **not** overuse them; keep them focused.  
- They are great for **pure functions** and **class-wide behavior**.

---

## Quick Examples

### Static Validation
```js
class Validator {
  static isString(v) {
    return typeof v === "string";
  }
}
```

### Static Counter
```js
class Counter {
  static count = 0;

  static increment() {
    this.count++;
  }
}

Counter.increment();
console.log(Counter.count);
```

---

Enjoy your reference! 🚀
