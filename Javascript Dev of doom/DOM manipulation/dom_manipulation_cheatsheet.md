# DOM Manipulation Cheat Sheet (Vanilla JavaScript)

## Selecting Elements
- `document.querySelector(selector)`
- `document.querySelectorAll(selector)`
- `document.getElementById(id)`
- `document.getElementsByClassName(className)`
- `document.getElementsByTagName(tag)`

## Creating Elements
```js
const div = document.createElement("div");
const p = document.createElement("p");
```

## Adding / Removing Elements
```js
parent.appendChild(child);
parent.removeChild(child);
element.remove();
element.append();
element.prepend();
element.replaceWith(newNode);
```

## Classes
```js
element.classList.add("visible");
element.classList.remove("hidden");
element.classList.toggle("active");
element.classList.contains("card");
```

## Attributes
```js
element.setAttribute("data-id", "123");
element.getAttribute("data-id");
element.removeAttribute("disabled");
```

## Dataset
```js
element.dataset.price = "100";
console.log(element.dataset.price);
```

## Inline Styles
```js
element.style.display = "none";
element.style.display = "";
element.style.color = "red";
```

## Events
```js
element.addEventListener("click", handler);
element.removeEventListener("click", handler);
```

## DOM Traversal
```js
element.parentElement;
element.children;
element.firstElementChild;
element.lastElementChild;
element.nextElementSibling;
element.previousElementSibling;
```

## Scoped Queries
```js
card.querySelector(".title");
card.querySelectorAll(".review");
```

## Insert HTML
```js
element.innerHTML = "<p>Hello!</p>";
element.textContent = "Hello!";

element.insertAdjacentHTML("beforeend", "<p>Added!</p>");
```

## Form Data
```js
const formData = new FormData(form);
formData.get("email");
formData.get("password");
```

## Observe DOM Changes
```js
const observer = new MutationObserver(muts => console.log(muts));
observer.observe(target, { childList: true, subtree: true });
```

## Summary
If you know:
- querySelector
- createElement
- classList
- dataset
- append/remove
- events
- innerHTML/textContent

You can do **99%** of DOM manipulation.
