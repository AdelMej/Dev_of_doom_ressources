# JavaScript Class Getters & Setters Cheat Sheet

## 📘 Overview

Getters and setters let you control how properties are **read** and
**written** inside a class.\
They allow validation, computed values, encapsulation, and cleaner APIs.

------------------------------------------------------------------------

# 🟦 Basic Syntax

## Getter

``` js
get propertyName() {
  return this._propertyName;
}
```

## Setter

``` js
set propertyName(value) {
  this._propertyName = value;
}
```

------------------------------------------------------------------------

# 🟩 Full Example

``` js
class Student {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (typeof value !== "string") {
      throw new TypeError("Name must be a string");
    }
    this._name = value;
  }
}

const s = new Student("Bob");
console.log(s.name); // Bob
s.name = "Alice";    // setter used
```

------------------------------------------------------------------------

# 🟧 Why Underscore Variables (`_name`)?

Because getters and setters use the *same property name*.\
The actual stored value must be different to avoid recursion.

Example of infinite recursion ❌:

``` js
set name(value) {
  this.name = value; // ❌ calls itself forever
}
```

Correct version:

``` js
set name(value) {
  this._name = value; // ✔ safe
}
```

------------------------------------------------------------------------

# 🟥 Adding Validation

``` js
set age(value) {
  if (!Number.isInteger(value) || value < 0) {
    throw new Error("Invalid age");
  }
  this._age = value;
}
```

------------------------------------------------------------------------

# 🟪 Computed Getters

``` js
get fullName() {
  return `${this.first} ${this.last}`;
}
```

------------------------------------------------------------------------

# 🟨 Read‑Only Properties

Just define a **getter** without a setter:

``` js
get id() {
  return this._id;
}
```

------------------------------------------------------------------------

# 🟫 Writing‑Only Properties

Rare but possible --- setter only:

``` js
set password(value) {
  this._hashed = hash(value);
}
```

------------------------------------------------------------------------

# 🔧 Define Getters/Setters Outside the Class

``` js
Object.defineProperty(obj, "score", {
  get() { return this._score; },
  set(v) { this._score = v; }
});
```

------------------------------------------------------------------------

# 🧠 Checking for Getters/Setters

``` js
Object.getOwnPropertyDescriptor(obj, "prop");
```

------------------------------------------------------------------------

# 🚀 Quick Reference Table

  Feature                                 Getter   Setter
  --------------------------------------- -------- --------
  Read value                              ✔        ❌
  Write value                             ❌       ✔
  Works like a property                   ✔        ✔
  Allows validation                       ✔        ✔
  Looks like method but used like field   ✔        ✔

------------------------------------------------------------------------

# 🏁 Best Practices

-   Use underscores (`_value`) for backing fields\
-   Validate everything in setters\
-   Keep getters pure and side‑effect‑free\
-   Avoid heavy logic in getters (no async, no slow loops)\
-   Use getters for computed properties and API cleanliness
