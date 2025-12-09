# JavaScript REST & SPREAD Cheat Sheet

## 1. REST in Functions (variadic functions)

Collect function arguments into an array:

``` js
function sum(...nums) {
  return nums.reduce((a, b) => a + b, 0);
}
```

## 2. REST in Array Destructuring

``` js
const [first, second, ...others] = [10, 20, 30, 40, 50];
```

## 3. REST in Object Destructuring

``` js
const user = { name: "Adel", age: 21, country: "FR", admin: true };
const { name, ...info } = user;
```

## 4. SPREAD for Arrays (copying & merging)

``` js
const arr = [1, 2, 3];
const copy = [...arr];
const merged = [...arr1, ...arr2];
```

## 5. SPREAD for Objects

``` js
const merged = { ...a, ...b };
```

## 6. SPREAD in Function Calls

``` js
const nums = [1, 5, 10];
Math.max(...nums);
```

## 7. REST vs SPREAD Summary

  Operator      Purpose
  ------------- ------------------------
  `...rest`     Gathers items (input)
  `...spread`   Expands items (output)
