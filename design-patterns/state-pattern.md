---
description: >-
  Learn the State Pattern, a behavioral design pattern that allows an object
  to alter its behavior when its internal state changes.
---

# State Pattern

## Introduction

The State Pattern is a behavioral design pattern that allows an object to alter its behavior when its internal state changes. The object will appear to change its class, making it useful for implementing state machines and managing complex state transitions.

## What is State ?

### Types of State

* Local State
* Shared State
* Remote State
* Meta State
* Router State

### Mental Model

![Modern Front-End Framework's Mental Model ](../.gitbook/assets/model.PNG)

### Local State

* Component State
* Form State (local copy)
* UI State (local concern)

### Shared State

* App State
* Data (cache or store)
* UI State (shared concern)

#### Local State → Shared State

> Two or more components need to **access** or **manipulate** the same piece of state, but should have no **knowledge of each other.**

#### Props → Context

> **Interstitial components** that shouldn't know about a prop explicitly accept it, exclusively to **pass it on to their children.**

### Remote State

* You don't own it
* You may cache it or derive other values from it
* You have an API to sync local changes to it
* Always async and not infallible (be guarded)

### Meta State / Router State

* Provided for you; you aren't the source of truth
* You have an API to request it changes
* Always respect it (purity)
* Not part of remote state, but often coupled to it

## Best Practices

### Core Principles  
1. **Colocate state with UI components** using `useState`/`useReducer` when scope is local  
2. **Normalize state structures** to avoid redundancy (e.g., denormalized data)  
3. **Use selectors/computed properties** for derived values instead of duplicated state  
4. **Immutable updates only** - always return new state references  
5. **Scope states appropriately**:  
   - Local ↔️ Component-level  
   - Shared ↔️ Cross-component but same app  
   - Remote ↔️ External data sources  

### Shared State Management  
{% hint style="info" %}
While shared mutable state is inherently complex ("the root of all evil"), it's necessary for large apps. Follow these best practices:  
✅ Encapsulate in centralized stores (Redux/Zustand)  
✅ Use reducers for immutable updates  
✅ Limit scope with Context API  
✅ Avoid direct mutations - use action creators  
✅ Add dev tools for debugging  
{% endhint %}

### Avoid Common Pitfalls  
- Don't share state between unrelated components  
- Use middleware for async (Redux Thunk, RTK Query)  
- Prefer functional updates for state mutations  
- Validate state shape with TypeScript  

## Popular Solutions

### Apollo  
![](../.gitbook/assets/apollo_logo.png)  
Manages remote state well with normalized caching and optimistic updates  

### Redux  
![](../.gitbook/assets/Redux.png)  
Centralized state management with action dispatches and reducers  

### React APIs  
- `useState()`: Local state  
- `useReducer()`: Complex local state  
- `useContext()`: Sharing state across tree  
- `useContextSelector()`: Efficient context consumption  

## Modern Patterns  
```javascript
// Recommended approach combining patterns
const { params } = useRouter(); // Router State
const appState = useAppContext(); // Shared State
const [local, setLocal] = useState(init); // Local State
const form = useForm(); // Form State  
const { data } = useFetchAPI(); // Remote State
```

## Performance Tips  
- Memoize selectors with `useSelector`/`useMemo`
- Use code splitting for state-heavy components
- Add loading states for async operations
- Optimize re-renders with `useCallback`/`useMemo`
