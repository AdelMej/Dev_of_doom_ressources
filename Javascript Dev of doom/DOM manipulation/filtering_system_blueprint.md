# Full Filtering System Blueprint

A complete, practical blueprint for a **frontend filtering system** suitable for small-to-medium apps.  
Includes UI components, modular architecture, performance tips, accessibility notes, and server-side considerations.

---

## Overview

This blueprint covers:
- Filter bar UI (search, range, multi-select tags)
- Data-driven DOM (using `data-*` attributes)
- Client-side filtering core
- Sorting & pagination
- Event delegation and debouncing
- Accessibility and UX
- Performance optimizations (virtualization, caching)
- Server-side filtering fallback
- Testing checklist

---

## 1. File Structure (modular)

```
/static/js/
  utils.js        // helpers: debounce, parseNumber, qs, qsa
  dom.js          // DOM helpers: createEl, renderList, clearChildren
  api.js          // fetch helpers, caching layer
  filter.js       // core filtering logic + state
  sort.js         // sorting utilities
  pagination.js   // pagination controls
  app.js          // wiring + init
/static/css/
  filters.css
  animations.css
/templates/
  place_card.html // template for a place card (server-side or client)
```

---

## 2. HTML: Filter Bar + Container

```html
<div class="filters">
  <input id="search" type="search" placeholder="Search places" aria-label="Search places" />
  <input id="priceMin" type="number" placeholder="Min price" />
  <input id="priceMax" type="number" placeholder="Max price" />
  <div id="tags" role="group" aria-label="Amenities">
    <button data-tag="wifi">Wi‑Fi</button>
    <button data-tag="parking">Parking</button>
    <button data-tag="kitchen">Kitchen</button>
  </div>
  <select id="sort">
    <option value="price-asc">Price ↑</option>
    <option value="price-desc">Price ↓</option>
    <option value="name-asc">Name A→Z</option>
  </select>
</div>

<section id="places" class="places-grid" aria-live="polite">
  <!-- place cards appended here -->
</section>

<nav id="pagination" aria-label="Pagination"></nav>
```

Each place card should include data attributes:

```html
<div class="place-card" data-id="..." data-price="120" data-name="Cozy Cottage" data-tags="wifi,kitchen" role="article">
  <!-- inner markup -->
</div>
```

---

## 3. Utilities (utils.js)

```js
export function qs(sel, root = document) { return root.querySelector(sel); }
export function qsa(sel, root = document) { return [...root.querySelectorAll(sel)]; }

export function debounce(fn, wait = 200) {
  let t;
  return (...args) => {
    clearTimeout(t);
    t = setTimeout(() => fn(...args), wait);
  };
}

export function parseNumber(v) {
  const n = Number(v);
  return Number.isFinite(n) ? n : null;
}
```

---

## 4. API Helper with Cache (api.js)

```js
export async function fetchJSON(url, options = {}) {
  const res = await fetch(url, options);
  if (!res.ok) throw new Error(`HTTP ${res.status}`);
  return res.json();
}

// Optional simple cache
const cache = new Map();
export async function cachedFetch(url) {
  if (cache.has(url)) return cache.get(url);
  const data = await fetchJSON(url);
  cache.set(url, data);
  return data;
}
```

---

## 5. Rendering (dom.js)

```js
export function createCard(place) {
  const div = document.createElement('div');
  div.className = 'place-card fade';
  div.dataset.id = place.id;
  div.dataset.price = String(place.price);
  div.dataset.name = place.name;
  div.dataset.tags = (place.tags || []).join(',');
  // Fill inner markup (title, image, btns)
  return div;
}

export function renderList(container, items) {
  container.innerHTML = '';
  const frag = document.createDocumentFragment();
  for (const it of items) frag.appendChild(createCard(it));
  container.appendChild(frag);
}
```

---

## 6. Filter State and Core Logic (filter.js)

```js
import { qsa, qs, debounce, parseNumber } from './utils.js';

const state = {
  search: '',
  minPrice: null,
  maxPrice: null,
  tags: new Set(),
  sort: 'price-asc',
  page: 1,
  perPage: 20,
  items: [] // loaded places
};

export function initFilters(items) {
  state.items = items;
  applyFilters();
}

export function applyFilters() {
  const filtered = state.items.filter(item => {
    if (state.search) {
      if (!item.name.toLowerCase().includes(state.search.toLowerCase())) return false;
    }
    if (state.minPrice !== null && item.price < state.minPrice) return false;
    if (state.maxPrice !== null && item.price > state.maxPrice) return false;
    if (state.tags.size) {
      const tags = item.tags || [];
      for (const t of state.tags) if (!tags.includes(t)) return false;
    }
    return true;
  });

  const sorted = sortItems(filtered, state.sort);
  const pageItems = paginate(sorted, state.page, state.perPage);
  renderPage(pageItems);
  renderPagination(Math.ceil(sorted.length / state.perPage), state.page);
}
```

---

## 7. Sorting Utilities (sort.js)

```js
export function sortItems(items, mode) {
  const arr = [...items];
  switch(mode) {
    case 'price-asc': arr.sort((a,b)=>a.price-b.price); break;
    case 'price-desc': arr.sort((a,b)=>b.price-a.price); break;
    case 'name-asc': arr.sort((a,b)=>a.name.localeCompare(b.name)); break;
  }
  return arr;
}
```

---

## 8. Pagination (pagination.js)

```js
export function paginate(items, page=1, per=20) {
  const start = (page-1)*per;
  return items.slice(start, start+per);
}

export function renderPagination(totalPages, currentPage) {
  const nav = qs('#pagination');
  nav.innerHTML = '';
  for (let i=1;i<=totalPages;i++) {
    const btn = document.createElement('button');
    btn.innerText = i;
    if (i===currentPage) btn.disabled = true;
    btn.addEventListener('click', ()=> { state.page=i; applyFilters(); });
    nav.appendChild(btn);
  }
}
```

---

## 9. Wiring & Event Delegation (app.js)

```js
import { qs, qsa, debounce, parseNumber } from './utils.js';
import { initFilters, applyFilters } from './filter.js';

const search = qs('#search');
const priceMin = qs('#priceMin');
const priceMax = qs('#priceMax');
const tags = qs('#tags');

search.addEventListener('input', debounce(e=>{
  state.search = e.target.value; state.page=1; applyFilters();
}, 200));

priceMin.addEventListener('input', e => { state.minPrice = parseNumber(e.target.value); state.page=1; applyFilters(); });
priceMax.addEventListener('input', e => { state.maxPrice = parseNumber(e.target.value); state.page=1; applyFilters(); });

tags.addEventListener('click', e => {
  const btn = e.target.closest('button[data-tag]');
  if (!btn) return;
  const tag = btn.dataset.tag;
  if (state.tags.has(tag)) { state.tags.delete(tag); btn.classList.remove('active'); }
  else { state.tags.add(tag); btn.classList.add('active'); }
  state.page=1; applyFilters();
});
```

---

## 10. Accessibility & UX

- Inputs should have `aria-label` or visible labels.  
- Use `aria-live="polite"` on the results container for screen readers.  
- Keyboard navigation: make tag buttons focusable and actionable with Enter/Space.  
- Provide feedback (loading spinner) when fetching large datasets.  
- Announce result counts (e.g., “12 results found”).

---

## 11. Performance & Scaling

**Client-side filtering is fine** for hundreds → low thousands of items. For larger datasets:

- **Server-side filtering**: query backend with filter params.  
- **Pagination / Windowing**: only render current page.  
- **Virtualization**: use windowing (e.g., virtual-scroll) to render only visible DOM nodes.  
- **Indexing**: precompute lowercase names, tag sets, numeric fields for faster comparisons.  
- **Debounce** input events to reduce work.

---

## 12. Server-side Filtering (fallback)

API example:

```
GET /api/v1/places?search=cozy&min_price=10&max_price=200&tags=wifi,kitchen&page=1&per_page=20&sort=price-asc
```

Server returns:
```json
{
  "items": [...],
  "total": 123,
  "page": 1,
  "per_page": 20
}
```

Frontend should:
- call server when dataset large or when filters change,
- update state.items with response.items,
- render and paginate using server totals.

---

## 13. Testing & Debugging Checklist

- [ ] Filter by name (case-insensitive)  
- [ ] Filter by min/max price (edge cases: empty, non-number)  
- [ ] Tag filtering (multiple tags)  
- [ ] Sorting modes  
- [ ] Pagination boundaries (last page, empty results)  
- [ ] Accessibility checks (screen reader announces changes)  
- [ ] Performance test with X items (measure render time)  
- [ ] Network failure handling (show error message)

---

## 14. Example: Minimal Integration Flow

1. On load: `const data = await cachedFetch('/api/v1/places');`
2. `initFilters(data)`
3. Wire inputs (app.js)
4. `applyFilters()` after every control change

---

## 15. Final Notes

- Prefer simple, well-named functions.  
- Keep state immutable where possible.  
- Use `dataset` for element metadata.  
- Avoid manipulating the DOM in deep nested loops — use `DocumentFragment`.  
- Profile with browser devtools if slowness appears.

---

If you want, I can generate:  
- A downloadable `.zip` with all files scaffolded, or  
- A single `.md` file (this one), or  
- A ready-to-drop JS/CSS/HTML scaffold.  
