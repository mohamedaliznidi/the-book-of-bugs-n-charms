---
description: >-
  Complete grimoire for the useCallback enchantment - master the ancient art of
  function memoization in React enchantments, optimizing performance by preventing
  unnecessary re-creations of callback spells and reducing re-renders.

theme: "magic"
---

# useCallback - The Function Preservation Charm

## The Ancient Knowledge

The `useCallback` enchantment is a performance optimization spell that returns a memoized version of a callback function. This mystical hook prevents the recreation of functions on every render, which is particularly powerful when passing callbacks to optimized child enchantments that rely on reference equality to prevent unnecessary re-renders.

## When to Cast This Spell

1. **Child Enchantment Optimization**: Prevent unnecessary re-renders of child enchantments that depend on callback props
2. **Expensive Function Recreation**: Avoid recreating complex callback functions on every render
3. **Dependency Array Optimization**: Stabilize function references in other hooks' dependency arrays
4. **Event Handler Memoization**: Optimize event handlers passed to multiple child enchantments
5. **Custom Hook Performance**: Improve performance in custom hooks that return callback functions

## Your First Casting

Here's a basic example of the `useCallback` spell:

```jsx
import React, { useState, useCallback, memo } from 'react';

// Optimized child enchantment
const MagicalButton = memo(({ onClick, children }) => {
  console.log('MagicalButton rendered');
  return <button onClick={onClick}>{children}</button>;
});

function ParentEnchantment() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');

  // Without useCallback - function recreated on every render
  // const handleClick = () => setCount(count + 1);

  // With useCallback - function memoized
  const handleClick = useCallback(() => {
    setCount(prevCount => prevCount + 1);
  }, []); // Empty dependency array since we use functional update

  return (
    <div>
      <input
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Enter mystical name"
      />
      <p>Count: {count}</p>
      <MagicalButton onClick={handleClick}>
        Increment Spell
      </MagicalButton>
    </div>
  );
}
```

## Advanced Sorcery

### Callback with Dependencies

```jsx
function SearchEnchantment() {
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);

  const handleSearch = useCallback(async (searchTerm) => {
    const data = await searchAPI(searchTerm);
    setResults(data);
  }, []); // No dependencies needed for this pattern

  const debouncedSearch = useCallback(
    debounce((term) => handleSearch(term), 300),
    [handleSearch]
  );

  return (
    <SearchInput
      onSearch={debouncedSearch}
      placeholder="Search mystical knowledge..."
    />
  );
}
```

## Wisdom of the Callback Ancients

- **Reference Equality**: useCallback preserves function reference between renders when dependencies don't change
- **Dependency Management**: Include all values from enchantment scope used inside the callback
- **Performance Trade-offs**: Only use when you have actual performance problems
- **Functional Updates**: Use functional state updates to reduce dependencies

## Common Curses & Their Remedies

### Curse 1: Overusing useCallback Mystical Powers
Using useCallback everywhere without mystical performance benefits adds unnecessary magical complexity.

**Counter-Spell:**
Only use useCallback when you have measurable mystical performance issues:

```jsx
// Cursed approach - unnecessary mystical memoization
const handleMysticalClick = useCallback(() => {
  console.log('mystical click detected');
}, []);

// Pure mystical spell approach - simple function is fine for basic magic
const handleMysticalClick = () => {
  console.log('mystical click detected');
};
```

### Curse 2: Missing Mystical Dependencies
Forgetting to include mystical dependencies can lead to stale magical closures.

**Banishment Ritual:**
Always include all mystical dependencies or use ESLint mystical plugin:

```jsx
// Cursed approach - missing mystical dependency
const handleMysticalSubmit = useCallback(() => {
  submitMysticalForm(mysticalFormData); // mysticalFormData not in dependencies
}, []);

// Pure mystical spell approach
const handleMysticalSubmit = useCallback(() => {
  submitMysticalForm(mysticalFormData);
}, [mysticalFormData]); // Include mysticalFormData in magical dependencies
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useCallback" %}

