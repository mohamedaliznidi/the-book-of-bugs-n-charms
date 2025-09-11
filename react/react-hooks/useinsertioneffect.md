---
description: >-
  Complete grimoire for the useInsertionEffect enchantment - master the ancient art of
  CSS-in-JS and DOM manipulation timing in React enchantments, enabling precise control
  over style injection and layout calculations before browser paint.

theme: "magic"
---

# useInsertionEffect - The Pre-Paint Insertion Enchantment

## The Ancient Knowledge

The `useInsertionEffect` enchantment is a specialized React hook that fires synchronously before all DOM mutations, making it perfect for injecting styles into the DOM before the browser calculates layout and paint. This mystical spell is primarily designed for CSS-in-JS libraries and scenarios where you need to manipulate the DOM before React's effects run, ensuring optimal performance and preventing layout thrashing.

## When to Cast This Spell

1. **CSS-in-JS Libraries**: Inject dynamic styles before layout calculations
2. **Style Sheet Management**: Add or remove stylesheets dynamically
3. **Critical CSS Injection**: Insert critical styles before first paint
4. **DOM Measurements**: Prepare DOM for accurate measurements in useLayoutEffect
5. **Third-Party Library Integration**: Initialize libraries that need early DOM access

## Your First Casting

Here's a basic example of using `useInsertionEffect` for dynamic style injection:

```jsx
import React, { useInsertionEffect, useState } from 'react';

// Simple CSS-in-JS implementation
function useStyles(styles) {
  const [styleId] = useState(() => `style-${Math.random().toString(36).substr(2, 9)}`);

  useInsertionEffect(() => {
    // Create and inject style element before layout
    const styleElement = document.createElement('style');
    styleElement.id = styleId;
    styleElement.textContent = styles;
    document.head.appendChild(styleElement);

    return () => {
      // Cleanup: remove style element
      const element = document.getElementById(styleId);
      if (element) {
        element.remove();
      }
    };
  }, [styles, styleId]);

  return styleId;
}

// Usage component
function DynamicStyledComponent({ color, size }) {
  const styles = `
    .dynamic-component-${color}-${size} {
      background-color: ${color};
      width: ${size}px;
      height: ${size}px;
      border-radius: 8px;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-weight: bold;
    }
    
    .dynamic-component-${color}-${size}:hover {
      transform: scale(1.1);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
    }
  `;

  const styleId = useStyles(styles);
  const className = `dynamic-component-${color}-${size}`;

  return (
    <div className={className}>
      {color} {size}px
    </div>
  );
}

// Demo component
function StyleInjectionDemo() {
  const [color, setColor] = useState('blue');
  const [size, setSize] = useState(100);

  return (
    <div className="style-injection-demo">
      <div className="controls">
        <label>
          Color:
          <select value={color} onChange={(e) => setColor(e.target.value)}>
            <option value="blue">Blue</option>
            <option value="red">Red</option>
            <option value="green">Green</option>
            <option value="purple">Purple</option>
          </select>
        </label>
        
        <label>
          Size:
          <input
            type="range"
            min="50"
            max="200"
            value={size}
            onChange={(e) => setSize(parseInt(e.target.value))}
          />
          {size}px
        </label>
      </div>
      
      <div className="preview">
        <DynamicStyledComponent color={color} size={size} />
      </div>
    </div>
  );
}
```

## Advanced Sorcery

### CSS-in-JS Library Implementation

```jsx
import React, { useInsertionEffect, useRef, useState } from 'react';

// Advanced CSS-in-JS system
class StyleManager {
  constructor() {
    this.styles = new Map();
    this.styleElement = null;
    this.updateScheduled = false;
  }

  addStyle(id, css) {
    this.styles.set(id, css);
    this.scheduleUpdate();
  }

  removeStyle(id) {
    this.styles.delete(id);
    this.scheduleUpdate();
  }

  scheduleUpdate() {
    if (!this.updateScheduled) {
      this.updateScheduled = true;
      // Use microtask to batch updates
      Promise.resolve().then(() => {
        this.updateStyles();
        this.updateScheduled = false;
      });
    }
  }

  updateStyles() {
    if (!this.styleElement) {
      this.styleElement = document.createElement('style');
      this.styleElement.setAttribute('data-styled-components', '');
      document.head.appendChild(this.styleElement);
    }

    const combinedCSS = Array.from(this.styles.values()).join('\n');
    this.styleElement.textContent = combinedCSS;
  }

  cleanup() {
    if (this.styleElement) {
      this.styleElement.remove();
      this.styleElement = null;
    }
    this.styles.clear();
  }
}

// Global style manager instance
const globalStyleManager = new StyleManager();

// Advanced styled component hook
function useStyledComponent(tag, styles) {
  const [className] = useState(() => `styled-${Math.random().toString(36).substr(2, 9)}`);
  const styleIdRef = useRef(null);

  useInsertionEffect(() => {
    // Generate CSS with proper scoping
    const css = `.${className} { ${styles} }`;
    styleIdRef.current = className;
    
    // Add to global style manager
    globalStyleManager.addStyle(className, css);

    return () => {
      // Cleanup: remove from style manager
      if (styleIdRef.current) {
        globalStyleManager.removeStyle(styleIdRef.current);
      }
    };
  }, [className, styles]);

  // Return component factory
  return React.forwardRef((props, ref) => {
    return React.createElement(tag, {
      ...props,
      ref,
      className: `${className} ${props.className || ''}`.trim()
    });
  });
}

// Usage examples
function AdvancedStyledComponents() {
  const StyledButton = useStyledComponent('button', `
    background: linear-gradient(45deg, #667eea 0%, #764ba2 100%);
    border: none;
    border-radius: 8px;
    color: white;
    padding: 12px 24px;
    font-size: 16px;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.3s ease;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    
    &:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
    }
    
    &:active {
      transform: translateY(0);
    }
  `);

  const StyledCard = useStyledComponent('div', `
    background: white;
    border-radius: 12px;
    padding: 24px;
    box-shadow: 0 4px 24px rgba(0, 0, 0, 0.1);
    border: 1px solid #e1e5e9;
    transition: all 0.3s ease;
    
    &:hover {
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.15);
      transform: translateY(-4px);
    }
  `);

  return (
    <div className="advanced-styled-demo">
      <StyledCard>
        <h3>Mystical Card Component</h3>
        <p>This card is styled using our advanced CSS-in-JS system with useInsertionEffect.</p>
        <StyledButton onClick={() => alert('Magic button clicked!')}>
          Cast Spell
        </StyledButton>
      </StyledCard>
    </div>
  );
}
```

### Critical CSS Injection

```jsx
import React, { useInsertionEffect } from 'react';

// Critical CSS manager
class CriticalCSSManager {
  constructor() {
    this.injectedCSS = new Set();
    this.criticalStyleElement = null;
  }

  injectCriticalCSS(css, id) {
    if (this.injectedCSS.has(id)) return;

    if (!this.criticalStyleElement) {
      this.criticalStyleElement = document.createElement('style');
      this.criticalStyleElement.setAttribute('data-critical-css', '');
      // Insert at the beginning of head for highest priority
      document.head.insertBefore(this.criticalStyleElement, document.head.firstChild);
    }

    this.criticalStyleElement.textContent += css;
    this.injectedCSS.add(id);
  }

  removeCriticalCSS(id) {
    if (!this.injectedCSS.has(id)) return;
    
    this.injectedCSS.delete(id);
    
    // Rebuild critical CSS without the removed styles
    if (this.criticalStyleElement) {
      // In a real implementation, you'd track individual CSS blocks
      // This is a simplified version
      if (this.injectedCSS.size === 0) {
        this.criticalStyleElement.textContent = '';
      }
    }
  }
}

const criticalCSSManager = new CriticalCSSManager();

// Hook for critical CSS injection
function useCriticalCSS(css, id) {
  useInsertionEffect(() => {
    criticalCSSManager.injectCriticalCSS(css, id);
    
    return () => {
      criticalCSSManager.removeCriticalCSS(id);
    };
  }, [css, id]);
}

// Above-the-fold component with critical CSS
function HeroSection() {
  const criticalCSS = `
    .hero-section {
      height: 100vh;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      text-align: center;
    }
    
    .hero-title {
      font-size: 4rem;
      font-weight: 800;
      margin-bottom: 1rem;
      text-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
    
    .hero-subtitle {
      font-size: 1.5rem;
      opacity: 0.9;
      max-width: 600px;
    }
  `;

  useCriticalCSS(criticalCSS, 'hero-section');

  return (
    <section className="hero-section">
      <div>
        <h1 className="hero-title">Welcome to the Mystical Realm</h1>
        <p className="hero-subtitle">
          Discover the ancient arts of React enchantments and master the power of modern web sorcery.
        </p>
      </div>
    </section>
  );
}
```

### Theme System with Dynamic CSS Variables

```jsx
import React, { useInsertionEffect, useContext, createContext } from 'react';

// Theme context
const ThemeContext = createContext();

// Theme provider with CSS variable injection
function ThemeProvider({ children, theme }) {
  useInsertionEffect(() => {
    // Inject CSS variables for the current theme
    const cssVariables = Object.entries(theme)
      .map(([key, value]) => `--${key}: ${value}`)
      .join('; ');

    // Apply to document root
    document.documentElement.style.cssText += cssVariables;

    return () => {
      // Cleanup: remove theme variables
      Object.keys(theme).forEach(key => {
        document.documentElement.style.removeProperty(`--${key}`);
      });
    };
  }, [theme]);

  return (
    <ThemeContext.Provider value={theme}>
      {children}
    </ThemeContext.Provider>
  );
}

// Hook to use theme
function useTheme() {
  const theme = useContext(ThemeContext);
  if (!theme) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return theme;
}

// Component that uses theme variables
function ThemedComponent() {
  const theme = useTheme();

  const componentCSS = `
    .themed-component {
      background-color: var(--primary-color);
      color: var(--text-color);
      border: 2px solid var(--border-color);
      border-radius: var(--border-radius);
      padding: var(--spacing-md);
      transition: all 0.3s ease;
    }

    .themed-component:hover {
      background-color: var(--primary-hover);
      transform: translateY(-2px);
      box-shadow: 0 4px 12px var(--shadow-color);
    }
  `;

  useInsertionEffect(() => {
    const styleElement = document.createElement('style');
    styleElement.textContent = componentCSS;
    document.head.appendChild(styleElement);

    return () => styleElement.remove();
  }, [componentCSS]);

  return (
    <div className="themed-component">
      <h3>Themed Component</h3>
      <p>This component adapts to the current theme using CSS variables.</p>
    </div>
  );
}

// Usage with different themes
function ThemeDemo() {
  const [currentTheme, setCurrentTheme] = React.useState('light');

  const themes = {
    light: {
      'primary-color': '#3b82f6',
      'primary-hover': '#2563eb',
      'text-color': '#1f2937',
      'border-color': '#e5e7eb',
      'border-radius': '8px',
      'spacing-md': '16px',
      'shadow-color': 'rgba(0, 0, 0, 0.1)'
    },
    dark: {
      'primary-color': '#6366f1',
      'primary-hover': '#5b21b6',
      'text-color': '#f9fafb',
      'border-color': '#374151',
      'border-radius': '8px',
      'spacing-md': '16px',
      'shadow-color': 'rgba(0, 0, 0, 0.3)'
    },
    neon: {
      'primary-color': '#10b981',
      'primary-hover': '#059669',
      'text-color': '#ecfdf5',
      'border-color': '#34d399',
      'border-radius': '16px',
      'spacing-md': '20px',
      'shadow-color': 'rgba(16, 185, 129, 0.4)'
    }
  };

  return (
    <div className="theme-demo">
      <div className="theme-selector">
        {Object.keys(themes).map(themeName => (
          <button
            key={themeName}
            onClick={() => setCurrentTheme(themeName)}
            className={currentTheme === themeName ? 'active' : ''}
          >
            {themeName.charAt(0).toUpperCase() + themeName.slice(1)}
          </button>
        ))}
      </div>

      <ThemeProvider theme={themes[currentTheme]}>
        <ThemedComponent />
      </ThemeProvider>
    </div>
  );
}
```

### Performance Monitoring and Optimization

```jsx
import React, { useInsertionEffect, useRef } from 'react';

// Performance monitoring hook
function usePerformanceMonitor(componentName) {
  const startTimeRef = useRef();
  const metricsRef = useRef({
    insertionEffectTime: 0,
    layoutEffectTime: 0,
    renderTime: 0
  });

  useInsertionEffect(() => {
    const insertionStart = performance.now();

    // Simulate some insertion work
    const styleElement = document.createElement('style');
    styleElement.textContent = `
      .perf-monitor-${componentName} {
        position: relative;
      }
      .perf-monitor-${componentName}::before {
        content: "⚡ ${componentName}";
        position: absolute;
        top: -20px;
        left: 0;
        font-size: 12px;
        color: #666;
      }
    `;
    document.head.appendChild(styleElement);

    const insertionEnd = performance.now();
    metricsRef.current.insertionEffectTime = insertionEnd - insertionStart;

    return () => {
      styleElement.remove();

      // Log performance metrics
      console.log(`Performance metrics for ${componentName}:`, {
        insertionEffect: `${metricsRef.current.insertionEffectTime.toFixed(2)}ms`,
        total: `${(performance.now() - startTimeRef.current).toFixed(2)}ms`
      });
    };
  }, [componentName]);

  React.useLayoutEffect(() => {
    const layoutStart = performance.now();

    // Simulate layout work
    const element = document.querySelector(`.perf-monitor-${componentName}`);
    if (element) {
      const rect = element.getBoundingClientRect();
      // Do something with measurements
    }

    const layoutEnd = performance.now();
    metricsRef.current.layoutEffectTime = layoutEnd - layoutStart;
  }, [componentName]);

  // Track render start time
  startTimeRef.current = performance.now();

  return metricsRef.current;
}

// Component with performance monitoring
function MonitoredComponent({ name, children }) {
  const metrics = usePerformanceMonitor(name);

  return (
    <div className={`perf-monitor-${name}`}>
      <div className="component-content">
        {children}
      </div>
      <div className="performance-info">
        <small>
          Insertion: {metrics.insertionEffectTime.toFixed(2)}ms |
          Layout: {metrics.layoutEffectTime.toFixed(2)}ms
        </small>
      </div>
    </div>
  );
}
```

## Wisdom of the Insertion Ancients

- **Use Sparingly**: Only use for CSS-in-JS libraries or critical DOM manipulation before layout
- **Synchronous Execution**: Runs synchronously before DOM mutations, so keep operations fast
- **No DOM Reading**: Avoid reading from DOM in useInsertionEffect as layout hasn't been calculated
- **Style Injection Priority**: Perfect for injecting styles that need to be available before first paint
- **Server-Side Rendering**: Has no effect during SSR, only runs on the client
- **Performance Critical**: Blocking nature means expensive operations can delay rendering

## Common Curses & Their Remedies

### Curse 1: Using for Non-Critical Operations
Using useInsertionEffect for operations that don't need to run before layout.

**Counter-Spell:**
```jsx
// ❌ Using useInsertionEffect for regular side effects
useInsertionEffect(() => {
  fetchUserData(); // This doesn't need to run before layout!
}, []);

// ✅ Use useEffect for regular side effects
useEffect(() => {
  fetchUserData();
}, []);

// ✅ Use useInsertionEffect only for style injection
useInsertionEffect(() => {
  const style = document.createElement('style');
  style.textContent = dynamicCSS;
  document.head.appendChild(style);
  return () => style.remove();
}, [dynamicCSS]);
```

### Curse 2: Reading from DOM During Insertion
Attempting to read layout information before it's calculated.

**Banishment Ritual:**
```jsx
// ❌ Reading DOM measurements in useInsertionEffect
useInsertionEffect(() => {
  const element = document.getElementById('my-element');
  const rect = element.getBoundingClientRect(); // Layout not calculated yet!
}, []);

// ✅ Read DOM measurements in useLayoutEffect
useLayoutEffect(() => {
  const element = document.getElementById('my-element');
  const rect = element.getBoundingClientRect(); // Layout is calculated
}, []);
```

### Curse 3: Expensive Synchronous Operations
Performing heavy computations that block rendering.

**Protective Ward:**
```jsx
// ❌ Heavy computation in useInsertionEffect
useInsertionEffect(() => {
  const result = expensiveCalculation(); // Blocks rendering!
  injectStyles(result);
}, []);

// ✅ Pre-compute or use lighter operations
const precomputedStyles = useMemo(() => expensiveCalculation(), [deps]);

useInsertionEffect(() => {
  injectStyles(precomputedStyles); // Fast injection only
}, [precomputedStyles]);
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useInsertionEffect" %}

{% embed url="https://github.com/reactwg/react-18/discussions/110" %}

{% embed url="https://react.dev/learn/synchronizing-with-effects" %}
```
