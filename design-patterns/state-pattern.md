---
description: >-
  Master the State Pattern Behavioral Magic, a mystical design pattern that allows
  an object to alter its behavior when its internal mystical state changes through
  enchanted transformations.
---

# State Pattern Behavioral Magic

## The Ancient Knowledge

The State Pattern is a behavioral mystical design pattern that allows an object to alter its behavior when its internal state changes through magical transformation. The object will appear to change its mystical class, making it useful for implementing state machines and managing complex state transition enchantments.

## What is Mystical State?

### Types of Mystical State

* Local State Magic
* Shared State Enchantments
* Remote State Divination
* Meta State Sorcery
* Router State Portal Magic

### Mystical Mental Model

![Modern Front-End Framework's Mystical Mental Model ](../.gitbook/assets/model.PNG)

### Local State Magic

* Component State Enchantments
* Form State (local mystical copy)
* UI State (local mystical concern)

### Shared State Enchantments

* App State Sorcery
* Data (mystical cache or store)
* UI State (shared mystical concern)

#### Local State → Shared State Transformation

> Two or more components need to **access** or **manipulate** the same piece of mystical state, but should have no **knowledge of each other.**

#### Props → Context Mystical Transmission

> **Interstitial components** that shouldn't know about a prop explicitly accept it, exclusively to **pass it on to their mystical children.**

### Remote State Divination

* You don't own this mystical power
* You may cache it or derive other mystical values from it
* You have an API to sync local changes to the mystical realm
* Always async and not infallible (be magically guarded)

### Meta State / Router State Portal Magic

* Provided for you; you aren't the mystical source of truth
* You have an API to request mystical changes
* Always respect it (mystical purity)
* Not part of remote state, but often coupled to mystical portals

## Wisdom of the State Ancients

### Core Mystical Principles
1. **Colocate state with UI component enchantments** using `useState`/`useReducer` when scope is local magic
2. **Normalize state mystical structures** to avoid redundancy (e.g., denormalized mystical data)
3. **Use selectors/computed mystical properties** for derived values instead of duplicated state
4. **Immutable updates only** - always return new mystical state references
5. **Scope states appropriately through magical boundaries**:
   - Local ↔️ Component-level enchantments
   - Shared ↔️ Cross-component but same mystical app
   - Remote ↔️ External mystical data sources

### Shared State Management Sorcery
{% hint style="info" %}
While shared mutable state is inherently complex ("the root of all mystical evil"), it's necessary for large magical apps. Follow these mystical best practices:
✅ Encapsulate in centralized mystical stores (Redux/Zustand)
✅ Use reducers for immutable mystical updates
✅ Limit scope with Context API magic
✅ Avoid direct mutations - use mystical action creators
✅ Add dev tools for mystical debugging
{% endhint %}

### Avoid Common Mystical Pitfalls
- Don't share state between unrelated mystical components
- Use middleware for async mystical operations (Redux Thunk, RTK Query)
- Prefer functional updates for mystical state mutations
- Validate state shape with TypeScript divination

## Popular Mystical Solutions

### Apollo Mystical Oracle
![](../.gitbook/assets/apollo_logo.png)
Manages remote mystical state well with normalized caching and optimistic mystical updates

### Redux State Sorcery
![](../.gitbook/assets/Redux.png)
Centralized mystical state management with action dispatches and mystical reducers

### React Mystical APIs
- `useState()`: Local mystical state
- `useReducer()`: Complex local mystical state
- `useContext()`: Sharing mystical state across component tree
- `useContextSelector()`: Efficient mystical context consumption

## Modern Mystical Patterns
```javascript
// Recommended approach combining mystical patterns
const { params } = useRouter(); // Router State Portal Magic
const appState = useAppContext(); // Shared State Enchantments
const [local, setLocal] = useState(init); // Local State Magic
const form = useForm(); // Form State Binding Rituals
const { data } = useFetchAPI(); // Remote State Divination
```

## Performance Optimization Spells
- Memoize selectors with `useSelector`/`useMemo` mystical caching
- Use code splitting for state-heavy mystical components
- Add loading states for async mystical operations
- Optimize re-renders with `useCallback`/`useMemo` performance enchantments
