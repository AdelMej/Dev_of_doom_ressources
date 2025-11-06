# 🧩 CSS Pseudo-Elements Cheat Sheet

## 🎯 Basic Syntax
```css
selector::pseudo-element {
  property: value;
}
```
> ✅ Use **two colons (::)** for pseudo-elements (modern standard).  
> ⚠️ Older browsers used one colon (`:before` instead of `::before`) — both often work.

---

## 🔹 ::before
Inserts content **before** an element’s content.

```css
h1::before {
  content: "★ ";
  color: gold;
}
```
✅ Common uses:
- Decorative icons
- Prefix text or symbols

---

## 🔹 ::after
Inserts content **after** an element’s content.

```css
h1::after {
  content: " ✦";
  color: silver;
}
```
✅ Common uses:
- Clearfix hacks (like `.row::after { content: ""; display: table; clear: both; }`)
- Decorative accents or separators

---

## 🔹 ::first-letter
Targets the **first letter** of a block of text.

```css
p::first-letter {
  font-size: 2em;
  font-weight: bold;
  color: crimson;
}
```
✅ Common uses:
- Drop caps (fancy first letter styles in articles)

---

## 🔹 ::first-line
Targets the **first line** of a block of text.

```css
p::first-line {
  text-transform: uppercase;
  letter-spacing: 1px;
}
```
✅ Common uses:
- Styling intro lines in paragraphs

---

## 🔹 ::selection
Targets text selected by the user.

```css
::selection {
  background: #d73953;
  color: white;
}
```
✅ Common uses:
- Custom highlight color

---

## 🔹 ::placeholder
Targets placeholder text inside input fields.

```css
input::placeholder {
  color: #999;
  font-style: italic;
}
```
✅ Common uses:
- Styling form placeholders

---

## 🔹 ::marker
Targets bullet or numbering in lists.

```css
li::marker {
  color: #d73953;
  font-size: 1.2em;
}
```
✅ Common uses:
- Customize list bullets or numbers

---

## 🔹 ::file-selector-button
Styles the **button** part of `<input type="file">`.

```css
input[type="file"]::file-selector-button {
  background: #d73953;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
}
```

---

## 🔹 ::backdrop
Styles the backdrop of full-screen elements (e.g. modals).

```css
::backdrop {
  background-color: rgba(0, 0, 0, 0.6);
}
```

---

## 🔹 ::cue (for captions in video/audio)
```css
::cue {
  color: yellow;
  background: black;
}
```

---

## ⚙️ Special / Rare
| Pseudo-Element | Description |
|----------------|--------------|
| `::part()` | Targets shadow-DOM parts (web components) |
| `::slotted()` | Styles slotted content in shadow DOM |
| `::spelling-error` | (Experimental) highlights spelling errors |
| `::grammar-error` | (Experimental) highlights grammar errors |

---

## 💡 Pro Tips
- `content:` is **required** for `::before` and `::after`.
- You can use emojis, Unicode, or even images:
  ```css
  h2::before {
    content: url(icon.svg);
  }
  ```
- Pseudo-elements **don’t exist in the DOM** — they’re visual only.
- Can be combined with pseudo-classes:
  ```css
  a:hover::after {
    content: " →";
  }
  ```
