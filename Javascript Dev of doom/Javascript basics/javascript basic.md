# 🧠 JavaScript Cheat Sheet (ES6+)

## 📌 Basics

### Variables
```js
let name = "Alice";   // mutable
const age = 25;       // immutable
var oldVar = true;    // avoid in modern JS
```

### Data Types
- **Primitive**: string, number, boolean, null, undefined, symbol, bigint  
- **Reference**: object, array, function

```js
typeof "Hi";      // "string"
typeof 42;        // "number"
typeof null;      // "object" (quirk)
```

### Type Conversion
```js
Number("5");      // 5
String(10);       // "10"
Boolean(0);       // false
```

---

## 🧮 Operators
```js
===   // strict equality
!==   // strict inequality
&&    // logical AND
||    // logical OR
??    // nullish coalescing
?.    // optional chaining
```

---

## 🔁 Control Flow
```js
if (x > 10) console.log("big");
else if (x > 5) console.log("medium");
else console.log("small");

for (let i = 0; i < 5; i++) console.log(i);

while (condition) doSomething();
```

### Switch
```js
switch (fruit) {
  case "apple": console.log("🍎"); break;
  case "banana": console.log("🍌"); break;
  default: console.log("🍇");
}
```

---

## 🧩 Functions

### Declarations
```js
function greet(name) {
  return `Hello ${name}`;
}
```

### Expressions
```js
const add = function(a, b) {
  return a + b;
};
```

### Arrow Functions
```js
const multiply = (a, b) => a * b;
const square = n => n * n;
```

---

## 📦 Objects & Arrays

### Objects
```js
const user = { name: "Bob", age: 30 };
console.log(user.name);
user.country = "FR";
```

### Object Destructuring
```js
const { name, age } = user;
```

### Arrays
```js
const nums = [1, 2, 3];
nums.push(4);
nums.map(n => n * 2);   // [2,4,6,8]
```

### Array Destructuring
```js
const [first, second] = nums;
```

---

## ⚙️ Spread & Rest

```js
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5];

function sum(...args) {
  return args.reduce((a, b) => a + b);
}
```

---

## 🧱 Classes & Inheritance

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    console.log(`${this.name} makes a sound`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks`);
  }
}
```

---

## ⏳ Promises & Async/Await

```js
const fetchData = () =>
  new Promise(resolve => setTimeout(() => resolve("done"), 1000));

fetchData().then(console.log);

async function getData() {
  const result = await fetchData();
  console.log(result);
}
```

---

## 🧮 Useful Array Methods
```js
arr.forEach(x => console.log(x));
arr.filter(x => x > 5);
arr.map(x => x * 2);
arr.reduce((sum, x) => sum + x, 0);
arr.find(x => x === 3);
arr.includes(2);   // true
```

---

## 🧰 JSON
```js
const obj = { a: 1, b: 2 };
const str = JSON.stringify(obj);
const parsed = JSON.parse(str);
```

---

## 🌐 Modules

### Export
```js
export const PI = 3.14;
export default function greet() { console.log("Hi"); }
```

### Import
```js
import greet, { PI } from "./utils.js";
```

---

## 🧭 DOM Basics (Browser)
```js
const btn = document.querySelector("button");
btn.addEventListener("click", () => alert("Clicked!"));
```

---

## 🧠 Tips
- Use `===` for comparison  
- Prefer `const` over `let`  
- Avoid global variables  
- Use arrow functions for callbacks  
- Handle errors with `try/catch`
