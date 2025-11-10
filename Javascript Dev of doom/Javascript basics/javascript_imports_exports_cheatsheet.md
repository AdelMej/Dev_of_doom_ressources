# 🧠 JavaScript Imports & Exports Cheat Sheet

## 📦 Exporting

### 1. **Named Exports**
Export multiple values from a module.

```js
// math.js
export const PI = 3.14159;
export function add(a, b) {
  return a + b;
}
export const multiply = (a, b) => a * b;
```

✅ You can have multiple named exports per file.

---

### 2. **Exporting Inline**
You can also define and export in one go.

```js
export function greet(name) {
  console.log(`Hello, ${name}!`);
}
```

---

### 3. **Export List**
Export existing variables or functions later in one statement.

```js
const name = 'John';
const age = 30;
function sayHi() {
  return `Hi, I'm ${name}`;
}

export { name, age, sayHi };
```

You can also rename while exporting:
```js
export { sayHi as greet };
```

---

### 4. **Default Exports**
Used for exporting a *single main thing* from a file.

```js
// logger.js
export default function log(message) {
  console.log(`[LOG]: ${message}`);
}
```

✅ Only **one default export** per file.

You can also export a value directly:
```js
export default 42;
```

---

## 📥 Importing

### 1. **Import Named Exports**
Import specific exports by name.

```js
import { PI, add } from './math.js';
console.log(add(PI, 2));
```

Rename while importing:
```js
import { add as sum } from './math.js';
```

---

### 2. **Import Everything**
Import all named exports as an object.

```js
import * as math from './math.js';
console.log(math.add(2, 3));
console.log(math.PI);
```

---

### 3. **Import Default Export**
Import the default export (any name you want).

```js
import log from './logger.js';
log('Server started');
```

You can combine default + named:
```js
import log, { PI, add } from './mathAndLog.js';
```

---

### 4. **Dynamic Imports (async)**
Load a module at runtime.

```js
const { add } = await import('./math.js');
console.log(add(2, 3));
```

---

## ⚙️ Notes

- In Node.js, use `.mjs` extension **or** set `"type": "module"` in `package.json` to enable `import/export`.
- In CommonJS (`require`/`module.exports`), use this for compatibility:
  ```js
  // old style
  const math = require('./math.js');
  module.exports = { add, multiply };
  ```
