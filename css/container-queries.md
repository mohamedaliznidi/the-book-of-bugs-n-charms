---
description: >-
  Complete guide to CSS Container Queries for responsive component design.
  Learn how to create components that adapt to their container size rather
  than viewport size, enabling true component-based responsive design.
---

# Container Queries

## Introduction

Container Queries allow elements to respond to the size of their containing element rather than the viewport size. This enables true component-based responsive design, where components can adapt their layout based on the space available to them, regardless of the overall page layout.

## Use Cases

1. **Component Libraries**: Create reusable components that adapt to any container size
2. **Card Layouts**: Design cards that work in different grid configurations
3. **Sidebar Components**: Components that adapt when moved between main content and sidebar
4. **Modular Design Systems**: Build truly responsive components independent of page layout

## Basic Container Query Setup

### 1. Container Type and Size

```css
/* Define a container context */
.card-container {
  container-type: inline-size; /* Query based on inline dimension (width) */
  /* container-type: size; Query based on both dimensions */
  /* container-type: normal; Default, no container queries */
}

/* Query the container size */
@container (min-width: 300px) {
  .card {
    display: flex;
    flex-direction: row;
  }
  
  .card-image {
    width: 150px;
    height: 100px;
  }
  
  .card-content {
    flex: 1;
    padding-left: 1rem;
  }
}

@container (max-width: 299px) {
  .card {
    display: flex;
    flex-direction: column;
  }
  
  .card-image {
    width: 100%;
    height: 200px;
  }
  
  .card-content {
    padding-top: 1rem;
  }
}
```

### 2. Named Containers

```css
/* Named container for specific targeting */
.sidebar {
  container-name: sidebar;
  container-type: inline-size;
}

.main-content {
  container-name: main;
  container-type: inline-size;
}

/* Query specific named containers */
@container sidebar (max-width: 250px) {
  .widget {
    font-size: 0.875rem;
    padding: 0.5rem;
  }
  
  .widget-title {
    font-size: 1rem;
  }
}

@container main (min-width: 600px) {
  .article {
    columns: 2;
    column-gap: 2rem;
  }
}
```

## Practical Examples

### 1. Responsive Card Component

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}

.card-wrapper {
  container-type: inline-size;
}

.card {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

/* Small card layout */
@container (max-width: 300px) {
  .card {
    text-align: center;
  }
  
  .card-image {
    width: 100%;
    height: 150px;
    object-fit: cover;
  }
  
  .card-content {
    padding: 1rem;
  }
  
  .card-title {
    font-size: 1.125rem;
    margin-bottom: 0.5rem;
  }
  
  .card-description {
    font-size: 0.875rem;
    color: #666;
    line-height: 1.4;
  }
  
  .card-actions {
    padding: 1rem;
    border-top: 1px solid #eee;
  }
  
  .card-button {
    width: 100%;
    padding: 0.5rem;
    font-size: 0.875rem;
  }
}

/* Medium card layout */
@container (min-width: 301px) and (max-width: 500px) {
  .card {
    display: flex;
    align-items: stretch;
  }
  
  .card-image {
    width: 120px;
    height: 120px;
    object-fit: cover;
    flex-shrink: 0;
  }
  
  .card-body {
    flex: 1;
    display: flex;
    flex-direction: column;
  }
  
  .card-content {
    flex: 1;
    padding: 1rem;
  }
  
  .card-title {
    font-size: 1.25rem;
    margin-bottom: 0.5rem;
  }
  
  .card-description {
    font-size: 0.875rem;
    color: #666;
    line-height: 1.5;
  }
  
  .card-actions {
    padding: 1rem;
    border-top: 1px solid #eee;
  }
}

/* Large card layout */
@container (min-width: 501px) {
  .card-image {
    width: 100%;
    height: 200px;
    object-fit: cover;
  }
  
  .card-content {
    padding: 1.5rem;
  }
  
  .card-title {
    font-size: 1.5rem;
    margin-bottom: 0.75rem;
  }
  
  .card-description {
    font-size: 1rem;
    color: #666;
    line-height: 1.6;
    margin-bottom: 1rem;
  }
  
  .card-meta {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.875rem;
    color: #888;
    margin-bottom: 1rem;
  }
  
  .card-actions {
    display: flex;
    gap: 0.5rem;
    padding: 1.5rem;
    border-top: 1px solid #eee;
  }
  
  .card-button {
    flex: 1;
    padding: 0.75rem;
  }
}
```

### 2. Adaptive Navigation Component

```css
.navigation-wrapper {
  container-name: nav;
  container-type: inline-size;
}

.navigation {
  background: #333;
  color: white;
}

/* Horizontal navigation for wide containers */
@container nav (min-width: 600px) {
  .nav-list {
    display: flex;
    list-style: none;
    margin: 0;
    padding: 0;
  }
  
  .nav-item {
    flex: 1;
  }
  
  .nav-link {
    display: block;
    padding: 1rem 1.5rem;
    text-decoration: none;
    color: white;
    text-align: center;
    border-right: 1px solid #555;
    transition: background-color 0.2s;
  }
  
  .nav-link:hover {
    background-color: #555;
  }
  
  .nav-item:last-child .nav-link {
    border-right: none;
  }
  
  .nav-toggle {
    display: none;
  }
}

/* Collapsible navigation for narrow containers */
@container nav (max-width: 599px) {
  .nav-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
  }
  
  .nav-toggle {
    background: none;
    border: none;
    color: white;
    font-size: 1.5rem;
    cursor: pointer;
  }
  
  .nav-list {
    display: none;
    list-style: none;
    margin: 0;
    padding: 0;
  }
  
  .nav-list.is-open {
    display: block;
  }
  
  .nav-link {
    display: block;
    padding: 0.75rem 1rem;
    text-decoration: none;
    color: white;
    border-bottom: 1px solid #555;
    transition: background-color 0.2s;
  }
  
  .nav-link:hover {
    background-color: #555;
  }
}
```

### 3. Dashboard Widget System

```css
.dashboard {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1rem;
  padding: 1rem;
}

.widget {
  container-type: inline-size;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

/* Compact widget layout */
@container (max-width: 350px) {
  .widget-header {
    padding: 1rem;
    border-bottom: 1px solid #eee;
  }
  
  .widget-title {
    font-size: 1rem;
    margin: 0;
  }
  
  .widget-content {
    padding: 1rem;
  }
  
  .widget-chart {
    height: 150px;
  }
  
  .widget-stats {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }
  
  .stat-item {
    text-align: center;
    padding: 0.5rem;
    background: #f8f9fa;
    border-radius: 4px;
  }
  
  .stat-value {
    font-size: 1.25rem;
    font-weight: bold;
    color: #333;
  }
  
  .stat-label {
    font-size: 0.75rem;
    color: #666;
    text-transform: uppercase;
  }
}

/* Standard widget layout */
@container (min-width: 351px) and (max-width: 500px) {
  .widget-header {
    padding: 1.5rem;
    border-bottom: 1px solid #eee;
  }
  
  .widget-title {
    font-size: 1.25rem;
    margin: 0;
  }
  
  .widget-content {
    padding: 1.5rem;
  }
  
  .widget-chart {
    height: 200px;
  }
  
  .widget-stats {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
  }
  
  .stat-item {
    text-align: center;
    padding: 1rem;
    background: #f8f9fa;
    border-radius: 6px;
  }
  
  .stat-value {
    font-size: 1.5rem;
    font-weight: bold;
    color: #333;
  }
  
  .stat-label {
    font-size: 0.875rem;
    color: #666;
    text-transform: uppercase;
    margin-top: 0.25rem;
  }
}

/* Expanded widget layout */
@container (min-width: 501px) {
  .widget-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 2rem;
    border-bottom: 1px solid #eee;
  }
  
  .widget-title {
    font-size: 1.5rem;
    margin: 0;
  }
  
  .widget-actions {
    display: flex;
    gap: 0.5rem;
  }
  
  .widget-content {
    padding: 2rem;
  }
  
  .widget-chart {
    height: 300px;
    margin-bottom: 2rem;
  }
  
  .widget-stats {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
    gap: 1rem;
  }
  
  .stat-item {
    text-align: center;
    padding: 1.5rem;
    background: #f8f9fa;
    border-radius: 8px;
  }
  
  .stat-value {
    font-size: 2rem;
    font-weight: bold;
    color: #333;
  }
  
  .stat-label {
    font-size: 1rem;
    color: #666;
    text-transform: uppercase;
    margin-top: 0.5rem;
  }
  
  .stat-change {
    font-size: 0.875rem;
    margin-top: 0.25rem;
  }
  
  .stat-change.positive {
    color: #28a745;
  }
  
  .stat-change.negative {
    color: #dc3545;
  }
}
```

## Advanced Techniques

### 1. Container Query Units

```css
.responsive-text {
  container-type: inline-size;
}

@container (min-width: 200px) {
  .text-content {
    /* Container query units */
    font-size: 4cqw; /* 4% of container width */
    line-height: 1.2;
    padding: 2cqh 3cqw; /* 2% of container height, 3% of container width */
  }
  
  .heading {
    font-size: 8cqw;
    margin-bottom: 2cqh;
  }
}
```

### 2. Combining with CSS Grid

```css
.layout-grid {
  display: grid;
  grid-template-columns: 1fr 300px;
  gap: 2rem;
}

.main-content {
  container-name: main;
  container-type: inline-size;
}

.sidebar {
  container-name: sidebar;
  container-type: inline-size;
}

/* Main content adapts to available space */
@container main (min-width: 600px) {
  .article-grid {
    display: grid;
    grid-template-columns: 2fr 1fr;
    gap: 2rem;
  }
}

@container main (max-width: 599px) {
  .article-grid {
    display: block;
  }
  
  .article-aside {
    margin-top: 2rem;
    padding-top: 2rem;
    border-top: 1px solid #eee;
  }
}

/* Sidebar components adapt independently */
@container sidebar (max-width: 250px) {
  .sidebar-widget {
    font-size: 0.875rem;
  }
  
  .sidebar-widget .widget-title {
    font-size: 1rem;
  }
}
```

## Best Practices

- **Container Context**: Always define `container-type` on parent elements
- **Named Containers**: Use named containers for complex layouts with multiple query contexts
- **Progressive Enhancement**: Design mobile-first, then enhance for larger containers
- **Performance**: Container queries are more performant than JavaScript-based solutions
- **Fallbacks**: Provide fallbacks for browsers that don't support container queries
- **Testing**: Test components in various container sizes, not just viewport sizes

## Common Pitfalls

### Issue 1: Missing Container Type
Container queries won't work without proper container context.

**Solution:**
Always define container-type:
```css
/* ❌ Wrong - no container context */
@container (min-width: 300px) {
  .card { /* Won't work */ }
}

/* ✅ Correct - with container context */
.card-container {
  container-type: inline-size;
}

@container (min-width: 300px) {
  .card { /* Works */ }
}
```

### Issue 2: Circular Dependencies
Querying the same element that defines the container.

**Solution:**
Query child elements, not the container itself:
```css
/* ❌ Wrong - circular dependency */
.container {
  container-type: inline-size;
}

@container (min-width: 300px) {
  .container { /* Circular! */ }
}

/* ✅ Correct - query children */
.container {
  container-type: inline-size;
}

@container (min-width: 300px) {
  .container .child { /* Correct */ }
}
```

### Issue 3: Browser Support
Container queries are relatively new and may not be supported in older browsers.

**Solution:**
Use feature detection and fallbacks:
```css
/* Fallback styles */
.card {
  display: block;
}

/* Enhanced styles with container queries */
@supports (container-type: inline-size) {
  .card-container {
    container-type: inline-size;
  }
  
  @container (min-width: 300px) {
    .card {
      display: flex;
    }
  }
}
```

## References

{% embed url="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Container_Queries" %}

{% embed url="https://caniuse.com/css-container-queries" %}

{% embed url="https://web.dev/cq-stable/" %}
