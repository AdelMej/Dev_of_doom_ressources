# JavaScript Special Methods (Magic Methods) Cheat Sheet

## 1. Primitive Conversion & Stringification

### toString()

Used when converting an object to a string.

### valueOf()

Legacy primitive converter.

### Symbol.toPrimitive

Controls full primitive conversion.

### Symbol.toStringTag

Customizes `[object Something]`.

## 2. Iteration Protocol

### Symbol.iterator

Defines sync iteration.

### Symbol.asyncIterator

Defines async iteration.

## 3. RegExp Behavior

-   Symbol.match
-   Symbol.search
-   Symbol.replace
-   Symbol.split
-   Symbol.matchAll

## 4. Array & Collection Behavior

-   Symbol.isConcatSpreadable
-   Symbol.species

## 5. Operator / Instance Behavior

-   Symbol.hasInstance

## 6. Scoping

-   Symbol.unscopables

## 7. Proxy Traps

(get, set, apply, construct, has, defineProperty, deleteProperty,
ownKeys, getPrototypeOf, setPrototypeOf, preventExtensions,
isExtensible)

## 8. Generators

-   \*method()
-   async \*method()
