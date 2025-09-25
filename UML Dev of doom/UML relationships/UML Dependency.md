# UML Dependency Cheat Sheet

A dependency is a **weak relationship** between two classes where one class **temporarily uses or depends on** the other.  
If the supplier changes, the dependent might be affected.

---

## 🔹 Dependency Basics

- Dashed arrow (`..>`) → points from the dependent (client) to the supplier.
- Means “**uses**, but doesn’t own.”
- It’s the **loosest UML relationship** (lighter than association, aggregation, or composition).

---

## 🔹 Example
```
classDiagram
	Teacher ..> Chalk : uses
```

✅ Meaning: A `Teacher` uses `Chalk`, but does not keep it as part of their structure.

---

## 🔹 Typical Cases of Dependency

- Using another class as a **parameter** in a method.
- Using another class as a **local variable**.
- Returning another class from a **method**.
- Temporarily calling another class’s method.

---

## 🔹 Dependency vs Association

- **Dependency (..>)** → temporary, lightweight, not stored as an attribute.
- **Association (--)** → longer-term relationship, usually stored as a field or reference.

Example:
```
classDiagram
    Teacher --> Student : has      %% Association
    Teacher ..> Chalk : uses       %% Dependency`
```

---

## 🔹 Where Dependency is Used

- **Class diagrams** → to show temporary usage.
- **Component diagrams** → to show dependencies between modules.
- **Use case diagrams** → with `<<include>>` or `<<extend>>` relationships (a kind of dependency).

---

## ✨ Quick Summary

- `..>` = dependency (temporary use).
- Arrow points from dependent to supplier.
- Does **not** imply ownership.
- Weaker than association, aggregation, or composition.
- Think of it as **“borrows, not owns.”**