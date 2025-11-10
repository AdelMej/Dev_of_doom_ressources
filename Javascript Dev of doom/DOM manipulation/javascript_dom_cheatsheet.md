# 🧭 JavaScript DOM Manipulation Cheat Sheet (Fixed)

This markdown uses HTML tables to avoid renderer-specific pipe/markdown table issues.

## Selecting Elements

<table>
  <thead>
    <tr><th>Selector</th><th>Example</th><th>Description</th></tr>
  </thead>
  <tbody>
    <tr><td><code>document.getElementById("id")</code></td><td><code>let el = document.getElementById("header");</code></td><td>Selects by ID</td></tr>
    <tr><td><code>document.getElementsByClassName("class")</code></td><td><code>let items = document.getElementsByClassName("box");</code></td><td>Selects by class (HTMLCollection)</td></tr>
    <tr><td><code>document.getElementsByTagName("tag")</code></td><td><code>let ps = document.getElementsByTagName("p");</code></td><td>Selects by tag name</td></tr>
    <tr><td><code>document.querySelector("selector")</code></td><td><code>let el = document.querySelector(".nav li");</code></td><td>First match (CSS selector)</td></tr>
    <tr><td><code>document.querySelectorAll("selector")</code></td><td><code>let els = document.querySelectorAll("div.box");</code></td><td>All matches (NodeList)</td></tr>
  </tbody>
</table>

## Changing Content

<table>
  <thead><tr><th>Task</th><th>Example</th></tr></thead>
  <tbody>
    <tr><td>Change text</td><td><code>el.textContent = "Hello World";</code></td></tr>
    <tr><td>Change HTML</td><td><code>el.innerHTML = "&lt;b&gt;Hello&lt;/b&gt; World";</code></td></tr>
    <tr><td>Append text</td><td><code>el.textContent += " More text";</code></td></tr>
  </tbody>
</table>

## Changing Style

<table>
  <thead><tr><th>Task</th><th>Example</th></tr></thead>
  <tbody>
    <tr><td>Change color</td><td><code>el.style.color = "red";</code></td></tr>
    <tr><td>Change background</td><td><code>el.style.backgroundColor = "black";</code></td></tr>
    <tr><td>Add multiple styles</td><td><code>el.style.cssText = "color: red; font-size: 20px;";</code></td></tr>
    <tr><td>Add/remove class</td><td><code>el.classList.add("active");</code> / <code>el.classList.remove("hidden");</code></td></tr>
    <tr><td>Toggle class</td><td><code>el.classList.toggle("dark-mode");</code></td></tr>
  </tbody>
</table>

## Creating & Removing Elements

<table>
  <thead><tr><th>Task</th><th>Example</th></tr></thead>
  <tbody>
    <tr><td>Create element</td><td><code>let div = document.createElement("div");</code></td></tr>
    <tr><td>Add text</td><td><code>div.textContent = "New item";</code></td></tr>
    <tr><td>Append to page</td><td><code>document.body.appendChild(div);</code></td></tr>
    <tr><td>Remove element</td><td><code>el.remove();</code></td></tr>
    <tr><td>Clone element</td><td><code>let copy = el.cloneNode(true);</code> (true = deep clone)</td></tr>
  </tbody>
</table>

## Attributes

<table>
  <thead><tr><th>Task</th><th>Example</th></tr></thead>
  <tbody>
    <tr><td>Get attribute</td><td><code>el.getAttribute("src");</code></td></tr>
    <tr><td>Set attribute</td><td><code>el.setAttribute("alt", "image");</code></td></tr>
    <tr><td>Remove attribute</td><td><code>el.removeAttribute("hidden");</code></td></tr>
    <tr><td>Access directly</td><td><code>el.id</code>, <code>el.src</code>, <code>el.href</code>, etc.</td></tr>
  </tbody>
</table>

## Event Handling

<table>
  <thead><tr><th>Task</th><th>Example</th></tr></thead>
  <tbody>
    <tr><td>Click</td><td><code>el.addEventListener("click", fn);</code></td></tr>
    <tr><td>Mouse over</td><td><code>el.addEventListener("mouseover", fn);</code></td></tr>
    <tr><td>Input change</td><td><code>el.addEventListener("input", fn);</code></td></tr>
    <tr><td>Remove listener</td><td><code>el.removeEventListener("click", fn);</code></td></tr>
  </tbody>
</table>

Example:
```js
document.querySelector("button").addEventListener("click", () => {
  alert("Button clicked!");
});
```

## Traversing the DOM

<table>
  <thead><tr><th>Task</th><th>Example</th></tr></thead>
  <tbody>
    <tr><td>Parent</td><td><code>el.parentElement</code></td></tr>
    <tr><td>Children</td><td><code>el.children</code></td></tr>
    <tr><td>First/last child</td><td><code>el.firstElementChild</code>, <code>el.lastElementChild</code></td></tr>
    <tr><td>Next/previous sibling</td><td><code>el.nextElementSibling</code>, <code>el.previousElementSibling</code></td></tr>
  </tbody>
</table>

## Quick Shortcuts

```js
// Shortcuts
const $ = sel => document.querySelector(sel);
const $$ = sel => document.querySelectorAll(sel);

// Example:
$("#myBtn").style.color = "red";
$$("p").forEach(p => p.style.fontSize = "18px");
```

