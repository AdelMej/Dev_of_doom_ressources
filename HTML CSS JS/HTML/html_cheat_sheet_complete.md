# HTML Cheat Sheet — Complete

A compact, printable reference for HTML (elements, attributes, semantics, examples, and best practices).

---

## Quick Boilerplate

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Page title</title>
    <link rel="stylesheet" href="styles.css">
    <script defer src="main.js"></script>
  </head>
  <body>
    <!-- content -->
  </body>
</html>
```

---

## Document Structure

- `<!doctype html>` — HTML5 doctype (required).
- `<html lang="...">` — root element, `lang` important for accessibility and SEO.
- `<head>` — metadata, scripts, styles, title.
- `<body>` — visible content.

---

## Essential `<head>` Elements

- `<meta charset="utf-8">` — character set.
- `<meta name="viewport" content="width=device-width, initial-scale=1">` — responsive layout.
- `<title>` — page title (required for accessibility/SEO).
- `<link rel="stylesheet" href="...">` — include CSS.
- `<script src="..." defer></script>` — include JS (use `defer` or `async` appropriately).
- `<meta name="description" content="...">` — SEO description.
- `<meta name="robots" content="index,follow">` — crawling behavior.
- `<link rel="icon" href="favicon.ico">` — favicon.

---

## Global Attributes (available on most elements)

- `id`, `class` — identify and style elements.
- `style` — inline CSS (avoid for maintainability).
- `title` — tooltip text.
- `hidden` — hides element from rendering.
- `data-*` — custom data attributes.
- `aria-*` — accessibility attributes.

---

## Text Content & Formatting

- Headings: `<h1>` — `<h6>` (semantic importance; one `<h1>` per page recommended).
- Paragraph: `<p>`
- Line break: `<br>`
- Horizontal rule: `<hr>`

Inline formatting:
- `<strong>` — important text (semantic) — typically bold.
- `<b>` — bold (visual only).
- `<em>` — emphasized text (semantic) — typically italic.
- `<i>` — italic (visual only).
- `<mark>` — highlighted text.
- `<small>` — small print.
- `<del>` / `<ins>` — deleted/inserted text.
- `<sub>` / `<sup>` — subscript / superscript.
- `<code>` — inline code.
- `<kbd>` — keyboard input.
- `<samp>` — sample output.
- `<var>` — variable.

---

## Links & Navigation

- `<a href="url">` — anchor (link).
  - `target="_blank"` opens new tab (use `rel="noopener noreferrer"` for security).
  - `download` attribute to suggest file download.
- `<nav>` — semantic container for navigation.

Example:
```html
<a href="/docs.html" title="Docs">Documentation</a>
```

---

## Images & Media

- `<img src="..." alt="..." width="..." height="...">` — images (always include `alt`).
- Responsive images: `<picture>` with `<source>` and `<img>` fallback.
- `<figure>` + `<figcaption>` — image with caption.
- `<audio controls>` — audio playback.
- `<video controls>` — video playback (use `<source>` for multiple formats).

Example:
```html
<figure>
  <img src="photo.jpg" alt="A mountain" loading="lazy">
  <figcaption>Sunrise over the mountain</figcaption>
</figure>
```

---

## Lists

- Unordered: `<ul>` with `<li>`
- Ordered: `<ol>` with `<li>`
- Description: `<dl>` with `<dt>` (term) and `<dd>` (definition)

---

## Tables

```html
<table>
  <caption>Monthly sales</caption>
  <thead>
    <tr><th>Month</th><th>Sales</th></tr>
  </thead>
  <tbody>
    <tr><td>Jan</td><td>$1,000</td></tr>
  </tbody>
  <tfoot>
    <tr><td>Total</td><td>$1,000</td></tr>
  </tfoot>
</table>
```

Best practice: use `<caption>`, `<thead>`, `<tbody>`, `<tfoot>` and proper `<th scope>` for accessibility.

---

## Forms & Inputs

Basic form elements and attributes:
- `<form action="..." method="get|post">`
- `<label for="id">` — associate label with input (clicking label focuses input).
- `<input type="text|email|password|tel|url|search|number|range|checkbox|radio|file|submit|reset|button|date|datetime-local|time|color" />`
- `<textarea>` — multi-line text.
- `<select>` with `<option>` — dropdown.
- `<button type="submit|button|reset">` — button.

Common input attributes:
- `name`, `id`, `value`, `placeholder`, `required`, `readonly`, `disabled`, `min`, `max`, `step`, `pattern`, `maxlength`, `multiple`, `accept` (for files), `autocomplete`.

Example form:
```html
<form action="/submit" method="post">
  <label for="email">Email</label>
  <input id="email" name="email" type="email" required>
  <button type="submit">Send</button>
</form>
```

Validation: prefer native HTML validation (`required`, `type="email"`, `pattern`) and progressively enhance with JS for custom behavior.

---

## Semantic HTML5 Elements

Use these instead of generic `<div>` and `<span>` when appropriate:
- `<header>`, `<footer>`
- `<main>` — main content (only one per page)
- `<nav>` — navigation
- `<section>` — thematic grouping
- `<article>` — independent, self-contained content
- `<aside>` — tangential content
- `<figure>` / `<figcaption>`

---

## Media Embedding

- `<iframe src="..." title="..." width height loading="lazy"></iframe>` — embed other pages (always include `title`).
- `<embed>`, `<object>` — legacy/alternate embedding.
- `<svg>` — inline vector graphics.
- `<canvas>` — drawable area (JS required).

---

## Accessibility (a11y) Basics

- Use semantic elements when possible.
- `alt` text for images.
- `<label>` for form controls; use `aria-label` or `aria-labelledby` if visual label isn't possible.
- Keyboard focus: ensure interactive elements are focusable (`tabindex` if needed).
- Use `role` when providing ARIA semantics.
- Provide captions/transcripts for media when needed.
- Color contrast: ensure readable contrast.

---

## Common Attributes & Techniques

- `loading="lazy"` on `<img>`/`<iframe>` for lazy-loading.
- `decoding="async"` on `<img>` to avoid blocking.
- `rel="noopener noreferrer"` when using `target="_blank"`.
- `prefetch`, `preload`, `dns-prefetch` for performance hints.
- Use `<meta name="theme-color" content="#fff">` to set address-bar color on mobile.

---

## Entities & Special Characters

- `&amp;` `&lt;` `&gt;` `&quot;` `&apos;` `&nbsp;`
- Use `&#xNN;` or `&#NNN;` numeric entities for special characters.

---

## Comments

```html
<!-- This is an HTML comment -->
```

---

## Example Components / Patterns

**Navigation bar**
```html
<nav aria-label="Main navigation">
  <ul>
    <li><a href="/">Home</a></li>
    <li><a href="/about">About</a></li>
    <li><a href="/contact">Contact</a></li>
  </ul>
</nav>
```

**Responsive image**
```html
<picture>
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description" loading="lazy">
</picture>
```

**Accessible form row**
```html
<div>
  <label for="username">Username</label>
  <input id="username" name="username" type="text" required>
</div>
```

---

## SEO & Performance Tips

- Keep semantic structure and headings meaningful.
- Use meaningful `title`, `meta description`, and `og:` tags for social sharing.
- Minify and compress assets (HTML/CSS/JS/images).
- Use `rel="preload"` for critical fonts and scripts.
- Serve images in modern formats (WebP, AVIF) and provide appropriately sized variants.
- Reduce render-blocking resources; use `defer` or `async` on scripts.

---

## Misc: Deprecated / Obsolete Tags (avoid)

- `<font>`, `<center>`, `<big>`, `<strike>`, `<frame>`, `<frameset>` — use CSS and modern layout instead.

---

## Quick Reference: Common Tags

`html, head, meta, title, link, script, style, body, header, nav, main, section, article, aside, footer, h1-h6, p, a, img, picture, source, ul, ol, li, dl, dt, dd, table, thead, tbody, tfoot, tr, th, td, form, label, input, textarea, select, option, button, figure, figcaption, canvas, svg, iframe`

---

## Example: Minimal Landing Page

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Landing</title>
</head>
<body>
  <header>
    <h1>Product Name</h1>
    <nav><a href="#features">Features</a></nav>
  </header>
  <main>
    <section id="hero">
      <h2>Hero headline</h2>
      <p>Short description and CTA.</p>
      <a href="#signup">Get started</a>
    </section>
  </main>
  <footer>
    <p>&copy; 2025 Company</p>
  </footer>
</body>
</html>
```

---

## Learn More / Next Steps

- Practice building small pages and inspect them with browser devtools.
- Read the MDN Web Docs for deep dives on each element and attribute.
- Use Lighthouse or similar tools to measure accessibility and performance.

---

*Want this exported as a printable PDF or a single-file HTML reference? Need a shorter cheat-sheet or one focused on forms, accessibility, or HTML + CSS snippets? Tell me and I’ll adapt it.*

