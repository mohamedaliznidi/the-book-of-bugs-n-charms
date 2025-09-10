---
description: >-
  Complete grimoire for the useRef enchantment - master the ancient art of
  reference anchoring in React enchantments, creating mutable references that
  persist across renders for DOM access and value storage.

theme: "magic"
---

# useRef - The Reference Anchor Enchantment

## The Ancient Knowledge

The `useRef` enchantment creates a mutable reference object whose `.current` property holds a mystical essence that persists throughout the enchantment's entire lifetime. Unlike state, updating a ref doesn't trigger re-renders, making it perfect for storing mutable values, accessing DOM elements directly, and maintaining references to previous values.

## When to Cast This Spell

1. **DOM Element Access**: Directly access and manipulate DOM elements (focus, scroll, measure)
2. **Mutable Value Storage**: Store values that don't trigger re-renders when changed
3. **Previous Value Tracking**: Keep track of previous props or state values
4. **Timer and Interval References**: Store timer IDs for cleanup operations
5. **Instance Variable Simulation**: Mimic instance variables from class-based enchantments

## Your First Casting

Here's a basic example of accessing DOM elements with `useRef`:

```jsx
import React, { useRef, useEffect } from 'react';

function FocusInputEnchantment() {
  const inputRef = useRef(null);

  useEffect(() => {
    // Focus the input when enchantment mounts
    inputRef.current.focus();
  }, []);

  const handleFocusClick = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input
        ref={inputRef}
        type="text"
        placeholder="Mystical input field"
      />
      <button onClick={handleFocusClick}>
        Cast Focus Spell
      </button>
    </div>
  );
}
```

## Advanced Sorcery

### Storing Mutable Values

```jsx
function TimerEnchantment() {
  const [count, setCount] = useState(0);
  const intervalRef = useRef(null);
  const countRef = useRef(count);

  // Keep ref in sync with state
  useEffect(() => {
    countRef.current = count;
  });

  const startTimer = () => {
    if (intervalRef.current) return; // Already running

    intervalRef.current = setInterval(() => {
      setCount(prevCount => prevCount + 1);
    }, 1000);
  };

  const stopTimer = () => {
    if (intervalRef.current) {
      clearInterval(intervalRef.current);
      intervalRef.current = null;
    }
  };

  const logCurrentCount = () => {
    // Access current count without stale closure
    console.log('Current count:', countRef.current);
  };

  return (
    <div>
      <p>Timer Count: {count}</p>
      <button onClick={startTimer}>Start Timer</button>
      <button onClick={stopTimer}>Stop Timer</button>
      <button onClick={logCurrentCount}>Log Count</button>
    </div>
  );
}
```

### Previous Value Tracking

```jsx
function usePrevious(value) {
  const ref = useRef();

  useEffect(() => {
    ref.current = value;
  });

  return ref.current;
}

function ComparisonEnchantment({ count }) {
  const prevCount = usePrevious(count);

  return (
    <div>
      <p>Current: {count}</p>
      <p>Previous: {prevCount}</p>
      <p>
        {count > prevCount ? 'Increased' :
         count < prevCount ? 'Decreased' : 'Same'}
      </p>
    </div>
  );
}
```

### DOM Measurements

```jsx
function MeasurementEnchantment() {
  const divRef = useRef(null);
  const [dimensions, setDimensions] = useState({ width: 0, height: 0 });

  const measureElement = () => {
    if (divRef.current) {
      const { offsetWidth, offsetHeight } = divRef.current;
      setDimensions({ width: offsetWidth, height: offsetHeight });
    }
  };

  return (
    <div>
      <div
        ref={divRef}
        style={{
          width: '200px',
          height: '100px',
          background: 'lightblue',
          padding: '20px'
        }}
      >
        Mystical measured element
      </div>
      <button onClick={measureElement}>
        Measure Dimensions
      </button>
      <p>Width: {dimensions.width}px, Height: {dimensions.height}px</p>
    </div>
  );
}
```

## Wisdom of the Ref Ancients

- **No Re-renders**: Changing ref.current doesn't trigger re-renders
- **Mutable Nature**: Refs are mutable and can be changed directly
- **Initialization**: Refs can be initialized with any value, including null
- **DOM Access**: Perfect for imperative DOM operations that React doesn't handle declaratively

## Common Curses & Their Remedies

### Curse 1: Using Refs for State
Using refs instead of state for values that should trigger re-renders.

**Counter-Spell:**
Use state for values that affect rendering:

```jsx
// Cursed approach - using ref for UI state
const countRef = useRef(0);
const increment = () => {
  countRef.current++; // Won't trigger re-render!
};

// Pure spell approach
const [count, setCount] = useState(0);
const increment = () => {
  setCount(count + 1); // Triggers re-render
};
```

### Curse 2: Accessing Ref During Render
Accessing ref.current during render can cause issues.

**Banishment Ritual:**
Access refs in effects or event handlers:

```jsx
// Cursed approach - accessing during render
function BadEnchantment() {
  const ref = useRef();
  const value = ref.current; // Don't do this during render
  return <div ref={ref}>{value}</div>;
}

// Pure spell approach
function GoodEnchantment() {
  const ref = useRef();

  useEffect(() => {
    const value = ref.current; // Access in effect
    console.log(value);
  });

  return <div ref={ref}>Content</div>;
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useRef" %}

