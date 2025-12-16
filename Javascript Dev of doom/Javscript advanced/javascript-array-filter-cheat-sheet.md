# JavaScript Array.filter() Cheat Sheet

> `Array.prototype.filter()` creates a new array with elements that pass a condition.

---

## Basic Syntax
```js
const newArray = array.filter((element, index, array) => {
  return condition;
});
```

Short form:
```js
const newArray = array.filter(el => el > 0);
```

---

## Simple Examples

### Filter positive numbers
```js
[-2, -1, 0, 1, 2].filter(n => n > 0);
// [1, 2]
```

### Remove falsy values
```js
[0, 1, false, 2, '', 3].filter(Boolean);
// [1, 2, 3]
```

### Filter strings by length
```js
['a', 'ab', 'abc'].filter(s => s.length > 1);
// ['ab', 'abc']
```

---

## Filtering Objects

### By property value
```js
users.filter(user => user.active);
```

### Multiple conditions
```js
users.filter(user =>
  user.active &&
  user.age >= 18
);
```

---

## Using index

```js
['a', 'b', 'c'].filter((value, index) => index % 2 === 0);
// ['a', 'c']
```

---

## Chaining

```js
items
  .filter(item => item.active)
  .map(item => item.name);
```

Another example:
```js
numbers
  .filter(n => n > 10)
  .filter(n => n % 2 === 0);
```

---

## filter vs map

| filter | map |
|---|---|
| Removes elements | Transforms elements |
| Length may change | Length stays same |
| Condition-based | Value-based |

---

## filter vs for loop

```js
const result = [];
for (const x of arr) {
  if (x > 0) {
    result.push(x);
  }
}
```

Equivalent:
```js
const result = arr.filter(x => x > 0);
```

---

## Common Mistakes

### Returning non-boolean values
```js
array.filter(x => x * 2); // ⚠️ truthy/falsy
```

Better:
```js
array.filter(x => x > 0);
```

### Using filter for side effects
```js
array.filter(x => {
  console.log(x); // ❌
  return true;
});
```

Use `forEach` instead.

---

## Removing duplicates
```js
array.filter((value, index, self) =>
  self.indexOf(value) === index
);
```

(Prefer `Set` for large arrays)

---

## Async filter (gotcha)

❌ This does NOT work:
```js
array.filter(async x => await isValid(x));
```

✅ Correct:
```js
const results = await Promise.all(
  array.map(async x => ({
    x,
    valid: await isValid(x),
  }))
);

const filtered = results
  .filter(r => r.valid)
  .map(r => r.x);
```

---

## When to Use filter()

✅ Remove unwanted items  
✅ Apply conditions  
❌ Transform values  
❌ Side effects  

---

## Key Takeaways

- `filter()` returns a new array
- Original array is unchanged
- Callback should return boolean
