# 📘 HTML `data-*` Attribute Cheat Sheet

## 🧩 What Are `data-*` Attributes?

`data-*` attributes let you store **custom metadata** on HTML elements
in a standards-compliant way.

``` html
<div data-price="29.99" data-category="books"></div>
```

They are accessible in JavaScript via the `dataset` API.

------------------------------------------------------------------------

## 📌 Syntax

### **HTML**

    data-<name>="<value>"

### **JavaScript**

``` js
element.dataset.<name>
```

> ⚠️ Note:\
> `data-price` → `element.dataset.price`\
> `data-long-name` → `element.dataset.longName` (camelCase)

------------------------------------------------------------------------

## 🧱 Examples

### Basic Usage

``` html
<div id="card" data-id="42" data-price="99"></div>
```

``` js
const card = document.getElementById("card");
console.log(card.dataset.id);     // "42"
console.log(card.dataset.price);  // "99"
```

------------------------------------------------------------------------

## 🔄 Setting + Updating Values

### **Set**

``` js
card.dataset.price = "120";
```

### **Remove**

``` js
delete card.dataset.price;
```

------------------------------------------------------------------------

## 🔢 Type Casting

All dataset values are **strings**.\
You must convert them manually:

``` js
const price = Number(card.dataset.price);
const isActive = card.dataset.active === "true";
```

------------------------------------------------------------------------

## 🧪 Checking Attribute Existence

``` js
if ("price" in card.dataset) {
    console.log("Price exists!");
}
```

------------------------------------------------------------------------

## 🧹 Looping Through All Data Attributes

``` js
for (const [key, value] of Object.entries(card.dataset)) {
    console.log(key, value);
}
```

------------------------------------------------------------------------

## ⚙️ Use Cases

-   Filtering (price, rating, category, etc.)
-   Passing config into JS without global variables
-   Storing IDs or references
-   UI state (expanded, active, toggled)
-   Making reusable components (cards, items, widgets)

------------------------------------------------------------------------

## 🛑 Naming Rules

-   Must start with `data-`
-   Must contain only letters, numbers, dashes
-   JS converts dashes → camelCase

Example:

    data-user-id → dataset.userId
    data-bg-color → dataset.bgColor

------------------------------------------------------------------------

## 📝 Best Practices

✔ Keep values short\
✔ Use lowercase with dashes (`data-user-id`)\
✔ Convert types when reading (`Number()`, `JSON.parse()`, etc.)\
✔ Use datasets for UI/state --- not large data blobs\
✔ Keep structure consistent across your components
