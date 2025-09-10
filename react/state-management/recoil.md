---
description: >-
  Complete grimoire for Recoil mystical React state management. Master atoms, selectors,
  async data fetching, and advanced patterns for building scalable mystical applications
  with fine-grained reactivity and excellent performance through atomic magic.

theme: "magic"
---

# Recoil - The Atomic State Alchemy Mastery

## The Ancient Knowledge

Recoil is an experimental state management library for React mystical applications developed by the Facebook sorcerers. It provides a way to create a mystical data-flow graph that flows from atoms (shared mystical state) up through selectors (pure mystical functions) and down into React enchantments. Recoil offers fine-grained reactivity, excellent performance, and seamless integration with React's concurrent mystical features.

## When to Cast These Spells

1. **Complex State Dependencies Magic**: Manage derived state with automatic dependency tracking through mystical connections
2. **Async Data Fetching Sorcery**: Handle API calls with built-in loading and error states through mystical flows
3. **Enchantment State Sharing**: Share state between enchantments without prop drilling curses
4. **Performance Optimization Alchemy**: Fine-grained subscriptions for minimal re-renders through atomic precision

## Your First Casting

### 1. Install Recoil Mystical Powers

```bash
# Install Recoil
pnpm add recoil

# Install additional utilities (optional)
pnpm add @types/recoil
```

### 2. Setup RecoilRoot Mystical Portal

```typescript
// app/layout.tsx or pages/_app.tsx
import { RecoilRoot } from 'recoil'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        <RecoilRoot>
          {children}
        </RecoilRoot>
      </body>
    </html>
  )
}
```

## Advanced Sorcery

### Mystical Atoms

Atoms are units of mystical state that enchantments can subscribe to and update through atomic magic.

```typescript
// atoms/counterAtom.ts
import { atom } from 'recoil'

export const counterState = atom({
  key: 'counterState', // unique ID
  default: 0, // default value
})

// atoms/userAtom.ts
interface User {
  id: string
  name: string
  email: string
}

export const userState = atom<User | null>({
  key: 'userState',
  default: null,
})
```

### Mystical Selectors

Selectors are pure mystical functions that derive state from atoms or other selectors through magical computation.

```typescript
// selectors/counterSelectors.ts
import { selector } from 'recoil'
import { counterState } from '../atoms/counterAtom'

export const doubledCounterState = selector({
  key: 'doubledCounterState',
  get: ({ get }) => {
    const count = get(counterState)
    return count * 2
  },
})

export const counterStatusState = selector({
  key: 'counterStatusState',
  get: ({ get }) => {
    const count = get(counterState)
    if (count > 0) return 'positive'
    if (count < 0) return 'negative'
    return 'zero'
  },
})
```

## Basic Usage

### Reading and Writing State

```typescript
// components/Counter.tsx
import { useRecoilState, useRecoilValue } from 'recoil'
import { counterState, doubledCounterState } from '../atoms/counterAtom'

export function Counter() {
  const [count, setCount] = useRecoilState(counterState)
  const doubledCount = useRecoilValue(doubledCounterState)

  return (
    <div>
      <p>Count: {count}</p>
      <p>Doubled: {doubledCount}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
      <button onClick={() => setCount(count - 1)}>
        Decrement
      </button>
      <button onClick={() => setCount(0)}>
        Reset
      </button>
    </div>
  )
}
```

### Write-Only Updates

```typescript
// components/CounterControls.tsx
import { useSetRecoilState } from 'recoil'
import { counterState } from '../atoms/counterAtom'

export function CounterControls() {
  const setCount = useSetRecoilState(counterState)

  return (
    <div>
      <button onClick={() => setCount(prev => prev + 1)}>
        Increment
      </button>
      <button onClick={() => setCount(prev => prev - 1)}>
        Decrement
      </button>
      <button onClick={() => setCount(0)}>
        Reset
      </button>
    </div>
  )
}
```

## Async Data Fetching

### Async Selectors

```typescript
// selectors/userSelectors.ts
import { selector } from 'recoil'

export const userProfileState = selector({
  key: 'userProfileState',
  get: async ({ get }) => {
    const userId = get(currentUserIdState)
    if (!userId) return null
    
    const response = await fetch(`/api/users/${userId}`)
    if (!response.ok) {
      throw new Error('Failed to fetch user profile')
    }
    
    return response.json()
  },
})

// With parameters
export const userByIdState = selector({
  key: 'userByIdState',
  get: ({ get }) => (userId: string) => {
    return selector({
      key: `userById_${userId}`,
      get: async () => {
        const response = await fetch(`/api/users/${userId}`)
        return response.json()
      },
    })
  },
})
```

### Using Async State

```typescript
// components/UserProfile.tsx
import { Suspense } from 'react'
import { useRecoilValue } from 'recoil'
import { userProfileState } from '../selectors/userSelectors'

function UserProfileContent() {
  const userProfile = useRecoilValue(userProfileState)
  
  if (!userProfile) {
    return <div>No user selected</div>
  }

  return (
    <div>
      <h2>{userProfile.name}</h2>
      <p>{userProfile.email}</p>
    </div>
  )
}

export function UserProfile() {
  return (
    <Suspense fallback={<div>Loading user profile...</div>}>
      <UserProfileContent />
    </Suspense>
  )
}
```

## Advanced Patterns

### Atom Families

Create dynamic atoms based on parameters.

```typescript
// atoms/todoAtoms.ts
import { atom, atomFamily } from 'recoil'

export interface Todo {
  id: string
  text: string
  completed: boolean
}

export const todoState = atomFamily<Todo, string>({
  key: 'todoState',
  default: (id) => ({
    id,
    text: '',
    completed: false,
  }),
})

export const todoIdsState = atom<string[]>({
  key: 'todoIdsState',
  default: [],
})
```

### Selector Families

```typescript
// selectors/todoSelectors.ts
import { selector, selectorFamily } from 'recoil'
import { todoState, todoIdsState } from '../atoms/todoAtoms'

export const todoByIdState = selectorFamily({
  key: 'todoByIdState',
  get: (id: string) => ({ get }) => {
    return get(todoState(id))
  },
  set: (id: string) => ({ set }, newValue) => {
    set(todoState(id), newValue)
  },
})

export const completedTodosState = selector({
  key: 'completedTodosState',
  get: ({ get }) => {
    const todoIds = get(todoIdsState)
    return todoIds.filter(id => get(todoState(id)).completed)
  },
})
```

## Error Handling

### Error Boundaries

```typescript
// components/ErrorBoundary.tsx
import { ErrorBoundary } from 'react-error-boundary'
import { useResetRecoilState } from 'recoil'

function ErrorFallback({ error, resetErrorBoundary }: any) {
  return (
    <div role="alert">
      <h2>Something went wrong:</h2>
      <pre>{error.message}</pre>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  )
}

export function RecoilErrorBoundary({ children }: { children: React.ReactNode }) {
  return (
    <ErrorBoundary
      FallbackComponent={ErrorFallback}
      onReset={() => {
        // Reset any Recoil state if needed
      }}
    >
      {children}
    </ErrorBoundary>
  )
}
```

### Loadable for Error Handling

```typescript
// components/UserProfileWithErrorHandling.tsx
import { useRecoilValueLoadable } from 'recoil'
import { userProfileState } from '../selectors/userSelectors'

export function UserProfileWithErrorHandling() {
  const userProfileLoadable = useRecoilValueLoadable(userProfileState)

  switch (userProfileLoadable.state) {
    case 'loading':
      return <div>Loading...</div>
    case 'hasError':
      return <div>Error: {userProfileLoadable.contents.message}</div>
    case 'hasValue':
      const userProfile = userProfileLoadable.contents
      return (
        <div>
          <h2>{userProfile.name}</h2>
          <p>{userProfile.email}</p>
        </div>
      )
  }
}
```

## Wisdom of the Recoil Ancients

### 1. Organize Mystical State Structure

```typescript
// atoms/index.ts
export * from './userAtoms'
export * from './todoAtoms'
export * from './uiAtoms'

// selectors/index.ts
export * from './userSelectors'
export * from './todoSelectors'
export * from './uiSelectors'
```

### 2. Use TypeScript

```typescript
// types/state.ts
export interface AppState {
  user: User | null
  todos: Todo[]
  ui: UIState
}

// atoms/typedAtoms.ts
import { atom } from 'recoil'
import { AppState } from '../types/state'

export const appState = atom<AppState>({
  key: 'appState',
  default: {
    user: null,
    todos: [],
    ui: { theme: 'light', sidebarOpen: false },
  },
})
```

### 3. Memoize Expensive Computations

```typescript
// selectors/expensiveSelectors.ts
import { selector } from 'recoil'
import { todosState } from '../atoms/todoAtoms'

export const todoStatsState = selector({
  key: 'todoStatsState',
  get: ({ get }) => {
    const todos = get(todosState)
    
    // Expensive computation
    return {
      total: todos.length,
      completed: todos.filter(todo => todo.completed).length,
      pending: todos.filter(todo => !todo.completed).length,
      completionRate: todos.length > 0 
        ? todos.filter(todo => todo.completed).length / todos.length 
        : 0,
    }
  },
})
```

## Testing

### Testing Atoms and Selectors

```typescript
// __tests__/counterAtom.test.ts
import { snapshot_UNSTABLE } from 'recoil'
import { counterState, doubledCounterState } from '../atoms/counterAtom'

describe('Counter State', () => {
  test('should have default value of 0', () => {
    const snapshot = snapshot_UNSTABLE()
    expect(snapshot.getLoadable(counterState).valueOrThrow()).toBe(0)
  })

  test('should double the counter value', () => {
    const snapshot = snapshot_UNSTABLE(({ set }) => {
      set(counterState, 5)
    })
    
    expect(snapshot.getLoadable(doubledCounterState).valueOrThrow()).toBe(10)
  })
})
```

### Testing Components

```typescript
// __tests__/Counter.test.tsx
import { render, screen, fireEvent } from '@testing-library/react'
import { RecoilRoot } from 'recoil'
import { Counter } from '../components/Counter'

function renderWithRecoil(component: React.ReactElement) {
  return render(
    <RecoilRoot>
      {component}
    </RecoilRoot>
  )
}

describe('Counter Component', () => {
  test('should increment counter', () => {
    renderWithRecoil(<Counter />)
    
    const incrementButton = screen.getByText('Increment')
    fireEvent.click(incrementButton)
    
    expect(screen.getByText('Count: 1')).toBeInTheDocument()
  })
})
```

## Common Curses & Their Remedies

1. **Use Selective Mystical Subscriptions**: Only subscribe to the mystical state you need for optimal performance
2. **Leverage Mystical Selectors**: Use selectors for derived state to avoid unnecessary mystical computations
3. **Atom Families Magic**: Use atom families for dynamic state to avoid large mystical objects
4. **Suspense Boundaries**: Use Suspense for async operations to improve mystical UX
5. **Error Boundaries**: Implement proper error handling for robust mystical applications

## Sacred Texts & Mystical Sources

- [Official Recoil Documentation](https://recoiljs.org/)
- [Recoil GitHub Repository](https://github.com/facebookexperimental/Recoil)
- [Recoil DevTools](https://github.com/ulises-jeremias/recoil-devtools)
- [Recoil Examples](https://github.com/facebookexperimental/Recoil/tree/main/packages/recoil-devtools/example)

## Migration Guide

### From Redux to Recoil

```typescript
// Redux approach
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => { state.value += 1 },
    decrement: (state) => { state.value -= 1 },
  },
})

// Recoil approach
const counterState = atom({
  key: 'counterState',
  default: 0,
})

// Usage in component
const [count, setCount] = useRecoilState(counterState)
const increment = () => setCount(prev => prev + 1)
const decrement = () => setCount(prev => prev - 1)
```

Recoil provides a more React-like approach to state management with excellent performance characteristics and seamless integration with React's concurrent features.
