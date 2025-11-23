# 📱 Media Queries Cheat Sheet

## 🎯 Basic Syntax

```css
@media (condition) {
  /* styles here */
}
```

## 📏 Most Common Breakpoints

```css
@media (max-width: 480px) {}
@media (max-width: 768px) {}
@media (max-width: 992px) {}
@media (max-width: 1200px) {}
@media (max-width: 1600px) {}
```

## 🔄 Mobile-First Strategy

```css
@media (min-width: 768px) {}
@media (min-width: 992px) {}
@media (min-width: 1200px) {}
```

## 🎛️ Useful Conditions

### Width
```css
@media (max-width: 600px) {}
@media (min-width: 600px) {}
@media (min-width: 600px) and (max-width: 1200px) {}
```

### Height
```css
@media (max-height: 700px) {}
```

### Orientation
```css
@media (orientation: landscape) {}
@media (orientation: portrait) {}
```

### Dark mode
```css
@media (prefers-color-scheme: dark) {}
```

### Reduced motion
```css
@media (prefers-reduced-motion: reduce) {}
```

## 🔥 Examples

### Change grid layout
```css
@media (max-width: 768px) {
  .layout {
    grid-template-columns: 1fr;
    grid-template-areas:
      "nav"
      "main"
      "aside"
      "footer";
  }
}
```

### Hide on mobile
```css
@media (max-width: 600px) {
  .sidebar {
    display: none;
  }
}
```

## 🧠 SCSS Mixins

```scss
$sm: 480px;
$md: 768px;
$lg: 992px;
$xl: 1200px;

@mixin sm { @media (min-width: $sm) { @content; } }
@mixin md { @media (min-width: $md) { @content; } }
@mixin lg { @media (min-width: $lg) { @content; } }
@mixin xl { @media (min-width: $xl) { @content; } }

.container {
  width: 100%;
  @include md { width: 80%; }
  @include lg { width: 60%; }
}
```
