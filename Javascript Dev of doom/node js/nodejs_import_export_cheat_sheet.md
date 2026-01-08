# Node.js Import / Export Cheat Sheet

A concise reference for using modules in Node.js (CommonJS and ES Modules).

---

## 1. Module Systems in Node.js

Node.js supports **two module systems**:

| System | Syntax | Default |
|-----|------|------|
| CommonJS | `require / module.exports` | ✅ Yes |
| ES Modules | `import / export` | ❌ Opt-in |

---

## 2. CommonJS (Most Holberton / backend projects)

### Export a Value
```js
module.exports = app;
```

### Export Multiple Values
```js
module.exports = {
  app,
  server,
};
```

### Import a Module
```js
const app = require('./app');
```

### Import Specific Properties
```js
const { app, server } = require('./app');
```

---

## 3. Exporting Functions

```js
function sum(a, b) {
  return a + b;
}

module.exports = sum;
```

```js
const sum = require('./sum');
```

---

## 4. Exporting Classes

```js
class User {
  constructor(name) {
    this.name = name;
  }
}

module.exports = User;
```

```js
const User = require('./User');
```

---

## 5. Exporting an Object

```js
module.exports = {
  name: 'API',
  version: '1.0.0',
};
```

```js
const config = require('./config');
```

---

## 6. CommonJS Gotchas

### ❌ Overwriting exports incorrectly
```js
exports = app; // WRONG
```

### ✅ Correct way
```js
module.exports = app;
```

### Using exports shortcut (safe way)
```js
exports.app = app;
exports.port = 1245;
```

---

## 7. ES Modules (Modern Node.js)

### Enable ES Modules
In `package.json`:
```json
{
  "type": "module"
}
```

OR use `.mjs` files.

---

## 8. ES Module Export

### Named Export
```js
export const app = {};
export function start() {}
```

### Default Export
```js
export default app;
```

---

## 9. ES Module Import

### Named Import
```js
import { app, start } from './app.js';
```

### Default Import
```js
import app from './app.js';
```

---

## 10. Mixing CommonJS & ES Modules

### Import CommonJS into ES Module
```js
import pkg from './legacy.cjs';
```

### Import ES Module into CommonJS
```js
const app = await import('./app.mjs');
```

---

## 11. Node.js Built-in Modules

```js
const http = require('http');
const fs = require('fs');
const path = require('path');
```

ESM:
```js
import http from 'http';
import fs from 'fs';
```

---

## 12. Best Practices

- Use **CommonJS** unless ES Modules are required
- Match file extensions explicitly in ES Modules
- Export **exact names expected by tests**
- One module = one responsibility

---

## 13. Typical Holberton Pattern

```js
const http = require('http');

const app = http.createServer();

app.listen(1245);

module.exports = app;
```

---

## Quick Summary

| Task | CommonJS |
|----|----|
| Export one thing | `module.exports = value` |
| Export many | `module.exports = { a, b }` |
| Import | `require('./file')` |
| Destructure | `const { a } = require('./file')` |

---

## Author Notes

- Stick to CommonJS for school projects
- Be strict with naming & exports
- Avoid mixing systems unless needed
