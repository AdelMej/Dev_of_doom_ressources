
# JavaScript Array Methods Cheat Sheet

## Creation
- **`const arr = [1, 2, 3];`** → Create an array
- **`Array.of(1, 2, 3)`** → Create an array from arguments
- **`Array.from('hello')`** → Convert iterable to array → `['h', 'e', 'l', 'l', 'o']`

---

## Basic Methods
- **`arr.length`** → Get length
- **`arr.toString()`** → Convert to string
- **`arr.join(', ')`** → Join elements with separator

---

## Adding / Removing Elements
- **`arr.push(el)`** → Add to end
- **`arr.pop()`** → Remove from end
- **`arr.unshift(el)`** → Add to start
- **`arr.shift()`** → Remove from start
- **`arr.splice(start, deleteCount, ...items)`** → Add/remove at index
- **`arr.slice(start, end)`** → Copy subarray (non-destructive)

---

## Searching / Checking
- **`arr.includes(el)`** → Check if exists
- **`arr.indexOf(el)`** → Get index of first occurrence
- **`arr.lastIndexOf(el)`** → Get index of last occurrence
- **`arr.find(fn)`** → Return first element matching condition
- **`arr.findIndex(fn)`** → Return index of first match
- **`arr.some(fn)`** → Return true if any element passes test
- **`arr.every(fn)`** → Return true if all elements pass test

---

## Iteration
- **`arr.forEach(fn)`** → Loop through array
- **`for (const el of arr)`** → Modern loop syntax

---

## Transforming
- **`arr.map(fn)`** → Transform each element
- **`arr.filter(fn)`** → Keep elements that pass test
- **`arr.reduce(fn, initial)`** → Accumulate into single value
- **`arr.flat(depth)`** → Flatten nested arrays
- **`arr.flatMap(fn)`** → Map and flatten one level
- **`arr.reverse()`** → Reverse order (mutates array)
- **`arr.sort(fn)`** → Sort elements (mutates array)

---

## Combining / Copying
- **`arr.concat(otherArr)`** → Merge arrays
- **`[...arr1, ...arr2]`** → Spread syntax merge
- **`arr.copyWithin(target, start, end)`** → Copy within array (mutates)

---

## Conversion
- **`arr.join(' ')`** → Convert to string
- **`Array.from(obj)`** → Convert iterable/object to array
- **`Array.isArray(val)`** → Check if value is array

---

## Examples
```js
const nums = [1, 2, 3, 4, 5];

nums.map(x => x * 2);        // [2, 4, 6, 8, 10]
nums.filter(x => x > 2);     // [3, 4, 5]
nums.reduce((a, b) => a + b, 0); // 15
nums.find(x => x === 3);     // 3
nums.includes(4);            // true
```
