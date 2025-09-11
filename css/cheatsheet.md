---
description: >-
  A comprehensive grimoire of styling enchantments covering essential mystical
  properties, element targeting spells, and modern CSS sorcery for quick
  magical reference during your web conjuration rituals.

theme: "magic"
---

# The Styling Enchantment Grimoire

## The Ancient Knowledge

This mystical grimoire provides swift access to essential styling spells, element targeting incantations, and modern CSS sorcery. It serves as your trusted companion for quickly locating the precise magical syntax needed for common web styling rituals.

## CSS Properties Reference Enchantments

### Typography Sorcery

```css
/* Font Family Spells */
font-family: 'Arial', sans-serif;
font-family: 'Georgia', serif;
font-family: 'Courier New', monospace;
font-family: system-ui, -apple-system, sans-serif; /* System fonts */

/* Font Size Enchantments */
font-size: 16px;        /* Pixel precision */
font-size: 1rem;        /* Root relative */
font-size: 1.2em;       /* Parent relative */
font-size: clamp(1rem, 2.5vw, 2rem); /* Responsive scaling */

/* Font Weight Mystical Powers */
font-weight: normal;    /* 400 */
font-weight: bold;      /* 700 */
font-weight: 100;       /* Thin */
font-weight: 900;       /* Black */

/* Text Styling Spells */
font-style: italic;
font-variant: small-caps;
text-decoration: underline;
text-decoration: line-through;
text-decoration: none;

/* Text Alignment Rituals */
text-align: left;
text-align: center;
text-align: right;
text-align: justify;

/* Line Height Enchantments */
line-height: 1.5;       /* Unitless (recommended) */
line-height: 24px;      /* Fixed pixel height */
line-height: 150%;      /* Percentage */

/* Letter & Word Spacing Magic */
letter-spacing: 0.1em;
word-spacing: 0.2em;
text-indent: 2em;

/* Text Transform Spells */
text-transform: uppercase;
text-transform: lowercase;
text-transform: capitalize;
```

### Color & Background Enchantments

```css
/* Color Manifestation Spells */
color: #ff6b6b;         /* Hex notation */
color: rgb(255, 107, 107);
color: rgba(255, 107, 107, 0.8);
color: hsl(0, 100%, 71%);
color: hsla(0, 100%, 71%, 0.8);
color: currentColor;    /* Inherit text color */

/* Background Color Magic */
background-color: #f8f9fa;
background-color: transparent;

/* Background Image Sorcery */
background-image: url('image.jpg');
background-image: linear-gradient(45deg, #ff6b6b, #4ecdc4);
background-image: radial-gradient(circle, #ff6b6b, #4ecdc4);

/* Background Position & Size Spells */
background-position: center;
background-position: top left;
background-position: 50% 25%;
background-size: cover;     /* Scale to cover */
background-size: contain;   /* Scale to fit */
background-size: 100% 50%;  /* Custom dimensions */

/* Background Repeat Enchantments */
background-repeat: no-repeat;
background-repeat: repeat-x;
background-repeat: repeat-y;

/* Background Attachment Magic */
background-attachment: fixed;   /* Parallax effect */
background-attachment: scroll;
```

### Spacing & Sizing Mystical Arts

```css
/* Margin Enchantments */
margin: 20px;           /* All sides */
margin: 10px 20px;      /* Vertical | Horizontal */
margin: 10px 15px 20px 25px; /* Top | Right | Bottom | Left */
margin-top: 10px;
margin-right: 15px;
margin-bottom: 20px;
margin-left: 25px;
margin: 0 auto;         /* Center horizontally */

/* Padding Spells */
padding: 20px;          /* All sides */
padding: 10px 20px;     /* Vertical | Horizontal */
padding: 10px 15px 20px 25px; /* Top | Right | Bottom | Left */
padding-top: 10px;
padding-right: 15px;
padding-bottom: 20px;
padding-left: 25px;

/* Width & Height Sorcery */
width: 300px;
width: 50%;
width: 100vw;           /* Viewport width */
width: min-content;     /* Shrink to content */
width: max-content;     /* Expand to content */
width: fit-content;     /* Fit content with constraints */

height: 200px;
height: 50vh;           /* Viewport height */
height: 100%;

/* Min/Max Dimensions */
min-width: 200px;
max-width: 800px;
min-height: 100px;
max-height: 500px;

/* Box Sizing Enchantment */
box-sizing: border-box; /* Include padding/border in size */
box-sizing: content-box; /* Default behavior */
```

### Border & Outline Mystical Barriers

```css
/* Border Enchantments */
border: 2px solid #333;
border-width: 1px;
border-style: solid;    /* solid, dashed, dotted, double */
border-color: #ff6b6b;

/* Individual Border Sides */
border-top: 1px solid #ccc;
border-right: 2px dashed #ff6b6b;
border-bottom: 3px dotted #4ecdc4;
border-left: 4px double #333;

/* Border Radius Magic */
border-radius: 8px;     /* All corners */
border-radius: 50%;     /* Perfect circle */
border-radius: 10px 20px 30px 40px; /* Top-left | Top-right | Bottom-right | Bottom-left */
border-top-left-radius: 10px;
border-top-right-radius: 20px;

/* Outline Spells (doesn't affect layout) */
outline: 2px solid #ff6b6b;
outline-offset: 4px;    /* Distance from element */
outline: none;          /* Remove default focus outline */
```

### Positioning & Layout Sorcery

```css
/* Position Enchantments */
position: static;       /* Default flow */
position: relative;     /* Relative to normal position */
position: absolute;     /* Relative to positioned parent */
position: fixed;        /* Relative to viewport */
position: sticky;       /* Sticky positioning */

/* Position Coordinates */
top: 10px;
right: 20px;
bottom: 30px;
left: 40px;

/* Z-Index Layering Magic */
z-index: 1;
z-index: 999;
z-index: -1;

/* Display Manifestation Spells */
display: block;
display: inline;
display: inline-block;
display: none;          /* Hide element */
display: flex;          /* Flexbox container */
display: grid;          /* Grid container */
display: table;
display: table-cell;

/* Visibility Enchantments */
visibility: visible;
visibility: hidden;     /* Hidden but takes space */
opacity: 0.5;          /* Semi-transparent */
opacity: 0;            /* Fully transparent */
```

## CSS Selectors Mastery Spells

### Basic Selector Enchantments

```css
/* Element Selectors */
h1 { }                  /* All h1 elements */
p { }                   /* All paragraphs */
div { }                 /* All divs */

/* Class Selectors */
.mystical-class { }     /* Elements with class="mystical-class" */
.btn.primary { }        /* Elements with both classes */

/* ID Selectors */
#unique-element { }     /* Element with id="unique-element" */

/* Universal Selector */
* { }                   /* All elements */

/* Attribute Selectors */
[data-magic] { }        /* Elements with data-magic attribute */
[type="text"] { }       /* Input elements with type="text" */
[class*="btn"] { }      /* Elements with "btn" in class name */
[href^="https"] { }     /* Links starting with "https" */
[src$=".jpg"] { }       /* Images ending with ".jpg" */
```

### Pseudo-Class Mystical States

```css
/* Interactive States */
:hover { }              /* Mouse hover */
:focus { }              /* Keyboard/mouse focus */
:active { }             /* Being clicked */
:visited { }            /* Visited links */

/* Structural Pseudo-Classes */
:first-child { }        /* First child element */
:last-child { }         /* Last child element */
:nth-child(2n) { }      /* Every even child */
:nth-child(odd) { }     /* Every odd child */
:nth-child(3n+1) { }    /* Every 3rd child starting from 1st */

/* Form States */
:checked { }            /* Checked checkboxes/radios */
:disabled { }           /* Disabled form elements */
:enabled { }            /* Enabled form elements */
:required { }           /* Required form fields */
:valid { }              /* Valid form inputs */
:invalid { }            /* Invalid form inputs */

/* Content States */
:empty { }              /* Elements with no content */
:not(.excluded) { }     /* Elements without .excluded class */
```

### Pseudo-Element Creation Magic

```css
/* Content Generation */
::before {
  content: "★";
  position: absolute;
}

::after {
  content: "";
  display: block;
  clear: both;
}

/* Text Styling */
::first-letter { }      /* First letter of text */
::first-line { }        /* First line of text */
::selection { }         /* Selected text */

/* Form Elements */
::placeholder { }       /* Input placeholder text */
```

### Combinator Relationship Spells

```css
/* Descendant Combinator */
.parent .child { }      /* .child inside .parent */

/* Child Combinator */
.parent > .child { }    /* Direct .child of .parent */

/* Adjacent Sibling */
h1 + p { }              /* p immediately after h1 */

/* General Sibling */
h1 ~ p { }              /* All p siblings after h1 */
```

## Flexbox Layout Sorcery

### Flex Container Enchantments

```css
/* Activate Flexbox Magic */
.flex-container {
  display: flex;        /* Create flex container */
  display: inline-flex; /* Inline flex container */
}

/* Flex Direction Spells */
flex-direction: row;          /* Default: left to right */
flex-direction: row-reverse;  /* Right to left */
flex-direction: column;       /* Top to bottom */
flex-direction: column-reverse; /* Bottom to top */

/* Flex Wrap Enchantments */
flex-wrap: nowrap;      /* Default: single line */
flex-wrap: wrap;        /* Allow wrapping */
flex-wrap: wrap-reverse; /* Wrap in reverse */

/* Flex Flow Shorthand */
flex-flow: row wrap;    /* direction + wrap */

/* Main Axis Alignment (justify-content) */
justify-content: flex-start;    /* Default: start of main axis */
justify-content: flex-end;      /* End of main axis */
justify-content: center;        /* Center of main axis */
justify-content: space-between; /* Equal space between items */
justify-content: space-around;  /* Equal space around items */
justify-content: space-evenly;  /* Equal space everywhere */

/* Cross Axis Alignment (align-items) */
align-items: stretch;     /* Default: stretch to fill */
align-items: flex-start;  /* Start of cross axis */
align-items: flex-end;    /* End of cross axis */
align-items: center;      /* Center of cross axis */
align-items: baseline;    /* Align baselines */

/* Multi-line Cross Axis (align-content) */
align-content: stretch;        /* Default */
align-content: flex-start;
align-content: flex-end;
align-content: center;
align-content: space-between;
align-content: space-around;

/* Gap Between Items */
gap: 20px;              /* Gap between all items */
row-gap: 10px;          /* Gap between rows */
column-gap: 15px;       /* Gap between columns */
```

### Flex Item Mystical Properties

```css
/* Flex Growth & Shrinkage */
flex-grow: 0;           /* Default: don't grow */
flex-grow: 1;           /* Grow to fill space */
flex-grow: 2;           /* Grow twice as much */

flex-shrink: 1;         /* Default: can shrink */
flex-shrink: 0;         /* Don't shrink */

flex-basis: auto;       /* Default: based on content */
flex-basis: 200px;      /* Initial size before growing/shrinking */
flex-basis: 0;          /* Start from zero */

/* Flex Shorthand */
flex: 1;                /* flex: 1 1 0% */
flex: 0 1 auto;         /* Default values */
flex: none;             /* flex: 0 0 auto */

/* Individual Item Alignment */
align-self: auto;       /* Inherit from parent */
align-self: flex-start;
align-self: flex-end;
align-self: center;
align-self: stretch;
align-self: baseline;

/* Item Order */
order: 0;               /* Default order */
order: 1;               /* Move later */
order: -1;              /* Move earlier */
```

### Practical Flexbox Patterns

```css
/* Center Everything */
.center-all {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

/* Navigation Bar */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
}

/* Card Layout */
.card-container {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
}

.card {
  flex: 1 1 300px;      /* Grow, shrink, min 300px */
}

/* Sidebar Layout */
.layout {
  display: flex;
  min-height: 100vh;
}

.sidebar {
  flex: 0 0 250px;      /* Fixed width sidebar */
}

.main-content {
  flex: 1;              /* Take remaining space */
}
```

## CSS Grid Layout Magic

### Grid Container Enchantments

```css
/* Activate Grid Magic */
.grid-container {
  display: grid;        /* Create grid container */
  display: inline-grid; /* Inline grid container */
}

/* Define Grid Tracks */
grid-template-columns: 200px 1fr 100px;    /* Fixed, flexible, fixed */
grid-template-columns: repeat(3, 1fr);     /* 3 equal columns */
grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); /* Responsive */
grid-template-columns: 1fr 2fr 1fr;        /* Proportional sizing */

grid-template-rows: 100px auto 50px;       /* Row heights */
grid-template-rows: repeat(3, 100px);      /* 3 rows of 100px */

/* Grid Gaps */
gap: 20px;              /* Gap between all tracks */
row-gap: 10px;          /* Gap between rows */
column-gap: 15px;       /* Gap between columns */
grid-gap: 20px;         /* Legacy syntax */

/* Grid Areas Template */
grid-template-areas:
  "header header header"
  "sidebar main main"
  "footer footer footer";

/* Shorthand */
grid-template:
  "header header" 100px
  "sidebar main" 1fr
  "footer footer" 50px
  / 200px 1fr;          /* rows / columns */
```

### Grid Item Placement Spells

```css
/* Line-based Placement */
grid-column-start: 1;
grid-column-end: 3;
grid-row-start: 2;
grid-row-end: 4;

/* Shorthand Placement */
grid-column: 1 / 3;     /* Start line / End line */
grid-column: 1 / span 2; /* Start / Span count */
grid-row: 2 / 4;

/* Area Placement */
grid-area: header;      /* Use named area */
grid-area: 1 / 1 / 2 / 4; /* row-start / col-start / row-end / col-end */

/* Auto Placement */
grid-column: auto;
grid-row: auto;
```

### Named Lines & Areas

```css
/* Named Grid Lines */
.grid-with-names {
  display: grid;
  grid-template-columns:
    [sidebar-start] 250px
    [sidebar-end main-start] 1fr
    [main-end];
  grid-template-rows:
    [header-start] 100px
    [header-end content-start] 1fr
    [content-end footer-start] 50px
    [footer-end];
}

/* Using Named Lines */
.header {
  grid-column: sidebar-start / main-end;
  grid-row: header-start / header-end;
}

/* Grid Areas */
.layout-grid {
  display: grid;
  grid-template-areas:
    "header header header"
    "nav main aside"
    "footer footer footer";
  grid-template-columns: 200px 1fr 150px;
  grid-template-rows: auto 1fr auto;
}

.header { grid-area: header; }
.nav { grid-area: nav; }
.main { grid-area: main; }
.aside { grid-area: aside; }
.footer { grid-area: footer; }
```

### Grid Alignment Enchantments

```css
/* Justify Items (inline/horizontal axis) */
justify-items: stretch;    /* Default */
justify-items: start;
justify-items: end;
justify-items: center;

/* Align Items (block/vertical axis) */
align-items: stretch;      /* Default */
align-items: start;
align-items: end;
align-items: center;

/* Justify Content (entire grid inline axis) */
justify-content: start;
justify-content: end;
justify-content: center;
justify-content: stretch;
justify-content: space-around;
justify-content: space-between;
justify-content: space-evenly;

/* Align Content (entire grid block axis) */
align-content: start;
align-content: end;
align-content: center;
align-content: stretch;
align-content: space-around;
align-content: space-between;
align-content: space-evenly;

/* Individual Item Alignment */
justify-self: start;       /* Override justify-items */
align-self: center;        /* Override align-items */
```

### Responsive Grid Patterns

```css
/* Auto-fit Responsive Grid */
.responsive-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
}

/* Auto-fill vs Auto-fit */
grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); /* Creates empty columns */
grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));  /* Stretches existing columns */

/* Dense Packing */
grid-auto-flow: dense;     /* Fill gaps with smaller items */

/* Implicit Grid Sizing */
grid-auto-columns: 100px;  /* Size of auto-generated columns */
grid-auto-rows: 50px;      /* Size of auto-generated rows */
```

## Responsive Design Enchantments

### Media Query Spells

```css
/* Breakpoint Magic */
@media (max-width: 768px) {
  /* Mobile styles */
  .container {
    padding: 1rem;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  /* Tablet styles */
  .container {
    padding: 2rem;
  }
}

@media (min-width: 1025px) {
  /* Desktop styles */
  .container {
    padding: 3rem;
  }
}

/* Feature Queries */
@media (hover: hover) {
  /* Devices that can hover */
  .button:hover {
    background-color: #007bff;
  }
}

@media (prefers-reduced-motion: reduce) {
  /* Respect user's motion preferences */
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

@media (prefers-color-scheme: dark) {
  /* Dark mode styles */
  body {
    background-color: #1a1a1a;
    color: #ffffff;
  }
}
```

### Responsive Units Sorcery

```css
/* Viewport Units */
width: 100vw;           /* 100% of viewport width */
height: 100vh;          /* 100% of viewport height */
font-size: 4vw;         /* 4% of viewport width */
padding: 2vh 3vw;       /* Viewport-based spacing */

/* Container Query Units (when supported) */
width: 50cqw;           /* 50% of container width */
height: 25cqh;          /* 25% of container height */

/* Relative Units */
font-size: 1rem;        /* Relative to root font size */
margin: 2em;            /* Relative to element's font size */
width: 50%;             /* Relative to parent */

/* Responsive Typography */
font-size: clamp(1rem, 2.5vw, 2rem); /* Min, preferred, max */
line-height: clamp(1.2, 1.5, 1.8);

/* Fluid Spacing */
margin: clamp(1rem, 5vw, 3rem);
padding: clamp(0.5rem, 2vw, 1.5rem);
```

### Mobile-First Approach

```css
/* Base styles (mobile first) */
.container {
  width: 100%;
  padding: 1rem;
}

.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 1rem;
}

/* Tablet and up */
@media (min-width: 768px) {
  .container {
    max-width: 750px;
    margin: 0 auto;
    padding: 2rem;
  }

  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* Desktop and up */
@media (min-width: 1024px) {
  .container {
    max-width: 1200px;
    padding: 3rem;
  }

  .grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

## CSS Animations & Transitions Spells

### Transition Enchantments

```css
/* Basic Transitions */
transition: all 0.3s ease;
transition: opacity 0.5s ease-in-out;
transition: transform 0.2s cubic-bezier(0.4, 0, 0.2, 1);

/* Multiple Properties */
transition:
  opacity 0.3s ease,
  transform 0.3s ease,
  background-color 0.2s ease;

/* Transition Properties */
transition-property: opacity, transform;
transition-duration: 0.3s, 0.5s;
transition-timing-function: ease, ease-in-out;
transition-delay: 0s, 0.1s;

/* Timing Functions */
transition-timing-function: ease;        /* Default */
transition-timing-function: linear;
transition-timing-function: ease-in;
transition-timing-function: ease-out;
transition-timing-function: ease-in-out;
transition-timing-function: cubic-bezier(0.25, 0.1, 0.25, 1);

/* Hover Effects */
.button {
  background-color: #007bff;
  transform: scale(1);
  transition: all 0.2s ease;
}

.button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}
```

### Keyframe Animation Magic

```css
/* Define Animation */
@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-30px);
  }
  60% {
    transform: translateY(-15px);
  }
}

/* Apply Animation */
.animated-element {
  animation: fadeIn 0.5s ease-out;
}

.bouncing-element {
  animation: bounce 2s infinite;
}

/* Animation Properties */
animation-name: fadeIn;
animation-duration: 0.5s;
animation-timing-function: ease-out;
animation-delay: 0.2s;
animation-iteration-count: infinite;
animation-direction: alternate;
animation-fill-mode: forwards;
animation-play-state: paused;

/* Animation Shorthand */
animation: fadeIn 0.5s ease-out 0.2s infinite alternate forwards;
```

### Transform Mystical Powers

```css
/* 2D Transforms */
transform: translate(50px, 100px);    /* Move element */
transform: translateX(50px);          /* Move horizontally */
transform: translateY(100px);         /* Move vertically */
transform: scale(1.5);                /* Scale uniformly */
transform: scaleX(2);                 /* Scale horizontally */
transform: scaleY(0.5);               /* Scale vertically */
transform: rotate(45deg);             /* Rotate */
transform: skew(30deg, 20deg);        /* Skew */

/* 3D Transforms */
transform: translateZ(50px);          /* Move in Z-axis */
transform: rotateX(45deg);            /* Rotate around X-axis */
transform: rotateY(45deg);            /* Rotate around Y-axis */
transform: rotateZ(45deg);            /* Rotate around Z-axis */

/* Multiple Transforms */
transform: translate(50px, 100px) rotate(45deg) scale(1.2);

/* Transform Origin */
transform-origin: center;             /* Default */
transform-origin: top left;
transform-origin: 50% 50%;
transform-origin: 10px 20px;
```

## Modern CSS Features Sorcery

### CSS Custom Properties (Variables)

```css
/* Define Custom Properties */
:root {
  --primary-color: #007bff;
  --secondary-color: #6c757d;
  --font-size-base: 1rem;
  --spacing-unit: 1rem;
  --border-radius: 0.375rem;
  --transition-speed: 0.3s;
}

/* Use Custom Properties */
.button {
  background-color: var(--primary-color);
  font-size: var(--font-size-base);
  padding: var(--spacing-unit);
  border-radius: var(--border-radius);
  transition: all var(--transition-speed) ease;
}

/* Fallback Values */
color: var(--text-color, #333);       /* Use #333 if --text-color not defined */

/* Dynamic Custom Properties */
.theme-dark {
  --primary-color: #0d6efd;
  --background-color: #1a1a1a;
  --text-color: #ffffff;
}

/* Computed Values */
.element {
  --base-size: 1rem;
  font-size: calc(var(--base-size) * 1.5);
  margin: calc(var(--spacing-unit) * 2);
}
```

### Container Queries Magic

```css
/* Container Query Setup */
.card-container {
  container-type: inline-size;
  container-name: card;
}

/* Container Query */
@container card (min-width: 300px) {
  .card {
    display: flex;
    flex-direction: row;
  }

  .card-image {
    width: 150px;
  }
}

@container (max-width: 250px) {
  .card-title {
    font-size: 1rem;
  }
}
```

### Logical Properties Enchantments

```css
/* Logical vs Physical Properties */
margin-inline-start: 1rem;    /* Instead of margin-left */
margin-inline-end: 1rem;      /* Instead of margin-right */
margin-block-start: 1rem;     /* Instead of margin-top */
margin-block-end: 1rem;       /* Instead of margin-bottom */

padding-inline: 2rem;         /* Horizontal padding */
padding-block: 1rem;          /* Vertical padding */

border-inline-start: 1px solid #ccc;  /* Start border */
border-inline-end: 1px solid #ccc;    /* End border */

/* Text Direction Aware */
text-align: start;            /* Instead of left */
text-align: end;              /* Instead of right */

/* Size Properties */
inline-size: 300px;           /* Instead of width */
block-size: 200px;            /* Instead of height */
```

### Advanced Selectors & Functions

```css
/* :is() Selector */
:is(h1, h2, h3, h4, h5, h6) {
  margin-block: 1em 0.5em;
}

/* :where() Selector (0 specificity) */
:where(ul, ol) li {
  margin-bottom: 0.5em;
}

/* :has() Selector (when supported) */
.card:has(img) {
  display: grid;
  grid-template-columns: auto 1fr;
}

/* Math Functions */
width: min(100%, 800px);      /* Minimum of values */
height: max(200px, 50vh);     /* Maximum of values */
font-size: clamp(1rem, 4vw, 2rem); /* Constrained scaling */

/* Color Functions */
color: hsl(200 50% 50%);      /* Modern HSL syntax */
background: oklch(0.7 0.15 180); /* OKLCH color space */
border-color: color-mix(in srgb, blue 30%, red); /* Color mixing */
```

### Performance & Accessibility

```css
/* Performance Optimizations */
will-change: transform;       /* Hint for browser optimization */
contain: layout style paint; /* Containment for performance */
content-visibility: auto;     /* Lazy rendering */

/* Accessibility Enhancements */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

@media (prefers-contrast: high) {
  .button {
    border: 2px solid currentColor;
  }
}

/* Focus Management */
:focus-visible {
  outline: 2px solid var(--focus-color);
  outline-offset: 2px;
}

/* Screen Reader Only */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

## Common Curses & Their Remedies

### Curse 1: Box Model Confusion
Elements not sizing as expected due to padding and borders.

**Counter-Spell:**
```css
* {
  box-sizing: border-box;
}
```

### Curse 2: Flexbox Item Overflow
Flex items overflowing their container.

**Banishment Ritual:**
```css
.flex-item {
  min-width: 0; /* Allow shrinking below content size */
  overflow: hidden; /* Prevent overflow */
}
```

### Curse 3: Grid Item Placement Issues
Grid items not appearing where expected.

**Protective Ward:**
```css
.grid-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  /* Use explicit sizing instead of auto */
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://developer.mozilla.org/en-US/docs/Web/CSS" %}

{% embed url="https://css-tricks.com/" %}

{% embed url="https://web.dev/learn/css/" %}

{% embed url="https://caniuse.com/" %}
