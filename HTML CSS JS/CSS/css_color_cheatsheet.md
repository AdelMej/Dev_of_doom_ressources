# 🎨 CSS Color System Cheat Sheet

A compact reference for managing colors in your project using custom properties (CSS variables).

## 🎯 1. Color Variable Naming

Use a predictable naming structure:

```
--color-{role}-{variant}
```

**Roles**
- bg → background
- text → text
- border → borders/dividers
- accent → primary branding color
- muted → low-contrast elements
- surface → cards, panels

**Variants**
- primary, secondary
- light, dark
- hover, active

## 🎨 2. Example Neutral Palette (Slate-Based)

```
--color-bg: #f8fafc;
--color-bg-secondary: #f1f5f9;

--color-text: #0f172a;
--color-text-muted: #475569;

--color-border: #e2e8f0;
--color-border-strong: #cbd5e1;

--color-surface: #ffffff;
--color-surface-alt: #f8fafc;
```

## 🌈 3. Accent Colors (Optional)

```
--color-accent: #3b82f6;
--color-accent-hover: #2563eb;
--color-accent-light: #93c5fd;
--color-accent-bg: #eff6ff;
```

## 📌 4. Semantic Colors

```
--color-success: #22c55e;
--color-success-bg: #ecfdf5;

--color-warning: #f59e0b;
--color-warning-bg: #fffbeb;

--color-error: #ef4444;
--color-error-bg: #fef2f2;

--color-info: #0ea5e9;
--color-info-bg: #f0f9ff;
```

## 🎛 5. Usage Examples

### Background
```css
body {
  background: var(--color-bg);
  color: var(--color-text);
}
```

### Card
```css
.card {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
}
```

### Accent Button
```css
.button-primary {
  background: var(--color-accent);
  color: white;
}

.button-primary:hover {
  background: var(--color-accent-hover);
}
```

## 🧪 6. Light/Dark Mode Structure (Recommended)

```
:root {
  --color-bg: #f8fafc;
  --color-text: #0f172a;
  --color-surface: #ffffff;
}

[data-theme="dark"] {
  --color-bg: #0f172a;
  --color-text: #f8fafc;
  --color-surface: #1e293b;
}
```
