---
description: >-
  Complete grimoire for the useReducer enchantment - master the ancient art of
  complex state alchemy in React enchantments, managing intricate state logic
  with reducer spells and dispatch rituals for predictable state transformations.

theme: "magic"
---

# useReducer - The Complex State Alchemy Enchantment

## The Ancient Knowledge

The `useReducer` enchantment is a powerful alternative to `useState` for managing complex state logic in React enchantments. This mystical hook accepts a reducer spell and initial state, returning the current state and a dispatch ritual to trigger state transformations. It's particularly potent when dealing with state that involves multiple sub-values or when the next state depends on the previous one.

## When to Cast This Spell

1. **Complex State Logic**: Manage state with multiple related values or complex update patterns
2. **State Transitions**: Handle predictable state transitions with clear action types
3. **Shared State Logic**: Extract and reuse state logic across multiple enchantments
4. **Testing Benefits**: Easier to test reducer functions in isolation
5. **Performance Optimization**: Optimize deep updates by passing dispatch down instead of callbacks

## Your First Casting

Here's a basic counter example using the `useReducer` spell:

```jsx
import React, { useReducer } from 'react';

// Reducer spell - defines how state transforms
function counterReducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      throw new Error(`Unknown action type: ${action.type}`);
  }
}

function CounterEnchantment() {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });

  return (
    <div>
      <p>Mystical Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>
        Increment Spell
      </button>
      <button onClick={() => dispatch({ type: 'decrement' })}>
        Decrement Spell
      </button>
      <button onClick={() => dispatch({ type: 'reset' })}>
        Reset Ritual
      </button>
    </div>
  );
}
```

## Advanced Sorcery

### Complex State Management

```jsx
const initialState = {
  user: null,
  loading: false,
  error: null,
  posts: []
};

function appReducer(state, action) {
  switch (action.type) {
    case 'FETCH_USER_START':
      return { ...state, loading: true, error: null };

    case 'FETCH_USER_SUCCESS':
      return {
        ...state,
        loading: false,
        user: action.payload,
        error: null
      };

    case 'FETCH_USER_ERROR':
      return {
        ...state,
        loading: false,
        error: action.payload
      };

    case 'ADD_POST':
      return {
        ...state,
        posts: [...state.posts, action.payload]
      };

    default:
      return state;
  }
}

function UserDashboardEnchantment() {
  const [state, dispatch] = useReducer(appReducer, initialState);

  const fetchUser = async (userId) => {
    dispatch({ type: 'FETCH_USER_START' });
    try {
      const user = await api.getUser(userId);
      dispatch({ type: 'FETCH_USER_SUCCESS', payload: user });
    } catch (error) {
      dispatch({ type: 'FETCH_USER_ERROR', payload: error.message });
    }
  };

  return (
    <div>
      {state.loading && <div>Loading mystical user data...</div>}
      {state.error && <div>Error: {state.error}</div>}
      {state.user && <UserProfile user={state.user} />}
    </div>
  );
}
```

## Wisdom of the Reducer Ancients

- **Pure Reducer Spells**: Reducer functions should be pure and predictable
- **Immutable Updates**: Always return new state objects, never mutate existing state
- **Action Patterns**: Use consistent action object patterns with type and payload
- **State Shape**: Design your state shape to be as flat and normalized as possible

## Common Curses & Their Remedies

### Curse 1: Mutating State Directly
Directly modifying state breaks React's update detection.

**Counter-Spell:**
Always return new state objects:

```jsx
// Cursed approach - mutating state
function badReducer(state, action) {
  state.count++; // Direct mutation!
  return state;
}

// Pure spell approach
function goodReducer(state, action) {
  return { ...state, count: state.count + 1 };
}
```

### Curse 2: Missing Action Types
Forgetting to handle action types can lead to unexpected behavior.

**Banishment Ritual:**
Always include a default case:

```jsx
function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useReducer" %}

