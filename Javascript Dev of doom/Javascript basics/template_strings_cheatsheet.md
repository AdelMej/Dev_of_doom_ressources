# JavaScript Template Strings (Template Literals) Cheat Sheet

## 🔹 1. Basic Syntax
Template strings use backticks instead of quotes:

```js
`Hello world`
```

---

## 🔹 2. Variable Interpolation
Insert variables with `${...}`:

```js
const name = "Adel";
const greeting = `Hello, ${name}!`;
```

---

## 🔹 3. Embedding Expressions
Any JS expression works:

```js
const a = 5, b = 3;
const result = `Sum = ${a + b}`;
```

You can even call functions:

```js
const upper = s => s.toUpperCase();
`Upper: ${upper("hello")}`;
```

---

## 🔹 4. Multiline Strings
Template strings preserve newlines:

```js
const text = `
Line 1
Line 2
Line 3
`;
```

---

## 🔹 5. Building HTML
Perfect for frontend string construction:

```js
const user = "Adel";
const html = `
<div class="card">
  <h1>${user}</h1>
</div>
`;
```

---

## 🔹 6. No More String Concatenation
Old way:

```js
"Hello " + name + "!"
```

Modern way:

```js
`Hello ${name}!`
```

---

## 🔹 7. Tagged Template Literals (Advanced)

```js
function tag(strings, ...values) {
  console.log(strings);
  console.log(values);
}

const city = "Paris";
tag`Welcome to ${city}!`;
```

---

## 🔹 8. String.raw
Outputs the string *exactly* as written:

```js
String.raw`C:\Users\Adel\Desktop`;
```

---

## 🔹 9. Whitespace Warning
Template literals preserve all indentation and spaces:

```js
const x = `
  indented text
`;
```

---

## 🔹 10. Summary Table

| Feature | Supported | Example |
|--------|-----------|---------|
| Interpolation | ✅ | `${value}` |
| Expressions | ✅ | `${a + b}` |
| Multiline | ✅ | backticks |
| HTML templates | ✅ | `<div>...</div>` |
| Tagged templates | ✅ | `tag\`...\`` |
| Raw strings | ✅ | `String.raw` |

