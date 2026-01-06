# 🧠 Node.js stdin / Encoding / Buffer Cheat Sheet

## Default behavior (IMPORTANT)

```js
process.stdin.on('data', (chunk) => {
  // chunk is a Buffer
});
```

- `chunk` is a **Buffer**
- Buffers = raw bytes
- NO string methods (`replace`, `trim`, etc.)

---

## What a Buffer is

- Node’s binary type
- Similar to `unsigned char[]` in C
- Used because stdin **may not be text**

```js
Buffer.isBuffer(chunk); // true
```

---

## Why `.replace()` failed

```js
chunk.replace(/\n$/, '');
```

❌ `TypeError: replace is not a function`

Because:
- `.replace()` is a **string method**
- `chunk` was a **Buffer**

---

## Two correct ways to handle text input

### Option 1: Set encoding (text mode)

```js
process.stdin.setEncoding('utf8');

process.stdin.on('data', (chunk) => {
  chunk.replace(/\n$/, '');
});
```

- stdin emits **strings**
- Must be set **before** reading data

---

### Option 2: Convert manually

```js
process.stdin.on('data', (chunk) => {
  const text = chunk.toString();
  text.replace(/\n$/, '');
});
```

- Always works
- Preferred for robustness

---

## stdin newline trap

Input usually ends with `\n`

```bash
echo "John"
# sends "John\n"
```

Remove it:

```js
chunk.replace(/\n$/, '');
```

or:

```js
chunk.trim();
```

---

## console.log vs stdout.write

### console.log
- Appends `\n`
- Can cause double newlines
- Bad for strict graders

### process.stdout.write
- Exact output
- Full control
- Grader-safe

---

## Event meanings

| Event | Meaning |
|------|--------|
| data | Data arrived |
| end | stdin closed (EOF) |

---

## Why 'end' doesn't fire interactively

- Terminal stdin stays open
- Fires only with:
  - Ctrl+D
  - pipes
  - file redirection

---

## Golden rule

> **stdin is binary by default; text is opt-in.**
