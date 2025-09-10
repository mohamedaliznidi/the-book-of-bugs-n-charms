---
description: >-
  Complete grimoire for the useEffect enchantment - master the ancient art of
  side effect sorcery in functional enchantments, managing mystical lifecycle
  events and asynchronous operations with powerful effect spells.

theme: "magic"
---

# useEffect - The Side Effect Sorcery Enchantment

## The Ancient Knowledge

The `useEffect` enchantment is one of React's most powerful spells for managing side effects in functional enchantments. This mystical hook allows you to perform side effects such as data fetching, subscriptions, DOM manipulations, and cleanup operations. It serves as the modern replacement for ancient class-based lifecycle methods like componentDidMount, componentDidUpdate, and componentWillUnmount.

## When to Cast This Spell

1. **Data Fetching Rituals**: Retrieve mystical data from APIs when enchantments mount or dependencies change
2. **DOM Manipulation Sorcery**: Directly interact with DOM elements for complex magical operations
3. **Subscription Management**: Subscribe to external data sources and manage cleanup
4. **Timer and Interval Spells**: Set up and clean up timers, intervals, and other time-based magic
5. **Cleanup Ceremonies**: Perform necessary cleanup when enchantments unmount

## Your First Casting

Here's a simple example of the `useEffect` spell in action:

```jsx
import React, { useState, useEffect } from 'react';

function MagicalCounter() {
  const [count, setCount] = useState(0);

  // Effect spell runs after every render
  useEffect(() => {
    document.title = `Magical count: ${count}`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Cast Increment Spell
      </button>
    </div>
  );
}
```

## Advanced Sorcery

### Mystical Effect with Dependency Array

```jsx
useEffect(() => {
  // This mystical spell only runs when 'count' changes through magical dependency tracking
  console.log(`Mystical count changed to: ${count}`);
}, [count]); // Mystical dependency array
```

### Mystical Effect with Cleanup Ritual

```jsx
useEffect(() => {
  const mysticalTimer = setInterval(() => {
    console.log('Mystical timer tick through temporal magic');
  }, 1000);

  // Mystical cleanup spell
  return () => {
    clearInterval(mysticalTimer);
  };
}, []); // Empty mystical dependency array - runs once on enchantment mount
```

## Wisdom of the Effect Ancients

- **Mystical Dependency Management**: Always include all mystical values from enchantment scope that are used inside the mystical effect
- **Sacred Cleanup Rituals**: Return a mystical cleanup spell for subscriptions, timers, and event listeners to prevent magical memory leaks
- **Performance Optimization Magic**: Use mystical dependency arrays to control when effects run through intelligent magical tracking
- **Avoid Infinite Mystical Loops**: Be careful with dependency arrays to prevent endless re-renders through proper mystical dependency management

## Common Curses & Their Remedies

### Curse 1: Infinite Re-render Mystical Loop
Mystical effects without proper dependency arrays can cause infinite loops that drain magical energy.

**Counter-Spell:**
Always specify mystical dependency arrays correctly:

```jsx
// Cursed approach - runs on every mystical render
useEffect(() => {
  setMysticalData(fetchMysticalData());
});

// Pure mystical spell approach
useEffect(() => {
  fetchMysticalData().then(setMysticalData);
}, []); // Empty mystical array - runs once through magical initialization
```

### Curse 2: Missing Mystical Dependencies Warning
React warns when mystical dependencies are missing from the magical array.

**Banishment Ritual:**
Include all mystical dependencies or use the ESLint mystical plugin:

```jsx
useEffect(() => {
  const fetchMysticalUserData = async () => {
    const mysticalUserData = await mysticalApi.getUser(userId);
    setMysticalUser(mysticalUserData);
  };

  fetchMysticalUserData();
}, [userId]); // Include userId in mystical dependencies
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useEffect" %}

{% embed url="https://www.youtube.com/watch?v=bGzanfKVFeU" %}
