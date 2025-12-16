# JavaScript Promise Cheat Sheet

## What is a Promise?
A Promise represents a value that may be available **now**, **later**, or **never**.

States:
- **pending**
- **fulfilled**
- **rejected**

---

## Creating a Promise

```js
const promise = new Promise((resolve, reject) => {
  if (success) {
    resolve(value);
  } else {
    reject(error);
  }
});
```

---

## Resolving / Rejecting

```js
resolve(data);
reject(new Error('Something went wrong'));
```

---

## Consuming a Promise

### then / catch
```js
promise
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.error(error);
  });
```

### finally
```js
promise.finally(() => {
  console.log('Always runs');
});
```

---

## Returning Promises

```js
function doSomething() {
  return Promise.resolve(42);
}
```

```js
function fail() {
  return Promise.reject(new Error('Fail'));
}
```

---

## async / await (Preferred)

```js
async function example() {
  try {
    const result = await promise;
    console.log(result);
  } catch (err) {
    console.error(err);
  }
}
```

---

## Promise Utilities

### Promise.resolve
```js
Promise.resolve(value);
```

### Promise.reject
```js
Promise.reject(error);
```

### Promise.all
Waits for all promises (fails fast)

```js
Promise.all([p1, p2, p3])
  .then(values => {})
  .catch(err => {});
```

### Promise.allSettled
Waits for all (never fails)

```js
Promise.allSettled([p1, p2]);
```

### Promise.race
First settled promise wins

```js
Promise.race([p1, p2]);
```

### Promise.any
First fulfilled promise wins

```js
Promise.any([p1, p2]);
```

---

## Common Mistakes

❌ Forgetting to return a Promise  
❌ Mixing callbacks and Promises  
❌ Not handling errors  
❌ Using await outside async

---

## Mental Model

- Promise = future value
- then = success path
- catch = error path
- await = pause until settled

---

## When to Use Promises

- API calls
- Timers
- File operations
- Async workflows

---

## One-Liner Patterns

```js
await fetch(url).then(r => r.json());
```

```js
const data = await Promise.all(requests);
```

---

## TL;DR

- Promises handle async code
- async/await is syntax sugar
- Always return or await promises
- Handle errors
