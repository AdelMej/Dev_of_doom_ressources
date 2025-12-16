# JavaScript `Set` Cheat Sheet

A `Set` is a collection of **unique values**. Values are compared using **SameValueZero**
(similar to `===`, but `NaN` is equal to `NaN`).

---

## Create a Set

```js
const set = new Set();
const setFromArray = new Set([1, 2, 3, 3]); // {1, 2, 3}
```

---

## Basic Operations

```js
set.add(value);        // Add value
set.delete(value);     // Remove value
set.has(value);        // true / false
set.clear();           // Remove all values
set.size;              // Number of elements
```

---

## Iteration

```js
for (const value of set) {
  console.log(value);
}

set.forEach(value => {
  console.log(value);
});
```

> `Set` preserves **insertion order**.

---

## Convert Between Set and Array

```js
const arr = [...set];
const arr2 = Array.from(set);

const newSet = new Set(arr);
```

---

## Common Use Cases

### Remove Duplicates

```js
const unique = [...new Set(array)];
```

### Fast Membership Check (O(1))

```js
if (set.has(value)) {
  // do something
}
```

### Track Seen Values

```js
const seen = new Set();

for (const item of items) {
  if (seen.has(item)) continue;
  seen.add(item);
}
```

---

## Set vs Array

| Feature | Set | Array |
|------|-----|-------|
| Unique values | ✅ | ❌ |
| Fast lookup | ✅ | ❌ |
| Indexed access | ❌ | ✅ |
| Order preserved | ✅ | ✅ |

---

## Set Operations (Manual)

### Union

```js
const union = new Set([...a, ...b]);
```

### Intersection

```js
const intersection = new Set(
  [...a].filter(x => b.has(x))
);
```

### Difference

```js
const difference = new Set(
  [...a].filter(x => !b.has(x))
);
```

---

## Objects in Sets

```js
const a = { x: 1 };
const b = { x: 1 };

const s = new Set();
s.add(a);
s.add(b);

s.size; // 2 (different references)
```

---

## Gotchas

- No indexing: `set[0]` ❌
- Values must be the **same reference** for objects
- No built-in map/filter (convert to array)

---

## When to Use `Set`

✅ Uniqueness matters  
✅ Fast existence checks  
✅ Tracking visited / active items  
❌ Ordered numeric access  
❌ Frequent transformations

---

## Mental Model 🦍

> **Set = box of unique items**  
> No duplicates  
> Fast lookups  
> No indexes
