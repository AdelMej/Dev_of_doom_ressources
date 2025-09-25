# UML Composition Cheat Sheet

Composition is a special type of association that represents a **strong whole–part relationship**. 
The part **cannot exist independently** of the whole.

---

## 🔹 Composition Basics

- Black diamond (`*--`) → placed at the “whole” side.
- Stronger than aggregation: parts are **owned** and destroyed with the whole.
- Indicates **exclusive lifetime dependency**.

---

## 🔹 Example
```
classDiagram
	House *-- Room : contains
```

✅ Meaning: A `House` is made of `Rooms`. If the `House` is destroyed, the `Rooms` cease to exist.

---

## 🔹 Multiplicity in Composition

- Works the same way as in associations and aggregation.
```
classDiagram
	Car "1" *-- "4" Wheel : has
```

✅ Meaning: A `Car` must have 4 `Wheels`, and those `Wheels` exist only as part of that `Car`.

---

## 🔹 Composition vs Aggregation

- **Aggregation (o--)** → whole–part, but parts live independently (Library–Books).
- **Composition (*--)** → whole–part, but parts **die with the whole** (House–Rooms).

---

## 🔹 Composition vs Association

- **Association** → just a connection (“knows about”).
- **Composition** → strong ownership + lifetime dependency.

---

## 🔹 Where Composition is Used

- **Class diagrams** → to show tightly bound whole–part structures.
- Rare in other UML diagrams.

---

## ✨ Quick Summary

- `*--` = composition (whole–part, dependent).
- Diamond is on the **whole** side.
- Parts are **owned exclusively** and destroyed with the whole.
- Strongest form of association.