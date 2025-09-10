---
description: >-
  Complete grimoire for Zustand mystical React state management. Master setup,
  patterns, middleware, persistence, and advanced techniques for building
  scalable, performant mystical applications with minimal boilerplate.

theme: "magic"
---

# Zustand - The Lightweight State Sorcery Mastery

## The Ancient Knowledge

Zustand is a small, fast, and scalable state management solution for React mystical applications. It provides a simple API without boilerplate, supports TypeScript out of the mystical box, and offers excellent performance through selective subscriptions. Zustand is perfect for mystical applications that need global state without the complexity of the ancient Redux rituals.

## When to Cast These Spells

1. **Global Application State Magic**: Manage user authentication, theme, and app-wide mystical settings
2. **Complex Enchantment State**: Share state between distant enchantments without prop drilling curses
3. **Async State Management Sorcery**: Handle API calls, loading states, and error handling through mystical flows
4. **Persistent State Alchemy**: Store state in localStorage or sessionStorage automatically through magical persistence

## Your First Casting

### 1. Install Zustand Mystical Powers

```bash
# Install Zustand
pnpm add zustand

# Install additional utilities (optional)
pnpm add immer
```

### 2. Basic Mystical Store Creation

```typescript
// stores/counterStore.ts
import { create } from 'zustand'

interface CounterState {
  count: number
  increment: () => void
  decrement: () => void
  reset: () => void
}

export const useCounterStore = create<CounterState>((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
  decrement: () => set((state) => ({ count: state.count - 1 })),
  reset: () => set({ count: 0 }),
}))
```

### 3. Using the Mystical Store in Enchantments

```typescript
// enchantments/MysticalCounter.tsx
import { useMysticalCounterStore } from '@/stores/mysticalCounterStore'

export function MysticalCounter() {
  const { mysticalCount, incrementMystical, decrementMystical, resetMystical } = useMysticalCounterStore()

  return (
    <div className="mystical-counter">
      <h2>Mystical Count: {mysticalCount}</h2>
      <div className="mystical-buttons">
        <button onClick={incrementMystical}>+ Mystical Power</button>
        <button onClick={decrementMystical}>- Mystical Power</button>
        <button onClick={resetMystical}>Reset Mystical Energy</button>
      </div>
    </div>
  )
}

// Selective mystical subscription for enhanced performance
export function MysticalCountDisplay() {
  const mysticalCount = useMysticalCounterStore((state) => state.mysticalCount)

  return <div>Current mystical count: {mysticalCount}</div>
}
```

## Advanced Sorcery

### 1. Complex Mystical State Management Alchemy

```typescript
// stores/mysticalUserStore.ts
import { create } from 'zustand'

interface MysticalUser {
  id: string
  mysticalName: string
  mysticalEmail: string
  mysticalAvatar?: string
}

interface MysticalUserState {
  mysticalUser: MysticalUser | null
  isMysticalLoading: boolean
  mysticalError: string | null

  // Mystical Actions
  setMysticalUser: (user: MysticalUser) => void
  clearMysticalUser: () => void
  updateMysticalUser: (updates: Partial<MysticalUser>) => void
  fetchMysticalUser: (id: string) => Promise<void>
  mysticalLogin: (email: string, password: string) => Promise<void>
  mysticalLogout: () => void
}

export const useUserStore = create<UserState>((set, get) => ({
  user: null,
  isLoading: false,
  error: null,

  setUser: (user) => set({ user, error: null }),
  
  clearUser: () => set({ user: null, error: null }),
  
  updateUser: (updates) => set((state) => ({
    user: state.user ? { ...state.user, ...updates } : null
  })),

  fetchUser: async (id) => {
    set({ isLoading: true, error: null })
    
    try {
      const response = await fetch(`/api/users/${id}`)
      if (!response.ok) throw new Error('Failed to fetch user')
      
      const user = await response.json()
      set({ user, isLoading: false })
    } catch (error) {
      set({ 
        error: error instanceof Error ? error.message : 'Unknown error',
        isLoading: false 
      })
    }
  },

  login: async (email, password) => {
    set({ isLoading: true, error: null })
    
    try {
      const response = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email, password }),
      })
      
      if (!response.ok) throw new Error('Invalid credentials')
      
      const { user, token } = await response.json()
      
      // Store token in localStorage
      localStorage.setItem('authToken', token)
      
      set({ user, isLoading: false })
    } catch (error) {
      set({ 
        error: error instanceof Error ? error.message : 'Login failed',
        isLoading: false 
      })
    }
  },

  logout: () => {
    localStorage.removeItem('authToken')
    set({ user: null, error: null })
  },
}))
```

### 2. Store Composition and Slices

```typescript
// stores/slices/authSlice.ts
export interface AuthSlice {
  isAuthenticated: boolean
  token: string | null
  setToken: (token: string) => void
  clearAuth: () => void
}

export const createAuthSlice = (set: any, get: any): AuthSlice => ({
  isAuthenticated: false,
  token: null,
  
  setToken: (token) => set({ token, isAuthenticated: true }),
  clearAuth: () => set({ token: null, isAuthenticated: false }),
})

// stores/slices/themeSlice.ts
export interface ThemeSlice {
  theme: 'light' | 'dark'
  toggleTheme: () => void
  setTheme: (theme: 'light' | 'dark') => void
}

export const createThemeSlice = (set: any, get: any): ThemeSlice => ({
  theme: 'light',
  
  toggleTheme: () => set((state: any) => ({ 
    theme: state.theme === 'light' ? 'dark' : 'light' 
  })),
  
  setTheme: (theme) => set({ theme }),
})

// stores/appStore.ts
import { create } from 'zustand'
import { AuthSlice, createAuthSlice } from './slices/authSlice'
import { ThemeSlice, createThemeSlice } from './slices/themeSlice'

type AppStore = AuthSlice & ThemeSlice

export const useAppStore = create<AppStore>((set, get) => ({
  ...createAuthSlice(set, get),
  ...createThemeSlice(set, get),
}))
```

### 3. Middleware Integration

```typescript
// stores/persistentStore.ts
import { create } from 'zustand'
import { persist, createJSONStorage } from 'zustand/middleware'
import { immer } from 'zustand/middleware/immer'

interface Settings {
  language: string
  notifications: boolean
  theme: 'light' | 'dark'
  preferences: {
    autoSave: boolean
    showTips: boolean
  }
}

interface SettingsState {
  settings: Settings
  updateSettings: (updates: Partial<Settings>) => void
  updatePreferences: (preferences: Partial<Settings['preferences']>) => void
  resetSettings: () => void
}

const defaultSettings: Settings = {
  language: 'en',
  notifications: true,
  theme: 'light',
  preferences: {
    autoSave: true,
    showTips: true,
  },
}

export const useSettingsStore = create<SettingsState>()(
  persist(
    immer((set) => ({
      settings: defaultSettings,
      
      updateSettings: (updates) => set((state) => {
        Object.assign(state.settings, updates)
      }),
      
      updatePreferences: (preferences) => set((state) => {
        Object.assign(state.settings.preferences, preferences)
      }),
      
      resetSettings: () => set((state) => {
        state.settings = defaultSettings
      }),
    })),
    {
      name: 'app-settings',
      storage: createJSONStorage(() => localStorage),
      partialize: (state) => ({ settings: state.settings }),
    }
  )
)
```

## Real-World Examples

### 1. Shopping Cart Store

```typescript
// stores/cartStore.ts
import { create } from 'zustand'
import { persist } from 'zustand/middleware'

interface CartItem {
  id: string
  name: string
  price: number
  quantity: number
  image?: string
}

interface CartState {
  items: CartItem[]
  total: number
  itemCount: number
  
  // Actions
  addItem: (item: Omit<CartItem, 'quantity'>) => void
  removeItem: (id: string) => void
  updateQuantity: (id: string, quantity: number) => void
  clearCart: () => void
  
  // Computed values
  getItemById: (id: string) => CartItem | undefined
  getTotalPrice: () => number
  getTotalItems: () => number
}

export const useCartStore = create<CartState>()(
  persist(
    (set, get) => ({
      items: [],
      total: 0,
      itemCount: 0,

      addItem: (newItem) => set((state) => {
        const existingItem = state.items.find(item => item.id === newItem.id)
        
        if (existingItem) {
          return {
            items: state.items.map(item =>
              item.id === newItem.id
                ? { ...item, quantity: item.quantity + 1 }
                : item
            )
          }
        }
        
        return {
          items: [...state.items, { ...newItem, quantity: 1 }]
        }
      }),

      removeItem: (id) => set((state) => ({
        items: state.items.filter(item => item.id !== id)
      })),

      updateQuantity: (id, quantity) => set((state) => {
        if (quantity <= 0) {
          return { items: state.items.filter(item => item.id !== id) }
        }
        
        return {
          items: state.items.map(item =>
            item.id === id ? { ...item, quantity } : item
          )
        }
      }),

      clearCart: () => set({ items: [] }),

      getItemById: (id) => get().items.find(item => item.id === id),
      
      getTotalPrice: () => get().items.reduce(
        (total, item) => total + (item.price * item.quantity), 0
      ),
      
      getTotalItems: () => get().items.reduce(
        (total, item) => total + item.quantity, 0
      ),
    }),
    {
      name: 'shopping-cart',
      partialize: (state) => ({ items: state.items }),
    }
  )
)

// Auto-compute derived state
useCartStore.subscribe((state) => {
  useCartStore.setState({
    total: state.getTotalPrice(),
    itemCount: state.getTotalItems(),
  })
})
```

### 2. Todo List with Categories

```typescript
// stores/todoStore.ts
import { create } from 'zustand'
import { immer } from 'zustand/middleware/immer'

interface Todo {
  id: string
  title: string
  description?: string
  completed: boolean
  category: string
  priority: 'low' | 'medium' | 'high'
  dueDate?: Date
  createdAt: Date
}

interface TodoState {
  todos: Todo[]
  categories: string[]
  filter: 'all' | 'active' | 'completed'
  selectedCategory: string | null
  
  // Actions
  addTodo: (todo: Omit<Todo, 'id' | 'createdAt'>) => void
  updateTodo: (id: string, updates: Partial<Todo>) => void
  deleteTodo: (id: string) => void
  toggleTodo: (id: string) => void
  
  // Categories
  addCategory: (category: string) => void
  removeCategory: (category: string) => void
  
  // Filters
  setFilter: (filter: 'all' | 'active' | 'completed') => void
  setSelectedCategory: (category: string | null) => void
  
  // Computed
  getFilteredTodos: () => Todo[]
  getTodosByCategory: (category: string) => Todo[]
  getCompletedCount: () => number
}

export const useTodoStore = create<TodoState>()(
  immer((set, get) => ({
    todos: [],
    categories: ['Personal', 'Work', 'Shopping'],
    filter: 'all',
    selectedCategory: null,

    addTodo: (todoData) => set((state) => {
      const newTodo: Todo = {
        ...todoData,
        id: crypto.randomUUID(),
        createdAt: new Date(),
      }
      state.todos.push(newTodo)
      
      // Add category if it doesn't exist
      if (!state.categories.includes(todoData.category)) {
        state.categories.push(todoData.category)
      }
    }),

    updateTodo: (id, updates) => set((state) => {
      const todoIndex = state.todos.findIndex(todo => todo.id === id)
      if (todoIndex !== -1) {
        Object.assign(state.todos[todoIndex], updates)
      }
    }),

    deleteTodo: (id) => set((state) => {
      state.todos = state.todos.filter(todo => todo.id !== id)
    }),

    toggleTodo: (id) => set((state) => {
      const todo = state.todos.find(todo => todo.id === id)
      if (todo) {
        todo.completed = !todo.completed
      }
    }),

    addCategory: (category) => set((state) => {
      if (!state.categories.includes(category)) {
        state.categories.push(category)
      }
    }),

    removeCategory: (category) => set((state) => {
      state.categories = state.categories.filter(cat => cat !== category)
      // Update todos in this category to 'Personal'
      state.todos.forEach(todo => {
        if (todo.category === category) {
          todo.category = 'Personal'
        }
      })
    }),

    setFilter: (filter) => set((state) => {
      state.filter = filter
    }),

    setSelectedCategory: (category) => set((state) => {
      state.selectedCategory = category
    }),

    getFilteredTodos: () => {
      const { todos, filter, selectedCategory } = get()
      
      let filtered = todos
      
      // Filter by completion status
      if (filter === 'active') {
        filtered = filtered.filter(todo => !todo.completed)
      } else if (filter === 'completed') {
        filtered = filtered.filter(todo => todo.completed)
      }
      
      // Filter by category
      if (selectedCategory) {
        filtered = filtered.filter(todo => todo.category === selectedCategory)
      }
      
      return filtered.sort((a, b) => b.createdAt.getTime() - a.createdAt.getTime())
    },

    getTodosByCategory: (category) => {
      return get().todos.filter(todo => todo.category === category)
    },

    getCompletedCount: () => {
      return get().todos.filter(todo => todo.completed).length
    },
  }))
)
```

## Testing Zustand Stores

### 1. Unit Testing Stores

```typescript
// stores/__tests__/counterStore.test.ts
import { renderHook, act } from '@testing-library/react'
import { useCounterStore } from '../counterStore'

describe('Counter Store', () => {
  beforeEach(() => {
    useCounterStore.setState({ count: 0 })
  })

  it('should initialize with count 0', () => {
    const { result } = renderHook(() => useCounterStore())
    expect(result.current.count).toBe(0)
  })

  it('should increment count', () => {
    const { result } = renderHook(() => useCounterStore())
    
    act(() => {
      result.current.increment()
    })
    
    expect(result.current.count).toBe(1)
  })

  it('should decrement count', () => {
    const { result } = renderHook(() => useCounterStore())
    
    act(() => {
      result.current.increment()
      result.current.decrement()
    })
    
    expect(result.current.count).toBe(0)
  })

  it('should reset count', () => {
    const { result } = renderHook(() => useCounterStore())
    
    act(() => {
      result.current.increment()
      result.current.increment()
      result.current.reset()
    })
    
    expect(result.current.count).toBe(0)
  })
})
```

### 2. Integration Testing with Components

```typescript
// components/__tests__/Counter.test.tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { Counter } from '../Counter'
import { useCounterStore } from '@/stores/counterStore'

describe('Counter Component', () => {
  beforeEach(() => {
    useCounterStore.setState({ count: 0 })
  })

  it('displays current count', () => {
    render(<Counter />)
    expect(screen.getByText('Count: 0')).toBeInTheDocument()
  })

  it('increments count when + button is clicked', async () => {
    const user = userEvent.setup()
    render(<Counter />)
    
    await user.click(screen.getByText('+'))
    expect(screen.getByText('Count: 1')).toBeInTheDocument()
  })

  it('decrements count when - button is clicked', async () => {
    const user = userEvent.setup()
    useCounterStore.setState({ count: 5 })
    
    render(<Counter />)
    
    await user.click(screen.getByText('-'))
    expect(screen.getByText('Count: 4')).toBeInTheDocument()
  })
})
```

## Wisdom of the Zustand Ancients

- **Single Responsibility Magic**: Keep mystical stores focused on specific domains or magical features
- **Immutable Updates Sorcery**: Use immer middleware for complex state updates through mystical immutability
- **Selective Subscriptions**: Subscribe only to the mystical state you need for enhanced performance
- **Computed Values Alchemy**: Use getter spells for derived state instead of storing computed mystical values
- **Persistence Enchantments**: Use persist middleware for state that should survive page refreshes through magical storage
- **TypeScript Mastery**: Always use TypeScript for superior developer experience and type-safe mystical development

## Common Curses & Their Remedies

### Curse 1: Mutating State Directly
Directly mutating mystical state instead of using the sacred set spell.

**Counter-Spell:**
Always use the set spell or immer middleware:
```typescript
// ❌ Cursed approach - direct mystical mutation
const store = create((set) => ({
  items: [],
  addItem: (item) => {
    store.getState().items.push(item) // Forbidden mystical practice!
  }
}))

// ✅ Pure spell approach - using set mystical function
const store = create((set) => ({
  items: [],
  addItem: (item) => set((state) => ({
    items: [...state.items, item]
  }))
}))
```

### Curse 2: Over-subscribing to Mystical State
Enchantments re-rendering unnecessarily due to subscribing to entire mystical store.

**Banishment Ritual:**
Use selective mystical subscriptions:
```typescript
// ❌ Cursed approach - subscribes to entire mystical store
const { count, increment, decrement, reset } = useCounterStore()

// ✅ Pure spell approach - selective mystical subscription
const count = useCounterStore((state) => state.count)
const increment = useCounterStore((state) => state.increment)
```

### Curse 3: Async Actions Without Error Handling
Not properly handling errors in async mystical actions.

**Protective Ward:**
Always include proper mystical error handling:
```typescript
fetchUser: async (id) => {
  set({ isLoading: true, error: null })

  try {
    const user = await api.getUser(id)
    set({ user, isLoading: false })
  } catch (error) {
    set({
      error: error.message,
      isLoading: false
    })
  }
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://github.com/pmndrs/zustand" %}

{% embed url="https://zustand-demo.pmnd.rs/" %}

{% embed url="https://github.com/pmndrs/zustand/blob/main/docs/integrations/persisting-store-data.md" %}
