# JavaScript Map Cheat Sheet

> ES6 `Map` (not `Array.map()`)

---

## Creating a Map
```js
const map = new Map();
```

With initial values:
```js
const map = new Map([
  ['a', 1],
  ['b', 2],
]);
```

---

## Basic Operations

### Set a value
```js
map.set('key', 'value');
map.set(1, 'number key');
map.set({}, 'object key');
```

### Get a value
```js
map.get('key'); // 'value'
```

### Check existence
```js
map.has('key'); // true / false
```

### Delete a key
```js
map.delete('key'); // true if deleted
```

### Clear all entries
```js
map.clear();
```

### Size
```js
map.size;
```

---

## Iteration

### Loop over entries
```js
for (const [key, value] of map) {
  console.log(key, value);
}
```

### Keys only
```js
for (const key of map.keys()) {
  console.log(key);
}
```

### Values only
```js
for (const value of map.values()) {
  console.log(value);
}
```

### Using forEach
```js
map.forEach((value, key) => {
  console.log(key, value);
});
```

---

## Conversions

### Map → Array
```js
[...map];                 // [[key, value], ...]
[...map.entries()];
```

### Map → Object (string keys only)
```js
Object.fromEntries(map);
```

### Object → Map
```js
const obj = { a: 1, b: 2 };
const map = new Map(Object.entries(obj));
```

---

## Common Patterns

### Default value
```js
const count = map.get(key) ?? 0;
map.set(key, count + 1);
```

### Counting occurrences
```js
for (const item of items) {
  map.set(item, (map.get(item) ?? 0) + 1);
}
```

### Using objects as keys
```js
const user = { id: 1 };
map.set(user, 'admin');
map.get(user); // 'admin'
```

---

## When to Use Map vs Object

| Use case | Map | Object |
|--------|-----|--------|
| Any key type | ✅ | ❌ |
| Frequent add/remove | ✅ | ⚠️ |
| Preserve insertion order | ✅ | ⚠️ |
| Simple JSON data | ❌ | ✅ |

---

## Map vs Array.map()

❌ Not this:
```js
array.map(fn);
```

✅ This:
```js
const map = new Map();
map.set(key, value);
```

---

## Gotchas

- Keys are compared by reference:
```js
map.set({}, 1);
map.get({}); // undefined
```

- Map is iterable by default:
```js
for (const entry of map) { ... }
```
