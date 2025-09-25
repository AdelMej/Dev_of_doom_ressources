# UML Generalization Cheat Sheet

**Generalization** represents an **inheritance** (or “is-a”) relationship between classes.  
It means one class (the child) inherits the properties and behavior of another (the parent).

---

## 🔹 Generalization Basics

- Shown as a **solid line with a hollow triangle arrowhead**.
- The arrow points **to the parent (superclass)**.
- Means: “Subclass **is a** Superclass.”

---

## 🔹 Examples

### 1. Simple Inheritance
```
classDiagram
     Animal <|-- Dog
```

✅ Meaning: `Dog` is a subclass of `Animal`.

---

### 2. Multiple Subclasses
```
classDiagram 
	Animal <|-- Dog
	Animal <|-- Cat
```

✅ Meaning: `Dog` and `Cat` are both subclasses of `Animal`.

---

### 3. Inheritance Chain
```
classDiagram
	LivingBeing <|-- Animal
    Animal <|-- Dog
```

✅ Meaning: `Dog` inherits from `Animal`, which inherits from `LivingBeing`.

---

## 🔹 Generalization vs Association

- **Generalization**:
    - “is a” relationship
    - Class inheritance
    - Arrow with hollow triangle

- **Association**:
    - “has a” or “uses a” relationship        
    - Objects linked
    - Plain line (with/without arrows)

Example side-by-side:
```
classDiagram
    Teacher --> Student : has
    Animal <|-- Dog : is a
```

---
## Where Generalization is Used

- **Class diagrams** → inheritance between classes.
- **Use case diagrams** → inheritance between actors.
- **(Sometimes) Component/Package diagrams** → specialization of components/packages.
- ❌ Not used in sequence, activity, or state diagrams.

---
## 🔹 Quick Summary

- Use **generalization** for **inheritance hierarchies**.
- Arrow: `Child <|-- Parent` (points to the more general class).
- It’s different from association: _association = “knows/has,” generalization = “is a.”_