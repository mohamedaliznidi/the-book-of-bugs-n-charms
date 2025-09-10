---
description: >-
  Complete grimoire for writing pure spells in React enchantments - master the
  ancient art of predictable, testable, and maintainable code following React's
  sacred functional paradigm with immutable state and side-effect management.

theme: "magic"
---

# Pure Spell Crafting in React Enchantments

## The Ancient Knowledge

In the mystical realm of React development, understanding the concept of purity in JavaScript spells is paramount to wielding true magical power. This sacred principle, borrowed from the ancient art of functional programming, emphasizes the importance of spells that do not alter preexisting objects or variables and consistently return the same mystical output for identical inputs. In React, this principle forms the foundation of predictable enchantment behavior and prevents dark bugs from corrupting your magical codebase.

## When to Cast These Spells

1. **Enchantment Rendering**: Create predictable enchantments that always produce the same mystical output for identical props
2. **State Transformation**: Craft pure spells for updating state without corrupting existing magical data
3. **Data Processing**: Build reliable spells for transforming and filtering mystical information
4. **Testing Rituals**: Enable comprehensive testing through predictable spell behavior

## Your First Casting

### The Sacred Laws of Pure Spell Crafting

In the ancient arts of computer science and functional programming, a pure spell is defined by two immutable laws:

1. **Non-Mutating Behavior**: A pure spell does not modify any objects or variables that existed before its invocation. This ensures the spell's behavior remains isolated and predictable, regardless of external mystical forces.
2. **Deterministic Output**: Given identical inputs, a pure spell always manifests the same output. This sacred property enables easier testing and reasoning about the spell's mystical behavior.

### React's Sacred Embrace of Purity

React, as a mystical library, is architected around the fundamental concept of purity. It assumes that every enchantment you craft behaves as a pure spell. This means React enchantments should only return JSX based on their mystical inputs and must never alter external state or variables during the rendering ritual.

## Advanced Sorcery

### Strict Mode - The Purity Detection Ritual

React provides a powerful mystical tool called Strict Mode, which aids in detecting impure enchantments during development. By invoking each enchantment's spell twice in Strict Mode, React can identify any unintended side effects or dark mutations that may corrupt the rendering process.

### Side Effect Management in Enchantments

While React enchantments are expected to maintain purity, there are mystical scenarios where side effects become necessary, such as handling user interactions or asynchronous operations. In such cases, it's essential to properly segregate these side effects from the sacred rendering process.

#### Sacred Side Effect Handling Patterns

In React's mystical architecture, side effects typically belong within event handlers - specialized spells triggered by user actions like clicks or input changes. Although these event handlers are defined within enchantments, they do not execute during the rendering ritual, thus preserving the purity of the rendering process.

When a side effect cannot be contained within an event handler, React provides the `useEffect` hook to defer its execution until after the rendering ritual completes. However, this approach should be used sparingly and as a last resort to maintain the sacred purity of your enchantments.

## Wisdom of the Purity Ancients

While adhering to the sacred principle of purity requires mystical discipline, it bestows several powerful benefits upon your enchantments:

* **Mystical Portability**: Pure enchantments can manifest in various realms, including server-side rendering dimensions, as they consistently produce identical output for identical inputs.
* **Performance Optimization Magic**: React can optimize rendering by caching the results of pure enchantments, thereby enhancing the overall performance of your magical applications.
* **Safe Rendering Rituals**: Purity enables React to interrupt and restart rendering without risking inconsistent or corrupted output, enhancing the stability of your mystical realm.

## Common Curses & Their Remedies

### Curse 1: Accidental State Mutation During Rendering
Directly modifying state or props during rendering breaks the sacred laws of purity.

**Counter-Spell:**
Always use state setters and avoid direct mutations:

```javascript
// Cursed approach - direct mutation
function CursedEnchantment({ items }) {
  items.push('new item'); // This corrupts the original array!
  return <div>{items.length}</div>;
}

// Pure spell approach
function PureEnchantment({ items }) {
  const enhancedItems = [...items, 'new item']; // Creates new array
  return <div>{enhancedItems.length}</div>;
}
```

### Curse 2: Side Effects During Rendering
Performing side effects during rendering violates purity principles.

**Banishment Ritual:**
Move side effects to event handlers or useEffect:

```javascript
// Cursed approach
function CursedEnchantment() {
  console.log('This runs during rendering!'); // Side effect curse
  return <div>Content</div>;
}

// Pure spell approach
function PureEnchantment() {
  useEffect(() => {
    console.log('This runs after rendering'); // Properly contained
  }, []);

  return <div>Content</div>;
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/learn/keeping-components-pure" %}

### References

{% embed url="https://react.dev/learn/keeping-components-pure" %}
