# UML Association Cheat Sheet

An association is a structural relationship between two classes.
It means objects of one class are connected to objects of another.

---

## 🔹 Association Basics

- Plain line (`--`) → bidirectional by default.
- Arrow (`-->`) → unidirectional (navigability: one class knows about the other).
- Two arrows (`<-->`) → explicitly bidirectional (same as plain line, but less common).

## 🔹 Types of Associations
### 1. Unidirectional Association

Only one class knows about the other.
```
classDiagram
    Teacher --> Student : teaches
```

✅ Meaning: Teacher has a reference to Student, but Student does not know Teacher.

---
### 2. Bidirectional Association

Both classes know about each other.
```
classDiagram
    Teacher -- Student : works with
```

✅ Meaning: Teacher has a reference to Student, and Student also knows Teacher.

---
### 3. Explicit Bidirectional (<-->)

Same as plain line, but arrows make it explicit.
```
classDiagram
    Teacher <--> Student
```

✅ Same meaning as above. Usually redundant.

## 🔹 Multiplicity in Associations

- `1` → Exactly one
- `0..1` → Zero or one (optional)
- `*` or `0..*` → Zero or many
- `1..*` → At least one
- `n` → Exactly n
- `n..m` → Between n and m

Example:
```
classDiagram
    Teacher "1" --> "*" Student : teaches
```


✅ Meaning: One teacher teaches many students, and each student has one teacher.

---
## 🔹 Association vs Dependency

- Association → a long-term structural link (usually stored as an attribute).
- Dependency (dashed arrow) → a short-term usage (like passing an object to a method).

Example:
```
classDiagram
    Teacher --> Student : has
    Teacher ..> Chalk : uses
```

✅ Teacher permanently knows Students (association).
✅ Teacher temporarily uses Chalk (dependency).

---
## 🔹 Association in Different Diagrams

- **Class Diagram** → solid line (with/without arrow).
- **Object Diagram** → same, but between specific instances.
- **Use Case Diagram** → actor linked to use case.
- **Sequence Diagram** → implied through messages, not drawn as a line.

## ✨ Quick Summary

- `--` = bidirectional association (default).
- `-->` = unidirectional (navigable).
- `<-->` = explicit bidirectional (same as plain line).
- **Add multiplicity** for "how many" objects are linked.
- Remember: association = "has a reference to."