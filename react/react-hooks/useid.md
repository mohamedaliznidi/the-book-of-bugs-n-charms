---
description: >-
  Complete grimoire for the useId enchantment - master the ancient art of
  unique identifier generation in React enchantments, enabling accessible
  forms and components through mystical ID creation and SSR-safe practices.

theme: "magic"
---

# useId - The Unique Identifier Enchantment

## The Ancient Knowledge

The `useId` enchantment is a React hook that generates unique, stable identifiers that are consistent between server and client rendering. This mystical spell is essential for creating accessible forms and components, ensuring proper relationships between elements while maintaining SSR compatibility and avoiding hydration mismatches.

## When to Cast This Spell

1. **Accessible Form Elements**: Connect labels, inputs, and error messages with proper ARIA relationships
2. **Component Libraries**: Generate unique IDs for reusable components without conflicts
3. **SSR Applications**: Ensure consistent IDs between server and client rendering
4. **ARIA Attributes**: Create proper accessibility relationships with describedby, labelledby, etc.
5. **Multiple Instance Components**: Handle components that appear multiple times on the same page

## Your First Casting

Here's a basic example of using `useId` for accessible form elements:

```jsx
import React, { useId, useState } from 'react';

function AccessibleInput({ label, type = 'text', required = false }) {
  const id = useId();
  const [value, setValue] = useState('');
  const [error, setError] = useState('');

  const errorId = `${id}-error`;
  const helpId = `${id}-help`;

  const validateInput = (inputValue) => {
    if (required && !inputValue.trim()) {
      setError('This field is required');
    } else {
      setError('');
    }
  };

  return (
    <div className="form-field">
      <label htmlFor={id} className="field-label">
        {label}
        {required && <span className="required-indicator">*</span>}
      </label>
      
      <input
        id={id}
        type={type}
        value={value}
        onChange={(e) => {
          setValue(e.target.value);
          validateInput(e.target.value);
        }}
        aria-describedby={error ? errorId : helpId}
        aria-invalid={error ? 'true' : 'false'}
        className={`form-input ${error ? 'error' : ''}`}
      />
      
      {error && (
        <div id={errorId} className="error-message" role="alert">
          {error}
        </div>
      )}
      
      <div id={helpId} className="help-text">
        Enter your {label.toLowerCase()}
      </div>
    </div>
  );
}

// Usage
function RegistrationForm() {
  return (
    <form className="registration-form">
      <AccessibleInput label="Email" type="email" required />
      <AccessibleInput label="Password" type="password" required />
      <AccessibleInput label="Confirm Password" type="password" required />
    </form>
  );
}
```

## Advanced Sorcery

### Complex Form with Multiple ID Relationships

```jsx
import React, { useId, useState } from 'react';

function AdvancedFormField({ 
  label, 
  type = 'text', 
  required = false, 
  helpText, 
  validation,
  options = [] // For select/radio inputs
}) {
  const baseId = useId();
  const [value, setValue] = useState('');
  const [errors, setErrors] = useState([]);
  const [touched, setTouched] = useState(false);

  // Generate related IDs
  const inputId = baseId;
  const errorId = `${baseId}-error`;
  const helpId = `${baseId}-help`;
  const listId = `${baseId}-list`;

  const validateField = (inputValue) => {
    if (!validation) return [];
    
    const validationErrors = [];
    
    if (required && !inputValue.trim()) {
      validationErrors.push('This field is required');
    }
    
    if (validation.minLength && inputValue.length < validation.minLength) {
      validationErrors.push(`Minimum length is ${validation.minLength} characters`);
    }
    
    if (validation.pattern && !validation.pattern.test(inputValue)) {
      validationErrors.push(validation.message || 'Invalid format');
    }
    
    return validationErrors;
  };

  const handleChange = (newValue) => {
    setValue(newValue);
    if (touched) {
      setErrors(validateField(newValue));
    }
  };

  const handleBlur = () => {
    setTouched(true);
    setErrors(validateField(value));
  };

  const renderInput = () => {
    const commonProps = {
      id: inputId,
      value,
      onChange: (e) => handleChange(e.target.value),
      onBlur: handleBlur,
      'aria-describedby': [
        errors.length > 0 ? errorId : null,
        helpText ? helpId : null
      ].filter(Boolean).join(' '),
      'aria-invalid': errors.length > 0 ? 'true' : 'false',
      className: `form-input ${errors.length > 0 ? 'error' : ''}`
    };

    switch (type) {
      case 'select':
        return (
          <select {...commonProps}>
            <option value="">Choose an option</option>
            {options.map((option, index) => (
              <option key={`${baseId}-option-${index}`} value={option.value}>
                {option.label}
              </option>
            ))}
          </select>
        );
      
      case 'textarea':
        return <textarea {...commonProps} rows={4} />;
      
      case 'radio':
        return (
          <fieldset>
            <legend>{label}</legend>
            <div id={listId} role="radiogroup" aria-describedby={helpText ? helpId : undefined}>
              {options.map((option, index) => {
                const radioId = `${baseId}-radio-${index}`;
                return (
                  <div key={radioId} className="radio-option">
                    <input
                      type="radio"
                      id={radioId}
                      name={baseId}
                      value={option.value}
                      checked={value === option.value}
                      onChange={(e) => handleChange(e.target.value)}
                      onBlur={handleBlur}
                    />
                    <label htmlFor={radioId}>{option.label}</label>
                  </div>
                );
              })}
            </div>
          </fieldset>
        );
      
      default:
        return <input type={type} {...commonProps} />;
    }
  };

  return (
    <div className="advanced-form-field">
      {type !== 'radio' && (
        <label htmlFor={inputId} className="field-label">
          {label}
          {required && <span className="required-indicator">*</span>}
        </label>
      )}
      
      {renderInput()}
      
      {errors.length > 0 && (
        <div id={errorId} className="error-messages" role="alert">
          {errors.map((error, index) => (
            <div key={`${baseId}-error-${index}`} className="error-message">
              {error}
            </div>
          ))}
        </div>
      )}
      
      {helpText && (
        <div id={helpId} className="help-text">
          {helpText}
        </div>
      )}
    </div>
  );
}
```

### Reusable Component Library with Unique IDs

```jsx
import React, { useId, useState } from 'react';

function Modal({ title, children, isOpen, onClose }) {
  const modalId = useId();
  const titleId = `${modalId}-title`;
  const contentId = `${modalId}-content`;

  if (!isOpen) return null;

  return (
    <div 
      className="modal-overlay" 
      onClick={onClose}
      role="dialog"
      aria-modal="true"
      aria-labelledby={titleId}
      aria-describedby={contentId}
    >
      <div 
        className="modal-content" 
        onClick={(e) => e.stopPropagation()}
      >
        <div className="modal-header">
          <h2 id={titleId} className="modal-title">
            {title}
          </h2>
          <button 
            onClick={onClose}
            aria-label="Close modal"
            className="modal-close"
          >
            ×
          </button>
        </div>
        
        <div id={contentId} className="modal-body">
          {children}
        </div>
      </div>
    </div>
  );
}

function Accordion({ items }) {
  const baseId = useId();
  const [openItems, setOpenItems] = useState(new Set());

  const toggleItem = (index) => {
    setOpenItems(prev => {
      const newSet = new Set(prev);
      if (newSet.has(index)) {
        newSet.delete(index);
      } else {
        newSet.add(index);
      }
      return newSet;
    });
  };

  return (
    <div className="accordion">
      {items.map((item, index) => {
        const itemId = `${baseId}-item-${index}`;
        const headerId = `${itemId}-header`;
        const panelId = `${itemId}-panel`;
        const isOpen = openItems.has(index);

        return (
          <div key={itemId} className="accordion-item">
            <button
              id={headerId}
              className="accordion-header"
              onClick={() => toggleItem(index)}
              aria-expanded={isOpen}
              aria-controls={panelId}
            >
              {item.title}
              <span className={`accordion-icon ${isOpen ? 'open' : ''}`}>
                ▼
              </span>
            </button>
            
            <div
              id={panelId}
              className={`accordion-panel ${isOpen ? 'open' : ''}`}
              role="region"
              aria-labelledby={headerId}
              hidden={!isOpen}
            >
              <div className="accordion-content">
                {item.content}
              </div>
            </div>
          </div>
        );
      })}
    </div>
  );
}

function Tooltip({ children, content, position = 'top' }) {
  const tooltipId = useId();
  const [isVisible, setIsVisible] = useState(false);

  return (
    <div className="tooltip-container">
      <div
        onMouseEnter={() => setIsVisible(true)}
        onMouseLeave={() => setIsVisible(false)}
        onFocus={() => setIsVisible(true)}
        onBlur={() => setIsVisible(false)}
        aria-describedby={isVisible ? tooltipId : undefined}
      >
        {children}
      </div>
      
      {isVisible && (
        <div
          id={tooltipId}
          role="tooltip"
          className={`tooltip tooltip-${position}`}
        >
          {content}
        </div>
      )}
    </div>
  );
}
```

### SSR-Safe Component Patterns

```jsx
import React, { useId, useState, useEffect } from 'react';

function SSRSafeComponent() {
  const id = useId();
  const [isClient, setIsClient] = useState(false);

  // Ensure client-side hydration
  useEffect(() => {
    setIsClient(true);
  }, []);

  // The ID will be consistent between server and client
  const formId = `${id}-form`;
  const fieldsetId = `${id}-fieldset`;

  return (
    <div className="ssr-safe-component">
      <form id={formId}>
        <fieldset id={fieldsetId}>
          <legend>User Preferences</legend>

          <div className="preference-group">
            <input
              type="checkbox"
              id={`${id}-notifications`}
              name="notifications"
            />
            <label htmlFor={`${id}-notifications`}>
              Enable notifications
            </label>
          </div>

          <div className="preference-group">
            <input
              type="checkbox"
              id={`${id}-marketing`}
              name="marketing"
            />
            <label htmlFor={`${id}-marketing`}>
              Receive marketing emails
            </label>
          </div>

          {/* Client-only features */}
          {isClient && (
            <div className="client-only-features">
              <input
                type="checkbox"
                id={`${id}-analytics`}
                name="analytics"
              />
              <label htmlFor={`${id}-analytics`}>
                Allow analytics tracking
              </label>
            </div>
          )}
        </fieldset>
      </form>
    </div>
  );
}
```

## Wisdom of the ID Ancients

- **Always Use for Accessibility**: Essential for proper ARIA relationships and screen reader support
- **SSR Consistency**: IDs remain consistent between server and client rendering
- **Avoid Manual ID Generation**: Never create IDs manually - always use useId for uniqueness
- **Prefix for Related Elements**: Use the base ID as a prefix for related elements
- **Component Reusability**: Enables components to be used multiple times without ID conflicts
- **Form Accessibility**: Critical for connecting labels, inputs, and error messages

## Common Curses & Their Remedies

### Curse 1: Using Manual ID Generation
Creating IDs manually can lead to conflicts and SSR mismatches.

**Counter-Spell:**
```jsx
// ❌ Manual ID generation - dangerous!
function BadComponent() {
  const id = `input-${Math.random()}`;

  return (
    <div>
      <label htmlFor={id}>Name</label>
      <input id={id} />
    </div>
  );
}

// ✅ Use useId for safe, unique IDs
function GoodComponent() {
  const id = useId();

  return (
    <div>
      <label htmlFor={id}>Name</label>
      <input id={id} />
    </div>
  );
}
```

### Curse 2: Not Connecting Related Elements
Failing to properly connect form elements reduces accessibility.

**Banishment Ritual:**
```jsx
// ❌ Disconnected elements
function InaccessibleForm() {
  return (
    <div>
      <label>Email</label>
      <input type="email" />
      <div className="error">Invalid email</div>
    </div>
  );
}

// ✅ Properly connected with useId
function AccessibleForm() {
  const id = useId();
  const errorId = `${id}-error`;

  return (
    <div>
      <label htmlFor={id}>Email</label>
      <input
        id={id}
        type="email"
        aria-describedby={errorId}
        aria-invalid="true"
      />
      <div id={errorId} role="alert">Invalid email</div>
    </div>
  );
}
```

### Curse 3: Overusing IDs for Styling
Using IDs for CSS styling instead of classes reduces reusability.

**Protective Ward:**
```jsx
// ❌ Using IDs for styling
function StyledWithId() {
  const id = useId();

  return (
    <div>
      <input id={id} className="form-input" />
      <style>{`
        #${id} { border: 1px solid blue; }
      `}</style>
    </div>
  );
}

// ✅ Use IDs for accessibility, classes for styling
function ProperlyStyled() {
  const id = useId();

  return (
    <div>
      <label htmlFor={id}>Input</label>
      <input id={id} className="form-input primary" />
    </div>
  );
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useId" %}

{% embed url="https://www.w3.org/WAI/WCAG21/Understanding/labels-or-instructions.html" %}

{% embed url="https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA" %}
