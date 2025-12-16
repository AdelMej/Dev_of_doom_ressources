# JavaScript ArrayBuffer Cheat Sheet

## What is ArrayBuffer?
- A fixed-length raw binary data buffer
- Cannot be directly read or written
- Used with TypedArrays or DataView

```js
const buffer = new ArrayBuffer(16); // 16 bytes
```

---

## Key Properties
```js
buffer.byteLength; // total size in bytes
```

---

## Typed Arrays (Views)
TypedArrays provide a typed view over an ArrayBuffer.

| Type | Bytes |
|----|------|
| Int8Array | 1 |
| Uint8Array | 1 |
| Uint16Array | 2 |
| Int32Array | 4 |
| Float32Array | 4 |
| Float64Array | 8 |

```js
const view = new Uint8Array(buffer);
view[0] = 255;
```

---

## Multiple Views on Same Buffer
```js
const buffer = new ArrayBuffer(8);
const u8 = new Uint8Array(buffer);
const u32 = new Uint32Array(buffer);

u32[0] = 0xdeadbeef;
console.log(u8);
```

---

## DataView (low-level control)
Use when you need:
- Endianness control
- Mixed types

```js
const dv = new DataView(buffer);
dv.setUint16(0, 500, true); // little-endian
dv.getUint16(0, true);
```

---

## Creating from TypedArray
```js
const arr = new Uint8Array([1, 2, 3]);
const buffer = arr.buffer;
```

---

## Slicing
```js
const slice = buffer.slice(0, 4);
```

---

## Common Use Cases
- Binary file parsing
- Network protocols
- WebSockets
- WebGL / WASM
- Performance-critical code

---

## Mental Model

ArrayBuffer = raw memory  
TypedArray = typed window  
Index = byte offset  

---

## When to Use
✔ Binary data  
✔ Performance  
✔ Interop (WASM, GPU, network)

❌ Regular app data  
❌ JSON-style objects

---

## Quick Example
```js
const buffer = new ArrayBuffer(4);
const view = new Uint32Array(buffer);
view[0] = 42;
console.log(view[0]);
```

---

## One-Line Summary
> ArrayBuffer is raw memory. TypedArrays tell you how to read it.
