# JavaScript Object Length Cheat Sheet

JavaScript objects don't have a native `.length` property, so you must
use one of these standard techniques to count the number of keys or
entries.

## 1. `Object.keys(obj).length`

Counts **own enumerable string keys**.

``` js
Object.keys(obj).length;
```

## 2. `Object.values(obj).length`

Counts **values**, same count as keys unless filtered.

``` js
Object.values(obj).length;
```

## 3. `Object.entries(obj).length`

Counts **key-value pairs**.

``` js
Object.entries(obj).length;
```

## 4. `Reflect.ownKeys(obj).length`

Counts **all keys**, including: - string keys\
- symbol keys\
- non-enumerable keys

``` js
Reflect.ownKeys(obj).length;
```

## 5. Safe `for...in` counting

Ignores inherited properties.

``` js
let count = 0;
for (const key in obj) {
  if (Object.hasOwn(obj, key)) count++;
}
```

## Not supported

Plain JS objects **do not** have:

    obj.length    // ❌ does not exist
    obj.size      // ❌ does not exist
