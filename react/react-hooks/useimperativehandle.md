---
description: >-
  Complete grimoire for the useImperativeHandle enchantment - master the ancient
  art of parent exposure spells in React enchantments, customizing the mystical
  essence exposed to parent enchantments through forwardRef magic.

theme: "magic"
---

# useImperativeHandle - The Parent Exposure Spell

## The Ancient Knowledge

The `useImperativeHandle` enchantment customizes the mystical essence that is exposed to parent enchantments when using `forwardRef`. This rare and powerful spell allows child enchantments to expose specific functionality to their ancestors, breaking the typical data flow pattern for special cases where imperative access is necessary.

## When to Cast This Spell

1. **Custom Input Controls**: Expose focus, blur, and validation methods for custom input enchantments
2. **Animation Controls**: Provide play, pause, and reset methods for animation enchantments
3. **Modal Management**: Expose open, close, and toggle methods for modal enchantments
4. **Library Integration**: Bridge React enchantments with imperative third-party libraries
5. **Testing Utilities**: Expose internal methods for easier testing of enchantment behavior

## Your First Casting

Here's a basic example of a custom input with exposed methods:

```jsx
import React, { forwardRef, useImperativeHandle, useRef, useState } from 'react';

const MagicalInput = forwardRef((props, ref) => {
  const inputRef = useRef(null);
  const [value, setValue] = useState('');

  useImperativeHandle(ref, () => ({
    // Expose these methods to parent enchantments
    focus: () => {
      inputRef.current.focus();
    },
    blur: () => {
      inputRef.current.blur();
    },
    clear: () => {
      setValue('');
      inputRef.current.focus();
    },
    getValue: () => value,
    setValue: (newValue) => setValue(newValue)
  }));

  return (
    <input
      ref={inputRef}
      value={value}
      onChange={(e) => setValue(e.target.value)}
      placeholder="Mystical input field"
      {...props}
    />
  );
});

// Parent enchantment using the exposed methods
function ParentEnchantment() {
  const magicalInputRef = useRef(null);

  const handleFocus = () => {
    magicalInputRef.current.focus();
  };

  const handleClear = () => {
    magicalInputRef.current.clear();
  };

  const handleGetValue = () => {
    const value = magicalInputRef.current.getValue();
    alert(`Current value: ${value}`);
  };

  return (
    <div>
      <MagicalInput ref={magicalInputRef} />
      <button onClick={handleFocus}>Focus Input</button>
      <button onClick={handleClear}>Clear Input</button>
      <button onClick={handleGetValue}>Get Value</button>
    </div>
  );
}
```

## Advanced Sorcery

### Modal Control Enchantment

```jsx
const MagicalModal = forwardRef(({ children }, ref) => {
  const [isOpen, setIsOpen] = useState(false);
  const modalRef = useRef(null);

  useImperativeHandle(ref, () => ({
    open: () => setIsOpen(true),
    close: () => setIsOpen(false),
    toggle: () => setIsOpen(prev => !prev),
    isOpen: () => isOpen,
    focusModal: () => {
      if (modalRef.current) {
        modalRef.current.focus();
      }
    }
  }));

  if (!isOpen) return null;

  return (
    <div
      className="modal-backdrop"
      onClick={() => setIsOpen(false)}
    >
      <div
        ref={modalRef}
        className="modal-content"
        tabIndex={-1}
        onClick={(e) => e.stopPropagation()}
      >
        {children}
        <button onClick={() => setIsOpen(false)}>
          Close Portal
        </button>
      </div>
    </div>
  );
});

function ModalControllerEnchantment() {
  const modalRef = useRef(null);

  return (
    <div>
      <button onClick={() => modalRef.current.open()}>
        Open Mystical Portal
      </button>
      <button onClick={() => modalRef.current.toggle()}>
        Toggle Portal
      </button>

      <MagicalModal ref={modalRef}>
        <h2>Mystical Modal Content</h2>
        <p>This modal can be controlled imperatively!</p>
      </MagicalModal>
    </div>
  );
}
```

### Animation Control Enchantment

```jsx
const AnimatedEnchantment = forwardRef(({ children }, ref) => {
  const [isAnimating, setIsAnimating] = useState(false);
  const [animationClass, setAnimationClass] = useState('');

  useImperativeHandle(ref, () => ({
    startAnimation: (type = 'bounce') => {
      setAnimationClass(type);
      setIsAnimating(true);
    },
    stopAnimation: () => {
      setAnimationClass('');
      setIsAnimating(false);
    },
    isAnimating: () => isAnimating,
    pulse: () => {
      setAnimationClass('pulse');
      setIsAnimating(true);
      setTimeout(() => {
        setAnimationClass('');
        setIsAnimating(false);
      }, 1000);
    }
  }));

  return (
    <div
      className={`animated-element ${animationClass}`}
      style={{
        transition: 'all 0.3s ease',
        transform: isAnimating ? 'scale(1.1)' : 'scale(1)'
      }}
    >
      {children}
    </div>
  );
});
```

## Wisdom of the Imperative Ancients

- **Use Sparingly**: This spell breaks React's declarative nature - use only when necessary
- **forwardRef Required**: Must be used in conjunction with forwardRef
- **Dependency Array**: Include dependencies to control when the handle is recreated
- **Testing Benefits**: Exposed methods make enchantments easier to test imperatively

## Common Curses & Their Remedies

### Curse 1: Overusing Imperative Handles
Using imperative handles when declarative props would suffice.

**Counter-Spell:**
Prefer declarative props over imperative methods:

```jsx
// Cursed approach - unnecessary imperative API
useImperativeHandle(ref, () => ({
  setValue: (value) => setValue(value) // Could be a prop
}));

// Pure spell approach - declarative prop
function MagicalInput({ value, onChange }) {
  return <input value={value} onChange={onChange} />;
}
```

### Curse 2: Forgetting Dependency Array
Not including dependencies can lead to stale closures.

**Banishment Ritual:**
Always include dependencies when needed:

```jsx
// Cursed approach - missing dependencies
useImperativeHandle(ref, () => ({
  getCurrentValue: () => value // Stale closure!
}));

// Pure spell approach
useImperativeHandle(ref, () => ({
  getCurrentValue: () => value
}), [value]); // Include value in dependencies
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useImperativeHandle" %}

