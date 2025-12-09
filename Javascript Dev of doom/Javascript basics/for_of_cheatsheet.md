# JavaScript `for...of` Loop Cheat Sheet

The `for...of` loop provides a clean, elegant way to iterate over **iterable** values in JavaScript.

---

## ✅ What You Can Iterate With `for...of`
`for...of` works on **iterables**, including:
- Arrays
- Strings
- Maps
- Sets
- Typed arrays
- Arguments objects
- Generator objects

---

## 🔹 Basic Syntax

```js
for (const item of iterable) {
  console.log(item);
}
```

---

## 🔹 Iterating Arrays

```js
const nums = [10, 20, 30];

for (const n of nums) {
  console.log(n);
}
```

---

## 🔹 Iterating Strings

```js
for (const char of "Hello") {
  console.log(char);
}
```

---

## 🔹 Iterating Maps

```js
const map = new Map([
  ['a', 1],
  ['b', 2]
]);

for (const [key, value] of map) {
  console.log(key, value);
}
```

---

## 🔹 Iterating Sets

```js
const set = new Set([1, 2, 3]);

for (const value of set) {
  console.log(value);
}
```

---

## 🔹 Iterating Over Objects (⚠️ Not allowed directly)

`for...of` **does NOT work** with plain objects:
```js
const obj = { a: 1, b: 2 };
for (const x of obj) {} // ❌ TypeError
```

Use:
- `Object.keys(obj)`
- `Object.values(obj)`
- `Object.entries(obj)`

Example:
```js
for (const [key, value] of Object.entries(obj)) {
  console.log(key, value);
}
```

---

## 🔹 Comparing `for...of` vs `for...in`

| Feature | `for...of` | `for...in` |
|--------|------------|-------------|
| Iterates values | ✅ | ❌ (keys only) |
| Works on arrays | ✅ | ⚠️ (iterates indices, not recommended) |
| Works on objects | ❌ | ✅ |
| Works on strings | ✅ | ⚠️ (indexes only) |
| Works on Maps/Sets | ✅ | ❌ |

---

## 🔹 Break, Continue, Return
You *can* use:
```js
break;
continue;
return; // inside functions
```

---

## 🔹 Example Using `break`
```js
for (const n of [5, 10, 15, 20]) {
  if (n === 15) break;
  console.log(n);
}
```

---

## 🔹 Async Iteration (`for await...of`)
Used for **async iterables**.

```js
for await (const item of asyncIterable) {
  console.log(item);
}
```

---

## 👍 Summary
- `for...of` iterates **values**, not keys.
- Works only with **iterables**.
- Cleaner and safer than `for...in` for arrays.
- Supports break/continue/return.

