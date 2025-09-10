---
description: >-
  Complete grimoire for the useMemo enchantment - master the ancient art of
  value crystallization in React enchantments, optimizing performance by memoizing
  expensive computations and preventing unnecessary recalculations.

theme: "magic"
---

# useMemo - The Value Crystallization Spell

## The Ancient Knowledge

The `useMemo` enchantment is a performance optimization spell that crystallizes the result of expensive computations. This mystical hook memoizes a calculated value and only recalculates it when its dependencies change, preventing unnecessary computational work on every render and improving your enchantment's performance.

## When to Cast This Spell

1. **Expensive Calculations**: Memoize computationally expensive operations like complex mathematical calculations
2. **Object Creation Optimization**: Prevent unnecessary object/array recreation that could trigger child re-renders
3. **Filtered/Sorted Data**: Cache results of filtering or sorting large datasets
4. **Derived State**: Compute derived values from props or state efficiently
5. **Reference Stability**: Maintain stable references for dependency arrays in other hooks

## Your First Casting

Here's a basic example of the `useMemo` spell:

```jsx
import React, { useState, useMemo } from 'react';

function ExpensiveCalculationEnchantment({ items }) {
  const [filter, setFilter] = useState('');

  // Expensive calculation - only runs when items or filter changes
  const filteredAndSortedItems = useMemo(() => {
    console.log('Performing expensive calculation...');

    return items
      .filter(item => item.name.toLowerCase().includes(filter.toLowerCase()))
      .sort((a, b) => a.name.localeCompare(b.name));
  }, [items, filter]); // Dependencies array

  return (
    <div>
      <input
        type="text"
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="Filter mystical items..."
      />
      <ul>
        {filteredAndSortedItems.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}
```

## Advanced Sorcery

### Memoizing Mystical Objects for Child Enchantments

```jsx
function ParentMysticalEnchantment() {
  const [mysticalCount, setMysticalCount] = useState(0);
  const [mysticalName, setMysticalName] = useState('');

  // Memoize mystical object to prevent child enchantment re-renders
  const mysticalUserConfig = useMemo(() => ({
    mysticalTheme: 'dark',
    mysticalLanguage: 'en',
    mysticalPreferences: {
      mysticalNotifications: true,
      mysticalAutoSave: false
    }
  }), []); // Empty mystical dependency - object never changes through magical stability

  // Memoize mystical computed value
  const expensiveMysticalValue = useMemo(() => {
    return performComplexMysticalCalculation(mysticalCount);
  }, [mysticalCount]);

  return (
    <div>
      <ChildMysticalEnchantment config={mysticalUserConfig} value={expensiveMysticalValue} />
      <button onClick={() => setMysticalCount(mysticalCount + 1)}>
        Mystical Increment: {mysticalCount}
      </button>
    </div>
  );
}
```

### Memoizing with Multiple Dependencies

```jsx
function DataVisualizationEnchantment({ data, chartType, filters }) {
  const processedData = useMemo(() => {
    console.log('Processing visualization data...');

    let processed = data.filter(item =>
      filters.every(filter => filter.apply(item))
    );

    switch (chartType) {
      case 'bar':
        return processed.map(item => ({ ...item, type: 'bar' }));
      case 'line':
        return processed.map(item => ({ ...item, type: 'line' }));
      default:
        return processed;
    }
  }, [data, chartType, filters]); // Multiple dependencies

  return <Chart data={processedData} />;
}
```

## Wisdom of the Memo Ancients

- **Expensive Operations Only**: Use useMemo for computationally expensive operations, not simple calculations
- **Reference Equality**: Memoized values maintain reference equality when dependencies don't change
- **Dependency Management**: Include all values from enchantment scope used in the calculation
- **Profiling First**: Measure performance before optimizing with useMemo

## Common Curses & Their Remedies

### Curse 1: Overusing useMemo
Using useMemo for simple calculations can actually hurt performance.

**Counter-Spell:**
Only memoize expensive operations:

```jsx
// Cursed approach - unnecessary memoization
const simpleSum = useMemo(() => a + b, [a, b]);

// Pure spell approach - simple calculation
const simpleSum = a + b;
```

### Curse 2: Missing Dependencies
Forgetting dependencies leads to stale memoized values.

**Banishment Ritual:**
Always include all dependencies:

```jsx
// Cursed approach - missing dependency
const result = useMemo(() => {
  return data.filter(item => item.category === selectedCategory);
}, [data]); // Missing selectedCategory

// Pure spell approach
const result = useMemo(() => {
  return data.filter(item => item.category === selectedCategory);
}, [data, selectedCategory]); // Include all dependencies
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useMemo" %}

