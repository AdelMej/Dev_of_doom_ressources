# FormData Cheat Sheet (JavaScript)

## Create FormData
### From form:
```js
const form = document.querySelector("form");
const fd = new FormData(form);
```

### Empty FormData:
```js
const fd = new FormData();
fd.append("username", "Alice");
fd.append("age", 20);
```

## Reading values
```js
fd.get("fieldName");
fd.has("email");
for (const [k,v] of fd.entries()) console.log(k,v);
fd.getAll("hobbies");
```

## Modify data
```js
fd.append("token", "123");
fd.set("username", "Bob");
fd.delete("password");
```

## Send with fetch
```js
fetch("/api/submit", { method:"POST", body: fd });
```

## Convert to JSON
```js
const json = Object.fromEntries(fd.entries());
fetch("/api/submit", {
  method:"POST",
  headers:{ "Content-Type":"application/json" },
  body: JSON.stringify(json)
});
```

## Files
```js
const file = fd.get("avatar");
console.log(file.name, file.size, file.type);
```

## Prevent reload
```js
form.addEventListener("submit", e => e.preventDefault());
```

## Debug
```js
for (const [k,v] of fd) console.log(k, v);
```
