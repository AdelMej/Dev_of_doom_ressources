# 🧠 Computed Property Names Cheat Sheet (JavaScript ES6)

Computed property names let you use **expressions as object keys** by wrapping them in `[ ]`.

---

## ✅ Basic Syntax

```js
const key = "username";
const user = {
  [key]: "Alice"
};

console.log(user.username); // "Alice"
```

---

## 🎯 Using Expressions Inside Keys

```js
const prefix = "user";
const id = 42;

const obj = {
  [prefix + id]: "active"
};

console.log(obj.user42); // "active"
```

---

## ⚡ Using Functions to Create Dynamic Keys

```js
function makeKey(base) {
  return base.toUpperCase();
}

const obj = {
  [makeKey("level")]: 10
};

console.log(obj.LEVEL); // 10
```

---

## 📦 Computed Property Names in Methods

```js
const action = "speak";

const dog = {
  [action]() {
    return "woof!";
  }
};

console.log(dog.speak()); // "woof!"
```

---

## 🧩 Useful for Mapping Arrays to Objects

```js
const fields = ["id", "name", "email"];
const values = [1, "Bob", "bob@example.com"];

const obj = {};

fields.forEach((key, i) => {
  obj[key] = values[i];
});

console.log(obj);
// { id: 1, name: 'Bob', email: 'bob@example.com' }
```

---

## 🔁 Using Computed Keys with Spread Operator

```js
const key = "status";

const obj = {
  id: 1,
  [key]: "online",
};

console.log(obj);
// { id: 1, status: 'online' }
```

---

## 🧠 When Computed Property Names Are Useful

- Building objects dynamically
- Converting arrays to objects
- Using variable values as keys
- Creating configuration objects
- Setting keys based on conditions
- Event-driven and reactive systems (e.g., Redux reducers)

---

## 🚫 Common Mistake: Forgetting the brackets

❌ Wrong:

```js
const key = "id";
const obj = {
  key: 123
};
```

✔️ Correct:

```js
const obj = {
  [key]: 123
};
```
