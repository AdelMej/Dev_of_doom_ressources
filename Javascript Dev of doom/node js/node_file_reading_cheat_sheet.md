# Node.js File Reading Cheat Sheet (Sync vs Async)

## Core Rule

-   **Sync**: blocks execution, returns values directly
-   **Async**: non-blocking, uses callbacks or Promises

------------------------------------------------------------------------

## Synchronous File Reading

### API

``` js
const fs = require('fs');
fs.readFileSync(path, encoding);
```

### Example

``` js
const fs = require('fs');

function readFileSyncExample(path) {
  try {
    const data = fs.readFileSync(path, 'utf8');
    return data;
  } catch (err) {
    throw Error('Cannot load file');
  }
}
```

### Characteristics

-   Blocks event loop
-   Simple control flow
-   Easy to test
-   Common in school projects

------------------------------------------------------------------------

## Asynchronous File Reading (Callback)

### API

``` js
const fs = require('fs');
fs.readFile(path, encoding, callback);
```

### Example

``` js
const fs = require('fs');

function readFileAsync(path) {
  fs.readFile(path, 'utf8', (err, data) => {
    if (err) {
      console.error(err);
      return;
    }
    console.log(data);
  });
}
```

### Characteristics

-   Non-blocking
-   Callback-based
-   Cannot return values directly

------------------------------------------------------------------------

## Asynchronous File Reading (Promise)

### API

``` js
const fs = require('fs').promises;
fs.readFile(path, encoding);
```

### Example

``` js
const fs = require('fs').promises;

function readFileAsync(path) {
  return fs.readFile(path, 'utf8');
}
```

Usage:

``` js
readFileAsync('file.txt')
  .then(console.log)
  .catch(console.error);
```

------------------------------------------------------------------------

## Async / Await (Recommended Async Style)

``` js
const fs = require('fs').promises;

async function readFileAsync(path) {
  try {
    const data = await fs.readFile(path, 'utf8');
    return data;
  } catch {
    throw Error('Cannot load file');
  }
}
```

------------------------------------------------------------------------

## When to Use What

  Situation             Use
  --------------------- -----------------
  School / grading      Sync
  CLI scripts           Sync
  Servers / APIs        Async
  `.then()` / `await`   Promise / async

------------------------------------------------------------------------

## Golden Rules

-   `readFileSync` → returns data
-   `readFile` → uses callback
-   `async` function → always returns a Promise
-   `.then()` requires a Promise
-   Never mix `import` with `require`

------------------------------------------------------------------------

## Common Errors

❌ Returning from async callback\
❌ Mixing sync and async styles\
❌ Logging errors instead of throwing\
❌ Using async when spec says sync

------------------------------------------------------------------------

## Quick Decision Guide

-   Spec says **synchronous** → `readFileSync`
-   Spec uses `.then()` → return a Promise
-   Spec uses `await` → async function
