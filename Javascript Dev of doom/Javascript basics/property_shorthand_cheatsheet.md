# JavaScript Property Shorthand Cheat Sheet

## 🔥 What Is Property Shorthand?

Property shorthand lets you create object properties using variable
names **without repeating the key and value**.

------------------------------------------------------------------------

## ✨ Basic Example

### ❌ Without shorthand

``` js
const name = "Alice";
const age = 25;

const user = {
  name: name,
  age: age,
};
```

### ✔️ With shorthand

``` js
const user = { name, age };
```

------------------------------------------------------------------------

## ✔️ When To Use It

### 1. Creating objects from variables

``` js
const x = 10;
const y = 20;

const point = { x, y };
```

### 2. Returning objects from functions

``` js
function createUser(username, email) {
  return { username, email };
}
```

### 3. Building config objects

``` js
const host = "localhost";
const port = 3000;

const config = { host, port };
```

------------------------------------------------------------------------

## 📌 Important Notes

-   Works only when **property name = variable name**.
-   If you want a *different* key name, you must use normal syntax:

``` js
const username = "bob";

const user = {
  name: username,
};
```

------------------------------------------------------------------------

## 🧠 Example With Methods

``` js
const first = "Ada";
const last = "Lovelace";

const person = {
  first,
  last,
  fullName() {
    return `${first} ${last}`;
  },
};
```

------------------------------------------------------------------------

## 📚 Summary Table

  Without Shorthand   With Shorthand
  ------------------- ----------------
  `{ a: a, b: b }`    `{ a, b }`
  Long, repetitive    Clean, modern
  Pre‑ES6 style       ES6+ standard
