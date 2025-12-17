# JavaScript String Methods Cheat Sheet

Strings in JavaScript are **immutable** — every method returns a **new string**.

---

## Basic Properties

```js
str.length
```

---

## Search & Check

```js
str.includes(substring)
str.indexOf(substring)
str.lastIndexOf(substring)

str.startsWith(prefix)
str.endsWith(suffix)
```

---

## Extracting Parts

```js
str.slice(start, end)
str.substring(start, end)
str.substr(start, length) // deprecated
```

Examples:
```js
"hello".slice(1, 4); // "ell"
"hello".substring(1, 4); // "ell"
```

---

## Replace

```js
str.replace("a", "b")
str.replaceAll("a", "b")
str.replace(/a/g, "b")
```

---

## Case Conversion

```js
str.toUpperCase()
str.toLowerCase()
```

---

## Trim Whitespace

```js
str.trim()
str.trimStart()
str.trimEnd()
```

---

## Split & Join

```js
str.split(",")
array.join(",")
```

---

## Padding

```js
str.padStart(length, padString)
str.padEnd(length, padString)
```

---

## Character Access

```js
str.charAt(index)
str[index]
str.charCodeAt(index)
```

---

## Comparison

```js
str.localeCompare(other)
```

---

## Repeat

```js
str.repeat(count)
```

---

## Regex Helpers

```js
str.match(regex)
str.search(regex)
str.matchAll(regex)
```

---

## Common Patterns

### Case-insensitive search

```js
str.toLowerCase().includes(query.toLowerCase())
```

### Safe substring check

```js
if (str?.includes("test")) {}
```

---

## Gotchas

- Strings are immutable
- `replace` replaces only first match unless regex or `replaceAll`
- `substring` swaps arguments if start > end

---

## Mental Model 🧠

> **String = immutable sequence of characters**  
> Methods return new strings  
> Search → includes / startsWith  
> Transform → replace / slice
