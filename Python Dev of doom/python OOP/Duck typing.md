# 🦆 Duck Typing in Python

### What is Duck Typing?

- Comes from the saying:
    > “If it walks like a duck and quacks like a duck, it’s a duck.”

- In Python, it means:
    - **The actual type/class of an object doesn’t matter**.
    - **What matters is whether the object has the methods/attributes you need.**

---

### Key Points

- Python relies on **behavior**, not explicit type checks.
- Flexible and dynamic: no need for interfaces or ABCs.
- Risk: runtime errors if the object doesn’t implement the expected methods.

---

### Example

```python
class Duck:
    def quack(self):
        print("Quack!")

class Person:
    def quack(self):
        print("I can quack too!")

def make_it_quack(thing):
    thing.quack()  # just assumes quack exists

d = Duck()
p = Person()

make_it_quack(d)  # Quack!
make_it_quack(p)  # I can quack too!

```

- No `isinstance()` checks needed.
- Works for **any object** that implements `.quack()`.

---

### Duck Typing vs Strict Typing (ABCs)

|Feature|Duck Typing|ABC / Type Hints|
|---|---|---|
|Flexibility|✅ Works with anything that has the right behavior|❌ Must inherit / match type hints|
|Enforcement|❌ Only at runtime (may fail if method missing)|✅ Static checking / enforced contracts|
|Best for|Quick scripts, small projects, dynamic behavior|Large projects, libraries, multiple contributors|

---

### Best Practices

- Use duck typing for **thin interfaces**: just assume the methods exist.
- If the codebase grows, or multiple devs are involved, consider **ABCs or type hints** to formalize contracts.