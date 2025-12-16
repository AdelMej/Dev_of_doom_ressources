# JavaScript Array.reduce() Cheat Sheet

> `Array.prototype.reduce()` reduces an array to a single value.

---

## Basic Syntax
```js
const result = array.reduce((accumulator, current, index, array) => {
  return accumulator;
}, initialValue);
```

Short form:
```js
const result = array.reduce((acc, cur) => acc + cur, 0);
```

---

## How reduce Works

- `accumulator` → value returned from previous iteration  
- `current` → current array element  
- `initialValue` → starting value for accumulator  

If `initialValue` is omitted:
- accumulator starts as the first element
- iteration starts at index 1

⚠️ Omitting `initialValue` is discouraged.

---

## Simple Examples

### Sum numbers
```js
[1, 2, 3].reduce((sum, n) => sum + n, 0);
// 6
```

### Multiply numbers
```js
[1, 2, 3, 4].reduce((prod, n) => prod * n, 1);
// 24
```

### Find max value
```js
[3, 7, 2].reduce((max, n) => n > max ? n : max, -Infinity);
```

---

## Reducing to Objects

### Count occurrences
```js
['a', 'b', 'a'].reduce((acc, val) => {
  acc[val] = (acc[val] ?? 0) + 1;
  return acc;
}, {});
```

### Group by property
```js
users.reduce((acc, user) => {
  const key = user.role;
  acc[key] ??= [];
  acc[key].push(user);
  return acc;
}, {});
```

---

## Reducing to Arrays

### Flatten array
```js
[[1, 2], [3, 4]].reduce((acc, arr) => {
  acc.push(...arr);
  return acc;
}, []);
```

### Map + Filter with reduce
```js
numbers.reduce((acc, n) => {
  if (n > 0) acc.push(n * 2);
  return acc;
}, []);
```

---

## reduce vs map / filter

| Method | Purpose |
|---|---|
| map | Transform elements |
| filter | Remove elements |
| reduce | General-purpose |
| forEach | Side effects |

Rule of thumb:
- Use `map` / `filter` when possible
- Use `reduce` when accumulating

---

## reduce vs for loop

```js
let sum = 0;
for (const n of arr) {
  sum += n;
}
```

Equivalent:
```js
const sum = arr.reduce((a, b) => a + b, 0);
```

---

## Common Mistakes

### Forgetting to return accumulator
```js
array.reduce((acc, x) => {
  acc += x; // ❌
}, 0);
```

Correct:
```js
array.reduce((acc, x) => {
  return acc + x;
}, 0);
```

---

### Missing initial value
```js
[].reduce((a, b) => a + b); // ❌ TypeError
```

Always provide initial value:
```js
[].reduce((a, b) => a + b, 0);
```

---

## Async reduce (gotcha)

❌ This does NOT work as expected:
```js
array.reduce(async (acc, x) => {
  return (await acc) + await fetchValue(x);
}, 0);
```

✅ Sequential async reduce:
```js
let result = 0;
for (const x of array) {
  result += await fetchValue(x);
}
```

---

## When to Use reduce()

✅ Summing / accumulating  
✅ Grouping data  
✅ Transforming to object or array  
❌ Simple map / filter cases  

---

## Key Takeaways

- `reduce()` can replace map + filter (but don't abuse it)
- Always return the accumulator
- Always provide an initial value
