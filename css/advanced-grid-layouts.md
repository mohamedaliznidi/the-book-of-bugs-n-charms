---
description: >-
  Advanced CSS Grid techniques for complex layouts including subgrid, named
  grid areas, dynamic grid sizing, and modern responsive design patterns.
  Master professional grid layouts for modern web applications.
---

# Advanced CSS Grid Layouts

## Introduction

CSS Grid provides powerful layout capabilities beyond basic grid structures. Advanced techniques include subgrid, named grid areas, dynamic sizing, and complex responsive patterns that enable professional-grade layouts for modern web applications.

## Use Cases

1. **Complex Page Layouts**: Multi-column layouts with dynamic content areas
2. **Dashboard Interfaces**: Flexible widget arrangements with varying sizes
3. **Magazine-Style Layouts**: Complex editorial designs with overlapping content
4. **Responsive Design Systems**: Adaptive layouts that work across all screen sizes

## Named Grid Areas

### 1. Basic Named Areas

```css
.page-layout {
  display: grid;
  grid-template-areas:
    "header header header"
    "sidebar main aside"
    "footer footer footer";
  grid-template-columns: 250px 1fr 200px;
  grid-template-rows: auto 1fr auto;
  min-height: 100vh;
  gap: 1rem;
}

.header {
  grid-area: header;
  background: #333;
  color: white;
  padding: 1rem;
}

.sidebar {
  grid-area: sidebar;
  background: #f5f5f5;
  padding: 1rem;
}

.main-content {
  grid-area: main;
  padding: 1rem;
}

.aside {
  grid-area: aside;
  background: #f5f5f5;
  padding: 1rem;
}

.footer {
  grid-area: footer;
  background: #333;
  color: white;
  padding: 1rem;
  text-align: center;
}
```

### 2. Responsive Named Areas

```css
.dashboard {
  display: grid;
  gap: 1rem;
  padding: 1rem;
}

/* Mobile layout */
@media (max-width: 768px) {
  .dashboard {
    grid-template-areas:
      "header"
      "stats"
      "chart"
      "recent"
      "sidebar";
    grid-template-columns: 1fr;
  }
}

/* Tablet layout */
@media (min-width: 769px) and (max-width: 1024px) {
  .dashboard {
    grid-template-areas:
      "header header"
      "stats chart"
      "recent sidebar";
    grid-template-columns: 1fr 1fr;
  }
}

/* Desktop layout */
@media (min-width: 1025px) {
  .dashboard {
    grid-template-areas:
      "header header header header"
      "sidebar stats stats chart"
      "sidebar recent recent chart";
    grid-template-columns: 250px 1fr 1fr 300px;
    grid-template-rows: auto 1fr 1fr;
  }
}

.dashboard-header { grid-area: header; }
.dashboard-sidebar { grid-area: sidebar; }
.dashboard-stats { grid-area: stats; }
.dashboard-chart { grid-area: chart; }
.dashboard-recent { grid-area: recent; }
```

## Dynamic Grid Sizing

### 1. Auto-Fit and Auto-Fill

```css
/* Auto-fit: Columns stretch to fill available space */
.gallery-auto-fit {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}

/* Auto-fill: Maintains column size, creates empty columns if needed */
.gallery-auto-fill {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  gap: 1rem;
}

/* Dynamic sizing with constraints */
.product-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(min(250px, 100%), 1fr));
  gap: 1rem;
  padding: 1rem;
}

.product-card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.2s;
}

.product-card:hover {
  transform: translateY(-4px);
}
```

### 2. Intrinsic Sizing

```css
.flexible-layout {
  display: grid;
  grid-template-columns: 
    max-content    /* Size to content */
    1fr           /* Fill remaining space */
    min-content   /* Minimum size needed */
    fit-content(200px); /* Content size, max 200px */
  gap: 1rem;
}

/* Dynamic rows based on content */
.masonry-style {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  grid-auto-rows: min-content;
  gap: 1rem;
}

.masonry-item {
  background: white;
  border-radius: 8px;
  padding: 1rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
```

## Complex Layout Patterns

### 1. Magazine-Style Layout

```css
.magazine-layout {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-auto-rows: 100px;
  gap: 1rem;
  padding: 1rem;
}

.hero-article {
  grid-column: 1 / 9;
  grid-row: 1 / 4;
  background: linear-gradient(rgba(0,0,0,0.4), rgba(0,0,0,0.4)),
              url('hero-image.jpg') center/cover;
  color: white;
  display: flex;
  align-items: end;
  padding: 2rem;
  border-radius: 12px;
}

.featured-article {
  grid-column: 9 / 13;
  grid-row: 1 / 3;
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.sidebar-widget {
  grid-column: 9 / 13;
  grid-row: 3 / 4;
  background: #f8f9fa;
  border-radius: 8px;
  padding: 1rem;
}

.article-grid {
  grid-column: 1 / 9;
  grid-row: 4 / 7;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}

.trending-section {
  grid-column: 9 / 13;
  grid-row: 4 / 7;
  background: white;
  border-radius: 8px;
  padding: 1rem;
}
```

### 2. Dashboard Grid System

```css
.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(24, 1fr);
  grid-auto-rows: 60px;
  gap: 1rem;
  padding: 1rem;
}

/* Widget size classes */
.widget-small {
  grid-column: span 6;
  grid-row: span 4;
}

.widget-medium {
  grid-column: span 8;
  grid-row: span 6;
}

.widget-large {
  grid-column: span 12;
  grid-row: span 8;
}

.widget-wide {
  grid-column: span 16;
  grid-row: span 4;
}

.widget-tall {
  grid-column: span 6;
  grid-row: span 10;
}

/* Responsive widget sizing */
@media (max-width: 1200px) {
  .dashboard-grid {
    grid-template-columns: repeat(12, 1fr);
  }
  
  .widget-small { grid-column: span 6; }
  .widget-medium { grid-column: span 6; }
  .widget-large { grid-column: span 12; }
  .widget-wide { grid-column: span 12; }
  .widget-tall { grid-column: span 6; }
}

@media (max-width: 768px) {
  .dashboard-grid {
    grid-template-columns: 1fr;
  }
  
  .widget-small,
  .widget-medium,
  .widget-large,
  .widget-wide,
  .widget-tall {
    grid-column: 1;
    grid-row: span 6;
  }
}
```

### 3. Overlapping Grid Elements

```css
.hero-section {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: repeat(8, 50px);
  gap: 1rem;
  position: relative;
}

.hero-background {
  grid-column: 1 / -1;
  grid-row: 1 / -1;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  border-radius: 12px;
}

.hero-content {
  grid-column: 2 / 8;
  grid-row: 2 / 7;
  z-index: 2;
  display: flex;
  flex-direction: column;
  justify-content: center;
  color: white;
  padding: 2rem;
}

.hero-image {
  grid-column: 8 / 12;
  grid-row: 1 / 8;
  z-index: 3;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.floating-card {
  grid-column: 10 / 13;
  grid-row: 6 / 9;
  z-index: 4;
  background: white;
  border-radius: 8px;
  padding: 1.5rem;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
}
```

## Subgrid (Where Supported)

### 1. Basic Subgrid Usage

```css
.main-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(3, auto);
  gap: 1rem;
}

.subgrid-container {
  grid-column: 2 / 4;
  grid-row: 1 / 3;
  
  /* Inherit parent grid structure */
  display: grid;
  grid-template-columns: subgrid;
  grid-template-rows: subgrid;
  gap: inherit;
}

.subgrid-item {
  background: #f0f0f0;
  padding: 1rem;
  border-radius: 4px;
}
```

### 2. Card Grid with Subgrid

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.card {
  display: grid;
  grid-template-rows: subgrid;
  grid-row: span 4;
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

.card-image {
  grid-row: 1;
  height: 200px;
  object-fit: cover;
}

.card-content {
  grid-row: 2;
  padding: 1.5rem;
}

.card-actions {
  grid-row: 3;
  padding: 1rem 1.5rem;
  border-top: 1px solid #eee;
  margin-top: auto;
}
```

## Performance Optimization

### 1. Efficient Grid Structures

```css
/* Efficient: Use simple grid structures */
.efficient-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}

/* Avoid: Overly complex nested grids */
.avoid-complex {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
}

.avoid-complex .item {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
}

.avoid-complex .item .nested {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
```

### 2. Responsive Grid Optimization

```css
.optimized-grid {
  display: grid;
  gap: 1rem;
}

/* Use container queries for better performance */
@container (min-width: 600px) {
  .optimized-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

@container (min-width: 900px) {
  .optimized-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

@container (min-width: 1200px) {
  .optimized-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

## Best Practices

- **Named Areas**: Use named grid areas for complex layouts to improve readability
- **Responsive Design**: Combine grid with container queries for optimal responsive behavior
- **Performance**: Avoid deeply nested grids; use flat structures when possible
- **Accessibility**: Ensure grid layouts work with screen readers and keyboard navigation
- **Fallbacks**: Provide fallbacks for older browsers that don't support advanced grid features
- **Testing**: Test layouts across different screen sizes and content lengths

## Common Pitfalls

### Issue 1: Grid Item Overflow
Grid items can overflow their containers without proper sizing.

**Solution:**
Use `minmax()` and `min()` functions:
```css
/* ❌ Wrong - can cause overflow */
.grid {
  grid-template-columns: repeat(auto-fit, 300px);
}

/* ✅ Correct - responsive sizing */
.grid {
  grid-template-columns: repeat(auto-fit, minmax(min(300px, 100%), 1fr));
}
```

### Issue 2: Implicit Grid Confusion
Items placed outside explicit grid create implicit tracks.

**Solution:**
Define explicit grid structure or use `grid-auto-*` properties:
```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: minmax(100px, auto);
  grid-auto-flow: row dense;
}
```

### Issue 3: Accessibility Issues
Complex grid layouts can confuse screen readers.

**Solution:**
Use proper semantic HTML and ARIA labels:
```html
<div class="grid" role="grid" aria-label="Product catalog">
  <div class="grid-item" role="gridcell" tabindex="0">
    <h3>Product Title</h3>
    <p>Product description</p>
  </div>
</div>
```

## References

{% embed url="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout" %}

{% embed url="https://css-tricks.com/snippets/css/complete-guide-grid/" %}

{% embed url="https://gridbyexample.com/" %}
