# UML Aggregation Cheat Sheet

Aggregation is a special type of association that represents a **whole–part relationship**.  
The part can exist **independently** of the whole.

---

## 🔹 Aggregation Basics

- White diamond (`o--`) → placed at the “whole” side.
- It’s still an **association**, but with the “whole–part” meaning.
- Parts can live on their own (not tightly bound).

---

## 🔹 Example
```
classDiagram
     Team o-- Player : has
```

✅ Meaning: A `Team` is made of `Players`, but `Players` exist without the `Team`.

---

## 🔹 Multiplicity in Aggregation

- Multiplicity works the same way as normal associations.

```
classDiagram
     Library "1" o-- "0..*" Book : contains
```

✅ Meaning: A library contains many books, but books can exist without a library.

---

## 🔹 Aggregation vs Association

- **Association** → just a connection (“knows about”).
- **Aggregation** → whole–part relationship, but **parts survive** if the whole is deleted.

---

## 🔹 Aggregation vs Composition

- **Aggregation (o--)** → parts can live without the whole (Library–Books).
- **Composition (*--)** → parts depend on the whole; if the whole dies, parts die too (House–Rooms).

---

## 🔹 Where Aggregation is Used

- **Class diagrams** → common for modeling whole–part.
- Rarely seen in other UML diagrams.

---

## ✨ Quick Summary

- `o--` = aggregation (whole–part, independent).
- Diamond is on the **whole** side.
- Multiplicity shows “how many parts”.
- Difference from association: adds **whole–part semantics**.
- Difference from composition: parts can live independently.