---
description: >-
  Complete guide to Vitest for modern React testing. Learn setup, configuration,
  testing patterns, and integration with React Testing Library for fast,
  reliable unit and integration tests in Vite-powered applications.
---

# Vitest

## Introduction

Vitest is a blazing fast unit testing framework powered by Vite. It provides native ES modules support, TypeScript integration, and excellent performance through Vite's transformation pipeline. Vitest is designed as a drop-in replacement for Jest with better performance and modern features.

## Use Cases

1. **React Component Testing**: Unit and integration tests for React components
2. **Utility Function Testing**: Testing pure functions and business logic
3. **API Integration Testing**: Testing API calls and data transformations
4. **Hook Testing**: Testing custom React hooks with React Testing Library

## Installation and Setup

### 1. Install Vitest and Dependencies

```bash
# Install Vitest and testing utilities
pnpm add -D vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event

# Install jsdom for DOM environment
pnpm add -D jsdom

# Install additional testing utilities
pnpm add -D @vitest/ui @vitest/coverage-v8
```

### 2. Vitest Configuration

```typescript
// vitest.config.ts
import { defineConfig } from 'vitest/config'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  test: {
    // Test environment
    environment: 'jsdom',
    
    // Global test setup
    globals: true,
    setupFiles: ['./src/test/setup.ts'],
    
    // Coverage configuration
    coverage: {
      provider: 'v8',
      reporter: ['text', 'json', 'html'],
      exclude: [
        'node_modules/',
        'src/test/',
        '**/*.d.ts',
        '**/*.config.*',
      ],
    },
    
    // Test file patterns
    include: ['**/*.{test,spec}.{js,mjs,cjs,ts,mts,cts,jsx,tsx}'],
    exclude: ['node_modules', 'dist', '.idea', '.git', '.cache'],
  },
  
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@utils': path.resolve(__dirname, './src/utils'),
      '@hooks': path.resolve(__dirname, './src/hooks'),
    },
  },
})
```

### 3. Test Setup File

```typescript
// src/test/setup.ts
import '@testing-library/jest-dom'
import { expect, afterEach } from 'vitest'
import { cleanup } from '@testing-library/react'
import * as matchers from '@testing-library/jest-dom/matchers'

// Extend Vitest's expect with jest-dom matchers
expect.extend(matchers)

// Cleanup after each test
afterEach(() => {
  cleanup()
})

// Mock IntersectionObserver
global.IntersectionObserver = class IntersectionObserver {
  constructor() {}
  disconnect() {}
  observe() {}
  unobserve() {}
}

// Mock ResizeObserver
global.ResizeObserver = class ResizeObserver {
  constructor() {}
  disconnect() {}
  observe() {}
  unobserve() {}
}

// Mock matchMedia
Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: (query: string) => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: () => {},
    removeListener: () => {},
    addEventListener: () => {},
    removeEventListener: () => {},
    dispatchEvent: () => {},
  }),
})
```

## Basic Testing Patterns

### 1. Component Testing

```typescript
// components/Button.tsx
interface ButtonProps {
  children: React.ReactNode
  onClick?: () => void
  variant?: 'primary' | 'secondary'
  disabled?: boolean
}

export function Button({ children, onClick, variant = 'primary', disabled = false }: ButtonProps) {
  return (
    <button
      onClick={onClick}
      disabled={disabled}
      className={`btn btn-${variant} ${disabled ? 'btn-disabled' : ''}`}
    >
      {children}
    </button>
  )
}
```

```typescript
// components/Button.test.tsx
import { describe, it, expect, vi } from 'vitest'
import { render, screen, fireEvent } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { Button } from './Button'

describe('Button', () => {
  it('renders children correctly', () => {
    render(<Button>Click me</Button>)
    expect(screen.getByRole('button', { name: 'Click me' })).toBeInTheDocument()
  })

  it('calls onClick when clicked', async () => {
    const handleClick = vi.fn()
    const user = userEvent.setup()
    
    render(<Button onClick={handleClick}>Click me</Button>)
    
    await user.click(screen.getByRole('button'))
    expect(handleClick).toHaveBeenCalledTimes(1)
  })

  it('applies correct variant class', () => {
    render(<Button variant="secondary">Secondary Button</Button>)
    expect(screen.getByRole('button')).toHaveClass('btn-secondary')
  })

  it('disables button when disabled prop is true', () => {
    render(<Button disabled>Disabled Button</Button>)
    expect(screen.getByRole('button')).toBeDisabled()
  })

  it('does not call onClick when disabled', async () => {
    const handleClick = vi.fn()
    const user = userEvent.setup()
    
    render(<Button onClick={handleClick} disabled>Disabled Button</Button>)
    
    await user.click(screen.getByRole('button'))
    expect(handleClick).not.toHaveBeenCalled()
  })
})
```

### 2. Hook Testing

```typescript
// hooks/useCounter.ts
import { useState, useCallback } from 'react'

export function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue)

  const increment = useCallback(() => {
    setCount(prev => prev + 1)
  }, [])

  const decrement = useCallback(() => {
    setCount(prev => prev - 1)
  }, [])

  const reset = useCallback(() => {
    setCount(initialValue)
  }, [initialValue])

  return { count, increment, decrement, reset }
}
```

```typescript
// hooks/useCounter.test.ts
import { describe, it, expect } from 'vitest'
import { renderHook, act } from '@testing-library/react'
import { useCounter } from './useCounter'

describe('useCounter', () => {
  it('initializes with default value', () => {
    const { result } = renderHook(() => useCounter())
    expect(result.current.count).toBe(0)
  })

  it('initializes with custom value', () => {
    const { result } = renderHook(() => useCounter(10))
    expect(result.current.count).toBe(10)
  })

  it('increments count', () => {
    const { result } = renderHook(() => useCounter())
    
    act(() => {
      result.current.increment()
    })
    
    expect(result.current.count).toBe(1)
  })

  it('decrements count', () => {
    const { result } = renderHook(() => useCounter(5))
    
    act(() => {
      result.current.decrement()
    })
    
    expect(result.current.count).toBe(4)
  })

  it('resets to initial value', () => {
    const { result } = renderHook(() => useCounter(10))
    
    act(() => {
      result.current.increment()
      result.current.increment()
    })
    
    expect(result.current.count).toBe(12)
    
    act(() => {
      result.current.reset()
    })
    
    expect(result.current.count).toBe(10)
  })
})
```

### 3. Async Testing

```typescript
// utils/api.ts
export async function fetchUser(id: string) {
  const response = await fetch(`/api/users/${id}`)
  if (!response.ok) {
    throw new Error('Failed to fetch user')
  }
  return response.json()
}

export async function createUser(userData: { name: string; email: string }) {
  const response = await fetch('/api/users', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(userData),
  })
  
  if (!response.ok) {
    throw new Error('Failed to create user')
  }
  
  return response.json()
}
```

```typescript
// utils/api.test.ts
import { describe, it, expect, vi, beforeEach } from 'vitest'
import { fetchUser, createUser } from './api'

// Mock fetch globally
global.fetch = vi.fn()

describe('API functions', () => {
  beforeEach(() => {
    vi.resetAllMocks()
  })

  describe('fetchUser', () => {
    it('fetches user successfully', async () => {
      const mockUser = { id: '1', name: 'John Doe', email: 'john@example.com' }
      
      vi.mocked(fetch).mockResolvedValueOnce({
        ok: true,
        json: async () => mockUser,
      } as Response)

      const result = await fetchUser('1')
      
      expect(fetch).toHaveBeenCalledWith('/api/users/1')
      expect(result).toEqual(mockUser)
    })

    it('throws error when fetch fails', async () => {
      vi.mocked(fetch).mockResolvedValueOnce({
        ok: false,
        status: 404,
      } as Response)

      await expect(fetchUser('1')).rejects.toThrow('Failed to fetch user')
    })
  })

  describe('createUser', () => {
    it('creates user successfully', async () => {
      const userData = { name: 'Jane Doe', email: 'jane@example.com' }
      const mockResponse = { id: '2', ...userData }
      
      vi.mocked(fetch).mockResolvedValueOnce({
        ok: true,
        json: async () => mockResponse,
      } as Response)

      const result = await createUser(userData)
      
      expect(fetch).toHaveBeenCalledWith('/api/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(userData),
      })
      expect(result).toEqual(mockResponse)
    })
  })
})
```

## Advanced Testing Patterns

### 1. Testing with Context

```typescript
// contexts/ThemeContext.tsx
import { createContext, useContext, useState } from 'react'

type Theme = 'light' | 'dark'

interface ThemeContextType {
  theme: Theme
  toggleTheme: () => void
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined)

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [theme, setTheme] = useState<Theme>('light')

  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light')
  }

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  )
}

export function useTheme() {
  const context = useContext(ThemeContext)
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider')
  }
  return context
}
```

```typescript
// components/ThemeToggle.test.tsx
import { describe, it, expect } from 'vitest'
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { ThemeProvider, useTheme } from '../contexts/ThemeContext'

function ThemeToggle() {
  const { theme, toggleTheme } = useTheme()
  
  return (
    <button onClick={toggleTheme}>
      Current theme: {theme}
    </button>
  )
}

function renderWithTheme(ui: React.ReactElement) {
  return render(
    <ThemeProvider>
      {ui}
    </ThemeProvider>
  )
}

describe('ThemeToggle', () => {
  it('displays current theme and toggles on click', async () => {
    const user = userEvent.setup()
    
    renderWithTheme(<ThemeToggle />)
    
    expect(screen.getByText('Current theme: light')).toBeInTheDocument()
    
    await user.click(screen.getByRole('button'))
    
    expect(screen.getByText('Current theme: dark')).toBeInTheDocument()
  })
})
```

### 2. Testing Server Components

```typescript
// components/ServerUserList.tsx
import { getUsers } from '@/lib/database'

export default async function ServerUserList() {
  const users = await getUsers()

  return (
    <div>
      <h2>Users</h2>
      <ul>
        {users.map(user => (
          <li key={user.id}>{user.name} - {user.email}</li>
        ))}
      </ul>
    </div>
  )
}
```

```typescript
// components/ServerUserList.test.tsx
import { describe, it, expect, vi } from 'vitest'
import { render, screen } from '@testing-library/react'
import ServerUserList from './ServerUserList'

// Mock the database function
vi.mock('@/lib/database', () => ({
  getUsers: vi.fn(),
}))

describe('ServerUserList', () => {
  it('renders user list', async () => {
    const mockUsers = [
      { id: '1', name: 'John Doe', email: 'john@example.com' },
      { id: '2', name: 'Jane Smith', email: 'jane@example.com' },
    ]

    const { getUsers } = await import('@/lib/database')
    vi.mocked(getUsers).mockResolvedValue(mockUsers)

    // Render the server component
    const component = await ServerUserList()
    render(component)

    expect(screen.getByText('Users')).toBeInTheDocument()
    expect(screen.getByText('John Doe - john@example.com')).toBeInTheDocument()
    expect(screen.getByText('Jane Smith - jane@example.com')).toBeInTheDocument()
  })
})
```

### 3. Testing with MSW (Mock Service Worker)

```bash
# Install MSW
pnpm add -D msw
```

```typescript
// src/test/mocks/handlers.ts
import { rest } from 'msw'

export const handlers = [
  rest.get('/api/users', (req, res, ctx) => {
    return res(
      ctx.json([
        { id: '1', name: 'John Doe', email: 'john@example.com' },
        { id: '2', name: 'Jane Smith', email: 'jane@example.com' },
      ])
    )
  }),

  rest.post('/api/users', (req, res, ctx) => {
    return res(
      ctx.json({ id: '3', name: 'New User', email: 'new@example.com' })
    )
  }),

  rest.get('/api/users/:id', (req, res, ctx) => {
    const { id } = req.params
    return res(
      ctx.json({ id, name: 'User Name', email: 'user@example.com' })
    )
  }),
]
```

```typescript
// src/test/mocks/server.ts
import { setupServer } from 'msw/node'
import { handlers } from './handlers'

export const server = setupServer(...handlers)
```

```typescript
// src/test/setup.ts (updated)
import '@testing-library/jest-dom'
import { expect, afterEach, beforeAll, afterAll } from 'vitest'
import { cleanup } from '@testing-library/react'
import * as matchers from '@testing-library/jest-dom/matchers'
import { server } from './mocks/server'

expect.extend(matchers)

// Start MSW server
beforeAll(() => server.listen())
afterEach(() => {
  cleanup()
  server.resetHandlers()
})
afterAll(() => server.close())
```

## Package.json Scripts

```json
{
  "scripts": {
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:run": "vitest run",
    "test:coverage": "vitest run --coverage",
    "test:watch": "vitest --watch"
  }
}
```

## Best Practices

- **Fast Feedback**: Use Vitest's watch mode for instant test feedback during development
- **Isolated Tests**: Each test should be independent and not rely on other tests
- **Descriptive Names**: Use clear, descriptive test names that explain what is being tested
- **Arrange-Act-Assert**: Structure tests with clear setup, action, and assertion phases
- **Mock External Dependencies**: Mock API calls, database connections, and external services
- **Test User Interactions**: Focus on testing user behavior rather than implementation details

## Common Pitfalls

### Issue 1: Async Test Timing
Tests may fail due to improper handling of async operations.

**Solution:**
Use proper async/await patterns and waitFor utilities:
```typescript
// ❌ Wrong - not waiting for async operation
it('loads data', () => {
  render(<AsyncComponent />)
  expect(screen.getByText('Data loaded')).toBeInTheDocument()
})

// ✅ Correct - waiting for async operation
it('loads data', async () => {
  render(<AsyncComponent />)
  await waitFor(() => {
    expect(screen.getByText('Data loaded')).toBeInTheDocument()
  })
})
```

### Issue 2: Testing Implementation Details
Testing internal component state instead of user-visible behavior.

**Solution:**
Focus on testing what users see and do:
```typescript
// ❌ Wrong - testing implementation
expect(component.state.isLoading).toBe(false)

// ✅ Correct - testing user-visible behavior
expect(screen.queryByText('Loading...')).not.toBeInTheDocument()
```

### Issue 3: Insufficient Test Cleanup
Tests affecting each other due to improper cleanup.

**Solution:**
Use proper cleanup and reset mocks:
```typescript
afterEach(() => {
  cleanup()
  vi.resetAllMocks()
})
```

## References

{% embed url="https://vitest.dev/" %}

{% embed url="https://testing-library.com/docs/react-testing-library/intro/" %}

{% embed url="https://mswjs.io/" %}
