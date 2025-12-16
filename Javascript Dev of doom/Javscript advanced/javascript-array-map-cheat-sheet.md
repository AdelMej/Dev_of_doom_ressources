# JavaScript Array.map() Cheat Sheet

> `Array.prototype.map()` transforms arrays by applying a function to each element.

---

## Basic Syntax
```js
const newArray = array.map((element, index, array) => {
  return transformedElement;
});
```

Short form:
```js
const newArray = array.map(el => el * 2);
```

---

## Simple Examples

### Multiply numbers
```js
[1, 2, 3].map(n => n * 2);
// [2, 4, 6]
```

### Convert strings to numbers
```js
['1', '2', '3'].map(Number);
// [1, 2, 3]
```

### Uppercase strings
```js
['a', 'b'].map(s => s.toUpperCase());
// ['A', 'B']
```

---

## Mapping Objects

### Extract a property
```js
users.map(user => user.name);
```

### Transform objects
```js
users.map(user => ({
  id: user.id,
  isAdmin: user.role === 'admin',
}));
```

---

## Using index

```js
['a', 'b', 'c'].map((value, index) => `${index}: ${value}`);
// ['0: a', '1: b', '2: c']
```

---

## Chaining

```js
numbers
  .map(n => n * 2)
  .map(n => n + 1);
```

Common chain:
```js
items
  .filter(item => item.active)
  .map(item => item.name);
```

---

## map vs forEach

| map | forEach |
|---|---|
| Returns new array | Returns undefined |
| Functional | Side effects |
| Chainable | Not chainable |

---

## map vs for loop

```js
const result = [];
for (let i = 0; i < arr.length; i++) {
  result.push(arr[i] * 2);
}
```

Equivalent:
```js
const result = arr.map(x => x * 2);
```

---

## Common Mistakes

### Forgetting return
```js
array.map(x => {
  x * 2; // ❌
});
```

Correct:
```js
array.map(x => {
  return x * 2;
});
```

Or:
```js
array.map(x => x * 2);
```

---

## Skipping values (use filter)

❌ Not valid:
```js
array.map(x => {
  if (x > 0) return x;
});
```

✅ Correct:
```js
array.filter(x => x > 0).map(x => x);
```

---

## Async map (gotcha)

```js
const results = await Promise.all(
  items.map(async item => {
    return await fetchData(item);
  })
);
```

---

## When to Use map()

✅ Transform data  
✅ Create new arrays  
❌ Side effects  
❌ Async without Promise.all  

---

## Key Takeaways

- `map()` always returns a new array
- Original array is not modified
- Same length in, same length out
