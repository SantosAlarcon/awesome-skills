# Responsive Design Basics

## Description
Core principles and techniques for building layouts that work across all screen sizes, from mobile to desktop.

## When to Use
- Starting a new project layout
- Converting fixed-width designs to responsive
- Debugging layout issues on specific devices
- Building component libraries with responsive behavior

## How to Use

1. **Mobile-first approach**: Start with mobile styles, add breakpoints for larger screens
2. **Use relative units**: rem, em, %, vw, vh instead of fixed pixels
3. **Fluid typography**: Scale text with clamp()
4. **Flexible grid**: CSS Grid and Flexbox with auto-fit/auto-fill
5. **Test at breakpoints**: Common breakpoints: 640px, 768px, 1024px, 1280px

## Examples

### Mobile-First Media Queries
```css
.container {
  width: 100%;
  padding: 0 1rem;
}

@media (min-width: 768px) {
  .container {
    max-width: 720px;
    margin: 0 auto;
    padding: 0 2rem;
  }
}

@media (min-width: 1024px) {
  .container {
    max-width: 960px;
  }
}
```

### Fluid Typography with clamp()
```css
.heading {
  font-size: clamp(1.5rem, 4vw + 1rem, 3rem);
  line-height: 1.2;
}
```

### Auto-Fit Grid
```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
}
```

### Responsive Navigation
```css
.nav {
  display: flex;
  flex-direction: column;
}

@media (min-width: 768px) {
  .nav {
    flex-direction: row;
    justify-content: space-between;
  }
  
  .nav-toggle {
    display: none;
  }
  
  .nav-menu {
    display: flex;
    gap: 2rem;
  }
}
```

### Flexible Images
```css
img {
  max-width: 100%;
  height: auto;
  display: block;
}
```

### Picture Element for Art Direction
```html
<picture>
  <source media="(min-width: 1024px)" srcset="hero-desktop.jpg">
  <source media="(min-width: 640px)" srcset="hero-tablet.jpg">
  <img src="hero-mobile.jpg" alt="Hero image">
</picture>
```

## Best Practices

- Use CSS custom properties for design tokens (colors, spacing, fonts)
- Avoid fixed heights on containers — use min-height instead
- Test touch targets: minimum 44x44px on mobile
- Consider reduced motion for users who prefer it
- Use DevTools device toolbar to test before deploying

## Related Skills
- [Frontend Development](../frontend/) — UI component patterns
