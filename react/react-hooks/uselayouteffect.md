---
description: >-
  Complete grimoire for the useLayoutEffect enchantment - master the ancient art
  of synchronous effect sorcery in React enchantments, performing DOM mutations
  and measurements before browser repainting for flicker-free magic.

theme: "magic"
---

# useLayoutEffect - The Synchronous Effect Ritual

## The Ancient Knowledge

The `useLayoutEffect` enchantment is similar to `useEffect`, but it fires synchronously after all DOM mutations and before the browser repaints the screen. This mystical hook is perfect for reading layout from the DOM and synchronously re-rendering, ensuring that users never see intermediate states that could cause visual flickering.

## When to Cast This Spell

1. **DOM Measurements**: Measure DOM elements and update state based on their dimensions
2. **Scroll Position Management**: Set scroll positions before the browser paints
3. **Animation Setup**: Initialize animations that need to start immediately
4. **Tooltip Positioning**: Calculate and set tooltip positions based on target elements
5. **Preventing Visual Flicker**: Avoid flash of unstyled content or layout shifts

## Your First Casting

Here's an example of measuring DOM elements with `useLayoutEffect`:

```jsx
import React, { useState, useLayoutEffect, useRef } from 'react';

function TooltipEnchantment({ children, tooltip }) {
  const [tooltipStyle, setTooltipStyle] = useState({});
  const targetRef = useRef(null);
  const tooltipRef = useRef(null);

  useLayoutEffect(() => {
    if (targetRef.current && tooltipRef.current) {
      const targetRect = targetRef.current.getBoundingClientRect();
      const tooltipRect = tooltipRef.current.getBoundingClientRect();

      // Calculate position to center tooltip above target
      const left = targetRect.left + (targetRect.width - tooltipRect.width) / 2;
      const top = targetRect.top - tooltipRect.height - 8;

      setTooltipStyle({
        position: 'fixed',
        left: `${left}px`,
        top: `${top}px`,
        visibility: 'visible'
      });
    }
  });

  return (
    <div>
      <div ref={targetRef}>
        {children}
      </div>
      <div
        ref={tooltipRef}
        style={{
          ...tooltipStyle,
          visibility: tooltipStyle.visibility || 'hidden',
          background: 'black',
          color: 'white',
          padding: '4px 8px',
          borderRadius: '4px',
          fontSize: '12px'
        }}
      >
        {tooltip}
      </div>
    </div>
  );
}
```

## Advanced Sorcery

### Preventing Layout Shift

```jsx
function DynamicContentEnchantment({ showContent }) {
  const [height, setHeight] = useState(0);
  const contentRef = useRef(null);

  useLayoutEffect(() => {
    if (showContent && contentRef.current) {
      // Measure content height before it's visible
      const contentHeight = contentRef.current.scrollHeight;
      setHeight(contentHeight);
    } else {
      setHeight(0);
    }
  }, [showContent]);

  return (
    <div
      style={{
        height: `${height}px`,
        overflow: 'hidden',
        transition: 'height 0.3s ease'
      }}
    >
      <div ref={contentRef}>
        {showContent && (
          <div>
            <h3>Mystical Content</h3>
            <p>This content appears without layout shift!</p>
            <p>The container height is pre-calculated.</p>
          </div>
        )}
      </div>
    </div>
  );
}
```

### Scroll Position Management

```jsx
function ScrollManagerEnchantment({ items, selectedId }) {
  const listRef = useRef(null);
  const selectedRef = useRef(null);

  useLayoutEffect(() => {
    if (selectedRef.current && listRef.current) {
      const list = listRef.current;
      const selected = selectedRef.current;

      const listRect = list.getBoundingClientRect();
      const selectedRect = selected.getBoundingClientRect();

      // Scroll to keep selected item in view
      if (selectedRect.bottom > listRect.bottom) {
        list.scrollTop += selectedRect.bottom - listRect.bottom;
      } else if (selectedRect.top < listRect.top) {
        list.scrollTop -= listRect.top - selectedRect.top;
      }
    }
  }, [selectedId]);

  return (
    <div
      ref={listRef}
      style={{ height: '200px', overflow: 'auto' }}
    >
      {items.map(item => (
        <div
          key={item.id}
          ref={item.id === selectedId ? selectedRef : null}
          style={{
            padding: '8px',
            background: item.id === selectedId ? 'lightblue' : 'white'
          }}
        >
          {item.name}
        </div>
      ))}
    </div>
  );
}
```

## Wisdom of the Layout Ancients

- **Synchronous Execution**: useLayoutEffect runs synchronously before browser paint
- **Performance Impact**: Can block visual updates if overused or with heavy computations
- **DOM Mutations**: Perfect for DOM measurements and immediate updates
- **Server Rendering**: Identical to useEffect during server-side rendering

## Common Curses & Their Remedies

### Curse 1: Overusing useLayoutEffect
Using useLayoutEffect when useEffect would suffice can hurt performance.

**Counter-Spell:**
Only use useLayoutEffect when you need synchronous DOM access:

```jsx
// Cursed approach - unnecessary synchronous effect
useLayoutEffect(() => {
  console.log('This could be useEffect');
}, []);

// Pure spell approach
useEffect(() => {
  console.log('This is fine as useEffect');
}, []);
```

### Curse 2: Heavy Computations in Layout Effect
Performing expensive operations blocks the browser paint.

**Banishment Ritual:**
Keep layout effects lightweight:

```jsx
// Cursed approach - heavy computation
useLayoutEffect(() => {
  const result = performExpensiveCalculation(); // Blocks painting!
  setResult(result);
}, [data]);

// Pure spell approach
useLayoutEffect(() => {
  // Only do DOM measurements here
  const rect = elementRef.current.getBoundingClientRect();
  setPosition({ x: rect.left, y: rect.top });
}, []);

useEffect(() => {
  // Heavy computations in regular effect
  const result = performExpensiveCalculation();
  setResult(result);
}, [data]);
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useLayoutEffect" %}

