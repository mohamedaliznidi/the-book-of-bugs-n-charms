---
description: >-
  Complete grimoire for the useContext enchantment - master the ancient art of
  context communion in functional enchantments, accessing shared mystical state
  across the enchantment tree without prop drilling.

theme: "magic"
---

# useContext - The Context Communion Enchantment

## The Ancient Knowledge

The `useContext` enchantment provides a mystical way to consume React context within functional enchantments. This powerful spell allows you to access shared state, themes, user information, or any mystical data that needs to be available across multiple enchantments without the tedious ritual of prop drilling through every level of the enchantment tree.

## When to Cast This Spell

1. **Theme Management**: Access mystical theme data (dark/light mode, colors, fonts) across enchantments
2. **User Authentication**: Share user authentication state throughout your magical application
3. **Language Localization**: Provide translation spells and locale data to nested enchantments
4. **Global State Access**: Access application-wide state without complex prop passing
5. **Configuration Sharing**: Distribute configuration settings across enchantment hierarchies

## Your First Casting

Here's how to create and consume mystical context:

```jsx
import React, { createContext, useContext, useState } from 'react';

// Create mystical context
const ThemeContext = createContext();

// Provider enchantment
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Consumer enchantment
function MagicalButton() {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <button
      style={{
        background: theme === 'dark' ? '#333' : '#fff',
        color: theme === 'dark' ? '#fff' : '#333'
      }}
      onClick={() => setTheme(theme === 'dark' ? 'light' : 'dark')}
    >
      Toggle Theme Magic
    </button>
  );
}
```

## Advanced Sorcery

### Custom Mystical Context Hook Pattern

```jsx
// Create custom mystical hook for context
function useMysticalTheme() {
  const mysticalContext = useContext(MysticalThemeContext);
  if (!mysticalContext) {
    throw new Error('useMysticalTheme must be used within MysticalThemeProvider');
  }
  return mysticalContext;
}

// Usage in mystical enchantment
function ThemedMysticalEnchantment() {
  const { mysticalTheme, setMysticalTheme } = useMysticalTheme();
  // ... rest of mystical enchantment logic
}
```

## Wisdom of the Context Ancients

- **Provider Placement**: Place context providers as high as needed in the enchantment tree
- **Performance Considerations**: Context changes cause all consuming enchantments to re-render
- **Multiple Contexts**: You can use multiple contexts in a single enchantment
- **Default Values**: Always provide meaningful default values when creating context

## Common Curses & Their Remedies

### Curse 1: Context is Undefined
Attempting to use context outside of its provider results in undefined values.

**Counter-Spell:**
Always wrap consuming enchantments with the appropriate provider:

```jsx
// Cursed approach - no provider
function App() {
  return <MagicalButton />; // Context will be undefined
}

// Pure spell approach
function App() {
  return (
    <ThemeProvider>
      <MagicalButton />
    </ThemeProvider>
  );
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useContext" %}

