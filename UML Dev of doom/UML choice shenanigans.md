# UML Part Relationships Cheat Sheet

| Relationship                     | Symbol | Lifetime Dependency               | Ownership          | Example                                            |
| -------------------------------- | ------ | --------------------------------- | ------------------ | -------------------------------------------------- |
| **Association**                  | `--`   | ❌ Independent                     | None               | Teacher -- Student                                 |
| **Aggregation**                  | `o--`  | ❌ Part can exist without whole    | Shared / weak      | Library o-- Book (books can exist outside library) |
| **Composition**                  | `*--`  | ✅ Part destroyed with whole       | Strong / exclusive | House *-- Room (no house → no rooms)               |
| **Generalization (Inheritance)** | `--▷`  | ❌ Subclass exists as its own type | “Is-a”             | Car --▷ Vehicle                                    |
| **Realization**                  | `..▷`  | ❌ Class depends on interface      | “Implements”       | Dog ..▷ AnimalBehavior                             |

---

## 🚀 How to decide

- **Association** → Just a link, no ownership.
- **Aggregation** → Whole _uses_ parts, but parts live on without it.
- **Composition** → Whole _owns_ parts; no whole = no parts.
- **Generalization** → One class is a specialized version of another.
- **Realization** → A class provides the behavior defined by an interface or abstract contract.

---

✨ Quick trick:

- If deleting the whole deletes the part → **Composition**
- If deleting the whole doesn’t delete the part → **Aggregation**
- If it’s just a connection → **Association**
- If it’s a hierarchy → **Generalization**
- If it’s a contract implementation → **Realization**