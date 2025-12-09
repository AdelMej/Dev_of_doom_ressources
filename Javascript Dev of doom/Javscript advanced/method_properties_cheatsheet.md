# Method Properties in JavaScript -- Cheat Sheet

## 1. What Are Method Properties?

Method properties are a concise way to define functions inside
JavaScript objects.

### Old Syntax (Verbose)

``` js
const obj = {
  sayHi: function() {
    return "Hello!";
  }
};
```

### New Syntax (Method Shorthand)

``` js
const obj = {
  sayHi() {
    return "Hello!";
  }
};
```

------------------------------------------------------------------------

## 2. Why Use Method Properties?

-   Shorter, cleaner syntax
-   Matches class method syntax
-   Easier to read in object literals
-   Useful for object-based modules and configs

------------------------------------------------------------------------

## 3. Examples

### Example 1: Simple Method

``` js
const math = {
  add(a, b) {
    return a + b;
  }
};
```

### Example 2: Methods + Other Properties

``` js
const user = {
 a name: "Alice",
  greet() {
    return `Hello, I'm ${this.name}`;
  }
};
```

### Example 3: Dynamic Key + Method (with computed property)

``` js
const action = "run";
const obj = {
  [action]() {
    return "Running!";
  }
};
```

### Example 4: Using `this`

``` js
const counter = {
  value: 0,
  increment() {
    this.value++;
  }
};
```

------------------------------------------------------------------------

## 4. Where You'll Use Method Properties

-   Object-based modules
-   Config objects (Node.js, frameworks)
-   Classes (similar syntax)
-   Namespaced API objects
-   Event handler collections

------------------------------------------------------------------------

## 5. Limitations

-   Not usable outside object literals
-   Not suited for functions that aren't methods
-   Still must be careful with `this` binding

------------------------------------------------------------------------

## 6. Quick Reference Table

  Feature             Old Syntax             Shorthand
  ------------------- ---------------------- --------------
  Method              `foo: function() {}`   `foo() {}`
  Uses `this`         Yes                    Yes
  Inside class        Already shorthand      Same
  With computed key   `['x']: function()`    `['x']() {}`

------------------------------------------------------------------------

## 7. Summary

Method properties provide a clean, modern way to define object functions
and are consistent with ES6+ class syntax. They improve readability and
are widely used in modern JavaScript codebases.
