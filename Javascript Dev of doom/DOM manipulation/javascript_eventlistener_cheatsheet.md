<# JavaScript Event Listeners Cheat Sheet

## 🎯 Basic Syntax

```js
element.addEventListener(event, callback);
```
- `event`: Type of event (e.g. `"click"`, `"input"`, `"submit"`)
- `callback`: Function to execute when event triggers

Example:
```js
document.querySelector("button").addEventListener("click", () => {
  console.log("Button clicked!");
});
```

---

## 🧠 Common Event Types

| Category       | Event Type(s)                     | Description |
|----------------|----------------------------------|--------------|
| Mouse          | `click`, `dblclick`, `mouseover`, `mouseout`, `mousedown`, `mouseup`, `mousemove` | Mouse interactions |
| Keyboard       | `keydown`, `keyup`, `keypress`   | Key actions |
| Form           | `submit`, `change`, `input`, `focus`, `blur` | Form and input behavior |
| Window         | `load`, `resize`, `scroll`, `unload` | Window lifecycle |
| Clipboard      | `copy`, `cut`, `paste`           | Clipboard actions |
| Touch (mobile) | `touchstart`, `touchmove`, `touchend` | Touchscreen actions |

---

## 🧩 Using Named Functions

```js
function handleClick() {
  console.log("Clicked!");
}

const btn = document.querySelector("#myButton");
btn.addEventListener("click", handleClick);
```

Remove listener:
```js
btn.removeEventListener("click", handleClick);
```

---

## 🎨 Event Object

Every event gives you access to the `event` object:

```js
document.addEventListener("click", (event) => {
  console.log(event.type);   // "click"
  console.log(event.target); // Element clicked
});
```

---

## ⚙️ Event Delegation

Attach a single listener to a parent element and detect events from children:

```js
document.querySelector("#list").addEventListener("click", (e) => {
  if (e.target.tagName === "LI") {
    console.log("Clicked on:", e.target.textContent);
  }
});
```

---

## ⌨️ Keyboard Events

```js
document.addEventListener("keydown", (e) => {
  if (e.key === "Enter") {
    console.log("Enter pressed!");
  }
});
```

---

## 🧼 Prevent Default and Stop Propagation

```js
document.querySelector("form").addEventListener("submit", (e) => {
  e.preventDefault(); // Prevents form from reloading the page
  console.log("Form submitted!");
});
```

```js
document.querySelector(".child").addEventListener("click", (e) => {
  e.stopPropagation(); // Stops event from bubbling to parent
  console.log("Child clicked!");
});
```

---

## 🔁 Multiple Events

```js
["click", "mouseover"].forEach(evt =>
  element.addEventListener(evt, () => console.log(`${evt} triggered`))
);
```

---

## 🧵 Once Option (Run Only Once)

```js
button.addEventListener("click", () => console.log("Clicked once!"), { once: true });
```

---

## 🧰 Tips
- Always use `addEventListener()` — don’t use inline `onclick` in HTML.  
- Remove listeners when elements are removed from DOM (for performance).  
- Combine with delegation for dynamic content.  

