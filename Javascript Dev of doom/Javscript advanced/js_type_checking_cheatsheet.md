# JavaScript Type-Checking Cheat Sheet

## 1. Primitives

-   **String:** `typeof v === "string"`
-   **Number (not NaN):** `typeof v === "number" && !Number.isNaN(v)`
-   **Boolean:** `typeof v === "boolean"`
-   **BigInt:** `typeof v === "bigint"`
-   **Symbol:** `typeof v === "symbol"`
-   **Undefined:** `typeof v === "undefined"`
-   **Null:** `v === null`

## 2. Functions

-   **Function:** `typeof v === "function"`

## 3. Objects

-   **Plain object:**
    `typeof v === "object" && v !== null && !Array.isArray(v)`
-   **Any non-null object:** `v !== null && typeof v === "object"`

## 4. Arrays

-   **Array:** `Array.isArray(v)`
-   **Array of strings:**
    `Array.isArray(v) && v.every(x => typeof x === "string")`
-   **Array of numbers:**
    `Array.isArray(v) && v.every(x => typeof x === "number" && !Number.isNaN(x))`

## 5. Special Cases

-   **NaN:** `Number.isNaN(v)`
-   **Finite number:** `Number.isFinite(v)`
-   **Integer:** `Number.isInteger(v)`
-   **Date:** `v instanceof Date`
-   **Valid date:** `v instanceof Date && !isNaN(v.getTime())`
-   **RegExp:** `v instanceof RegExp`

## 6. Mixed-Type Checks

-   **String OR number:**
    `typeof v === "string" || typeof v === "number"`
-   **Array OR object:** `(typeof v === "object" && v !== null)`

## 7. Existence Checks

-   **Not null/undefined:** `v != null`
-   **Strict:** `v !== null && v !== undefined`

## 8. Helpers

``` js
const isPlainObject = (v) =>
  v !== null && typeof v === "object" && !Array.isArray(v);

const isStringArray = (v) =>
  Array.isArray(v) && v.every(x => typeof x === "string");

const isNumber = (v) =>
  typeof v === "number" && !Number.isNaN(v);
```
