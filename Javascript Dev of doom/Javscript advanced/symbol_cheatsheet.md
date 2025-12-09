# JavaScript Symbols Cheat Sheet

## What Is a Symbol?

Symbols are a primitive data type introduced in ES6.\
They create unique identifiers, mainly used as keys on objects to avoid
naming collisions.

------------------------------------------------------------------------

## Creating Symbols

``` js
const sym1 = Symbol();
const sym2 = Symbol("description"); // optional label
```

------------------------------------------------------------------------

## Symbols Are Unique

``` js
Symbol("id") === Symbol("id"); // false
```

------------------------------------------------------------------------

## Using Symbols as Object Keys

``` js
const ID = Symbol("id");

const user = {
  name: "Alice",
  [ID]: 123
};

console.log(user[ID]); // 123
```

------------------------------------------------------------------------

## Global Symbol Registry (`Symbol.for`)

``` js
const s1 = Symbol.for("token");
const s2 = Symbol.for("token");

console.log(s1 === s2); // true
```

Retrieve its key:

``` js
Symbol.keyFor(s1); // "token"
```

------------------------------------------------------------------------

## Built‑in Well‑Known Symbols

These let you customize JavaScript behavior.

### `Symbol.iterator`

Controls iteration:

``` js
const obj = {
  data: [1,2,3],
  [Symbol.iterator]() {
    return this.data.values();
  }
};

for (const x of obj) console.log(x);
```

### `Symbol.toStringTag`

Customize `[object Something]`:

``` js
class Person {
  get [Symbol.toStringTag]() {
    return "PersonObject";
  }
}

Object.prototype.toString.call(new Person()); 
// "[object PersonObject]"
```

### `Symbol.toPrimitive`

Controls object → primitive conversion:

``` js
const obj = {
  [Symbol.toPrimitive](hint) {
    if (hint === "number") return 10;
    if (hint === "string") return "hello";
    return 42;
  }
};

+obj;        // 10
`${obj}`;    // "hello"
```

### `Symbol.hasInstance`

Customize `instanceof`:

``` js
class Custom {
  static [Symbol.hasInstance](instance) { return true; }
}

console.log({} instanceof Custom); // true
```

### `Symbol.isConcatSpreadable`

Controls array spreading:

``` js
const arr = [1,2,3];
arr[Symbol.isConcatSpreadable] = false;

console.log([].concat(arr)); 
// [[1,2,3]]
```

### `Symbol.match`, `Symbol.replace`, `Symbol.search`, `Symbol.split`

Customize regex-like behavior.

Example:

``` js
const matcher = {
  [Symbol.match](str) {
    return str.includes("cat") ? ["cat"] : null;
  }
};

"dogcatfish".match(matcher); 
// ["cat"]
```

### `Symbol.species`

Controls constructor used when creating derived objects.

------------------------------------------------------------------------

## Why Use Symbols?

-   Prevent property name collisions\
-   Create "private-like" object keys\
-   Plug into JavaScript's internal behavior via well-known symbols\
-   More expressive meta-programming

------------------------------------------------------------------------

## Quick Reference of Well‑Known Symbols

  Symbol                        Purpose
  ----------------------------- ----------------------------------------
  `Symbol.iterator`             Customize iteration
  `Symbol.toStringTag`          Custom `[object Tag]`
  `Symbol.toPrimitive`          Control primitive conversion
  `Symbol.hasInstance`          Customize `instanceof`
  `Symbol.isConcatSpreadable`   Control array spreading
  `Symbol.match`                Override `String.prototype.match`
  `Symbol.replace`              Override `String.prototype.replace`
  `Symbol.search`               Override `String.prototype.search`
  `Symbol.split`                Override `String.prototype.split`
  `Symbol.species`              Choose constructor for derived objects
  `Symbol.unscopables`          Hide properties from `with` blocks

------------------------------------------------------------------------

## Summary

Symbols unlock powerful customization capabilities in JavaScript,
providing encapsulation, unique keys, and control over built-in language
mechanics.
