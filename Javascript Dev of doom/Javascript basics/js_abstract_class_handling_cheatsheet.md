# JavaScript Abstract Class Handling Cheat Sheet

This cheat sheet covers practical patterns for simulating abstract classes in JavaScript using constructor guards, abstract method stubs, and strict override detection.

---

## ⭐ What Is an Abstract Class?

An abstract class:
- Cannot be instantiated directly  
- Provides a shared API for subclasses  
- May define abstract methods that *must* be overridden  

JavaScript does not support true abstract classes, but we can emulate them cleanly.

---

## 🚫 1. Prevent Direct Instantiation

Use `new.target` to detect if someone calls the class constructor directly.

```js
class Building {
  constructor() {
    if (new.target === Building) {
      throw new Error("Cannot instantiate abstract class Building");
    }
  }
}
```

### Example:
```js
new Building(); // ❌ Error
```

---

## 📢 2. Classic Abstract Method Pattern (Throw When Called)

Define a method that throws unless the subclass overrides it.

```js
evacuationWarningMessage() {
  throw new Error("Class extending Building must override evacuationWarningMessage");
}
```

### Notes:
- No error at instantiation  
- Error appears **only when the method is called**  
- This is the style commonly used in Holberton projects  

---

## 🕵️ 3. Strict Override Enforcement (Throw on Instantiation)

Detect whether the subclass failed to override the method by comparing prototype references.

```js
if (this.evacuationWarningMessage === Building.prototype.evacuationWarningMessage) {
  throw new Error("Subclass must override evacuationWarningMessage");
}
```

### ✔ Detects missing override immediately  
### ✔ Prevents misconfigured objects from existing  

---

## 🛠 4. Full Strict Abstract Class Pattern (Recommended)

```js
class Building {
  constructor() {
    if (new.target === Building) {
      throw new Error("Cannot instantiate abstract class Building");
    }

    if (this.evacuationWarningMessage === Building.prototype.evacuationWarningMessage) {
      throw new Error("Subclass must override evacuationWarningMessage");
    }
  }

  evacuationWarningMessage() {
    throw new Error("Abstract method must be implemented");
  }
}
```

---

## 🧠 5. How to Detect If a Method Has Been Overridden

```js
this.evacuationWarningMessage === Building.prototype.evacuationWarningMessage
```

If this expression is **true**, the subclass did *not* implement the method.

---

## 📝 6. When to Use Each Pattern

| Goal | Best Technique |
|------|----------------|
| Block instantiating abstract class | `new.target` check |
| Enforce method override immediately | prototype comparison |
| Enforce method override only when used | throw inside abstract method |
| Emulate full Java-style abstraction | combine all approaches |

---

## ⚠ 7. Common Pitfalls

### ❌ Not calling the method  
Abstract method throws only when executed.

### ❌ Error swallowed by try/catch  
Example:

```js
try {
  new TestBuilding(200);
} catch (err) {
  console.log(err); // You MUST log it to see it
}
```

### ❌ Typos in method names  
JS won't warn you; it silently treats the method as unimplemented.

---

## 🧩 8. Example Subclass

```js
class OfficeBuilding extends Building {
  evacuationWarningMessage() {
    return "Evacuate immediately!";
  }
}

const office = new OfficeBuilding(200);
console.log(office.evacuationWarningMessage());
```

---

This cheat sheet gives you everything you need to emulate abstract classes safely and cleanly in JavaScript.
