# ML Multiplicity Cheat Sheet

Multiplicity defines how many objects of one class can be associated with objects of another class.

### 🔹 Common Notations
| Notation      | Meaning                | Example                                          |
| ------------- | ---------------------- | ------------------------------------------------ |
| `1`           | Exactly one            | A `Person` has **1 birth certificate**           |
| `0..1`        | Zero or one (optional) | A `Person` may or may not have a `DriverLicense` |
| `*` or `0..*` | Zero or many           | A `Teacher` can teach **any number of Students** |
| `1..*`        | At least one           | A `Team` must have **one or more Players**       |
| `n`           | Exactly *n*            | A `Car` has **4 Wheels**                         |
| `n..m`        | Between *n* and *m*    | A `DiceGame` has **2 to 6 Players**              |

---
## 1. One-to-One (`1 — 1`)
```
+-------------+ 1      1 +-------------+
|   Person    | --------> |  Passport  |
+-------------+           +-------------+
```

Each person has exactly one passport.

---
### 2. One-to-Many (`1 — *`)
```
+-------------+ 1      * +-------------+
|   Teacher   | --------> |  Student   |
+-------------+           +-------------+
```

One teacher can have many students. Each student has one teacher.

---
### 3. Many-to-Many (`* — *`)
```
+-------------+ *      * +-------------+
|   Teacher   | --------> |  Student   |
+-------------+           +-------------+
```

A teacher can have many students, and a student can have many teachers.

---

### 4. Optional (`0..1 — *`)
```
+-------------+ 0..1   * +-------------+
|   Manager   | --------> |  Employee  |
+-------------+           +-------------+
```


A manager may exist (0..1), and can manage many employees.

### 5. Fixed Number (`1 — n`)
```
+-------------+ 1      4 +-------------+
|    Car      | --------> |   Wheel    |
+-------------+           +-------------+
```

Every car has exactly 4 wheels.

---
### 6. Range (n..m)
```
+-------------+ 2..6   1 +-------------+
|   DiceGame  | --------> |   Player   |
+-------------+           +-------------+
```

A dice game must have between 2 and 6 players, and each player belongs to exactly one game.