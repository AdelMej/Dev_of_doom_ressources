# JavaScript Constructor Cheat Sheet

## 1. Basic Class Constructor

Runs automatically when `new` is used. Must call `super()` in subclasses
before using `this`.

``` js
class Car {
  constructor(model) {
    this.model = model;
  }
}
```

## 2. Default Constructor Behavior

If omitted, JS creates:

-   Base class: `constructor() {}`
-   Subclass: `constructor(...args) { super(...args); }`

## 3. Constructor in Inheritance

### Base class:

``` js
class Vehicle {
  constructor(type) {
    this.type = type;
  }
}
```

### Subclass:

``` js
class Car extends Vehicle {
  constructor(type, model) {
    super(type);
    this.model = model;
  }
}
```

## 4. What `this.constructor` Points To

``` js
this.constructor === TheClassUsedWithNew;
```

Examples:

``` js
new Car("BMW").constructor === Car;        // true
new SportsCar().constructor === SportsCar; // true
```

## 5. Why Symbol.species Exists

Controls which constructor is used for derived objects in built‑ins like
Arrays, Promises, Maps.

``` js
class MyArray extends Array {
  static get [Symbol.species]() { return Array; }
}
```

  species value   effect
  --------------- --------------------------------
  class           used to construct results
  undefined       defaults to `this.constructor`
  null            disables construction

## 6. When to Use super()

-   Base class: no
-   Subclass: yes, before using `this`

## 7. Common Pitfalls

### Using `this` before `super()` → error

### `new this.constructor()` may call subclass constructor unexpectedly

## 8. Quick Summary

-   constructor runs on `new`
-   subclass must call super
-   `this.constructor` = actual runtime class
-   `Symbol.species` overrides derived instance construction
