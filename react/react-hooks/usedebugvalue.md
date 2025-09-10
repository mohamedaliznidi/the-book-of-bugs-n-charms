---
description: >-
  Complete grimoire for the useDebugValue enchantment - master the ancient art
  of debug divination in React enchantments, providing mystical insights for
  custom hooks in React DevTools with enhanced debugging capabilities.

theme: "magic"
---

# useDebugValue - The Debug Divination Charm

## The Ancient Knowledge

The `useDebugValue` enchantment provides mystical insight for custom hooks in React DevTools. This debugging spell allows you to display additional information about your custom hooks, making it easier to understand their internal state and behavior during development. It's particularly powerful when building complex custom hooks that other sorcerers will use.

## When to Cast This Spell

1. **Custom Hook Development**: Add debugging information to custom hooks for better developer experience
2. **Complex State Logic**: Display computed or derived values that aren't immediately obvious
3. **Library Development**: Provide helpful debugging info for hooks in shared libraries
4. **Team Collaboration**: Make custom hooks more transparent for other developers
5. **Performance Monitoring**: Display performance-related information during development

## Your First Casting

Here's a basic example of using `useDebugValue` in a custom hook:

```jsx
import React, { useState, useEffect, useDebugValue } from 'react';

function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(navigator.onLine);

  useEffect(() => {
    const handleOnline = () => setIsOnline(true);
    const handleOffline = () => setIsOnline(false);

    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);

    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);

  // This will show in React DevTools
  useDebugValue(isOnline ? 'Connected to mystical network' : 'Disconnected from realm');

  return isOnline;
}

function NetworkStatusEnchantment() {
  const isOnline = useOnlineStatus();

  return (
    <div>
      <h3>Network Status</h3>
      <p>Status: {isOnline ? '🟢 Online' : '🔴 Offline'}</p>
    </div>
  );
}
```

## Advanced Sorcery

### Debug Value with Formatter Function

```jsx
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error(`Error reading localStorage key "${key}":`, error);
      return initialValue;
    }
  });

  // Use formatter function for expensive debug computations
  useDebugValue(
    { key, storedValue },
    ({ key, storedValue }) => `${key}: ${JSON.stringify(storedValue).slice(0, 50)}...`
  );

  const setValue = (value) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error(`Error setting localStorage key "${key}":`, error);
    }
  };

  return [storedValue, setValue];
}
```

### Complex Custom Hook with Multiple Debug Values

```jsx
function useApiData(url, options = {}) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);
  const [lastFetch, setLastFetch] = useState(null);

  // Multiple debug values for different aspects
  useDebugValue(data, (data) =>
    data ? `Data loaded: ${Object.keys(data).length} keys` : 'No data'
  );

  useDebugValue(loading ? 'Fetching...' : 'Idle');

  useDebugValue(error, (error) =>
    error ? `Error: ${error.message}` : 'No errors'
  );

  useEffect(() => {
    let cancelled = false;

    const fetchData = async () => {
      setLoading(true);
      setError(null);

      try {
        const response = await fetch(url, options);
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        const result = await response.json();

        if (!cancelled) {
          setData(result);
          setLastFetch(new Date().toISOString());
        }
      } catch (err) {
        if (!cancelled) {
          setError(err);
        }
      } finally {
        if (!cancelled) {
          setLoading(false);
        }
      }
    };

    fetchData();

    return () => {
      cancelled = true;
    };
  }, [url, JSON.stringify(options)]);

  return { data, loading, error, lastFetch };
}
```

### Conditional Debug Values

```jsx
function useCounter(initialValue = 0, step = 1) {
  const [count, setCount] = useState(initialValue);
  const [history, setHistory] = useState([initialValue]);

  // Only show debug info in development
  if (process.env.NODE_ENV === 'development') {
    useDebugValue({
      current: count,
      step,
      changes: history.length - 1,
      lastChange: history[history.length - 1]
    }, (debugInfo) =>
      `Count: ${debugInfo.current} (${debugInfo.changes} changes, step: ${debugInfo.step})`
    );
  }

  const increment = () => {
    const newCount = count + step;
    setCount(newCount);
    setHistory(prev => [...prev, newCount]);
  };

  const decrement = () => {
    const newCount = count - step;
    setCount(newCount);
    setHistory(prev => [...prev, newCount]);
  };

  const reset = () => {
    setCount(initialValue);
    setHistory([initialValue]);
  };

  return { count, increment, decrement, reset, history };
}
```

## Wisdom of the Debug Ancients

- **Development Only**: useDebugValue only affects development builds, not production
- **Custom Hooks Only**: Only use in custom hooks, not in regular enchantments
- **Formatter Function**: Use the second parameter for expensive debug computations
- **Meaningful Labels**: Provide clear, helpful debug information for other developers

## Common Curses & Their Remedies

### Curse 1: Using in Regular Enchantments
Using useDebugValue in regular enchantments instead of custom hooks.

**Counter-Spell:**
Only use useDebugValue in custom hooks:

```jsx
// Cursed approach - in regular enchantment
function MyEnchantment() {
  const [count, setCount] = useState(0);
  useDebugValue(count); // Don't do this!
  return <div>{count}</div>;
}

// Pure spell approach - in custom hook
function useCounter() {
  const [count, setCount] = useState(0);
  useDebugValue(count); // This is correct!
  return [count, setCount];
}
```

### Curse 2: Expensive Debug Computations
Performing expensive operations directly in useDebugValue.

**Banishment Ritual:**
Use the formatter function for expensive computations:

```jsx
// Cursed approach - expensive computation
useDebugValue(JSON.stringify(complexObject)); // Runs every render!

// Pure spell approach - formatter function
useDebugValue(complexObject, (obj) =>
  JSON.stringify(obj) // Only runs when DevTools is open
);
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useDebugValue" %}

