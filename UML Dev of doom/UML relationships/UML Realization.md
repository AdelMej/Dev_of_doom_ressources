# UML Realization Cheat Sheet

**Realization** represents the relationship between an **interface** and a **class (or component)** that implements it.  
It means the class **guarantees to provide the behavior** defined by the interface.

---

## 🔹 Realization Basics

- Shown as a **dashed line with a hollow triangle arrowhead**.
- The arrow points **to the interface (the contract)**.
- Means: “Class **implements** Interface.”

---

## 🔹 Examples

### 1. Simple Interface Implementation
```
classDiagram
	interface Drivable {
	         +drive()     
	}
	Car ..|> Drivable`
```

✅ Meaning: `Car` implements the `Drivable` interface.

---

### 2. Multiple Interfaces
```
classDiagram
	interface Flyable {
		+fly()
	}

	interface Swimmable {
		+swim()
	}
	Duck ..|> Flyable
	Duck ..|> Swimmable
```

✅ Meaning: `Duck` implements both `Flyable` and `Swimmable`.

---

## 🔹 Realization vs Generalization

- **Generalization (`<|--`)**:
    - Inheritance (child **is a** parent).
    - Reuse of code and attributes.
    - Between classes (or actors in use cases).

- **Realization (`..|>`)**:
    - Implementation of an interface (class **fulfills a contract**).
    - Behavior is promised, not inherited.
    - Between a class/component and an interface.

Example side-by-side:

`classDiagram     Animal <|-- Dog        : is a (generalization)     Car ..|> Drivable      : implements (realization)`

---

## 🔹 Where Realization is Used

- **Class diagrams** → when classes implement interfaces.
- **Component diagrams** → when a component provides an interface.
- **Use case diagrams** → sometimes used to show system behaviors realization.

---

## ✨ Quick Summary

- `..|>` = realization (implements contract).
- Arrow points to the **interface**.
- Use when modeling **interfaces or abstract contracts**.
- Difference from generalization: **realization = implements**, **generalization = inherits**.