---
description: >-
  Complete guide to XState for React applications. Learn state machines, actors,
  guards, actions, and advanced patterns for building predictable, robust
  applications with visual state management and excellent developer experience.
---

# XState

## Introduction

XState is a library for creating, interpreting, and executing finite state machines and statecharts. It provides a declarative way to manage complex application logic with visual state machines that are predictable, testable, and easy to understand. XState is particularly powerful for managing UI states, form flows, and complex business logic.

## Use Cases

1. **Complex UI States**: Manage loading, error, and success states with clear transitions
2. **Multi-Step Forms**: Handle form validation, navigation, and submission flows
3. **Authentication Flows**: Manage login, logout, and session states
4. **Game Logic**: Implement game states, player actions, and game flow
5. **API Integration**: Handle request states, retries, and error recovery

## Installation and Basic Setup

### 1. Install XState

```bash
# Install XState and React integration
pnpm add xstate @xstate/react

# Install additional utilities (optional)
pnpm add @xstate/inspect
```

### 2. Basic Machine Creation

```typescript
// machines/toggleMachine.ts
import { createMachine } from 'xstate'

export const toggleMachine = createMachine({
  id: 'toggle',
  initial: 'inactive',
  states: {
    inactive: {
      on: {
        TOGGLE: 'active'
      }
    },
    active: {
      on: {
        TOGGLE: 'inactive'
      }
    }
  }
})
```

## Core Concepts

### States and Transitions

```typescript
// machines/trafficLightMachine.ts
import { createMachine } from 'xstate'

export const trafficLightMachine = createMachine({
  id: 'trafficLight',
  initial: 'red',
  states: {
    red: {
      after: {
        5000: 'green' // Transition after 5 seconds
      }
    },
    yellow: {
      after: {
        2000: 'red'
      }
    },
    green: {
      after: {
        8000: 'yellow'
      }
    }
  }
})
```

### Context (Extended State)

```typescript
// machines/counterMachine.ts
import { assign, createMachine } from 'xstate'

interface CounterContext {
  count: number
  step: number
}

export const counterMachine = createMachine<CounterContext>({
  id: 'counter',
  initial: 'active',
  context: {
    count: 0,
    step: 1
  },
  states: {
    active: {
      on: {
        INCREMENT: {
          actions: assign({
            count: (context) => context.count + context.step
          })
        },
        DECREMENT: {
          actions: assign({
            count: (context) => context.count - context.step
          })
        },
        SET_STEP: {
          actions: assign({
            step: (_, event) => event.value
          })
        }
      }
    }
  }
})
```

### Guards (Conditional Transitions)

```typescript
// machines/authMachine.ts
import { createMachine, assign } from 'xstate'

interface AuthContext {
  user: User | null
  error: string | null
  attempts: number
}

export const authMachine = createMachine<AuthContext>({
  id: 'auth',
  initial: 'idle',
  context: {
    user: null,
    error: null,
    attempts: 0
  },
  states: {
    idle: {
      on: {
        LOGIN: 'authenticating'
      }
    },
    authenticating: {
      invoke: {
        src: 'authenticateUser',
        onDone: {
          target: 'authenticated',
          actions: assign({
            user: (_, event) => event.data,
            error: null
          })
        },
        onError: [
          {
            target: 'locked',
            guard: 'tooManyAttempts',
            actions: assign({
              error: 'Account locked due to too many failed attempts'
            })
          },
          {
            target: 'idle',
            actions: assign({
              error: (_, event) => event.data.message,
              attempts: (context) => context.attempts + 1
            })
          }
        ]
      }
    },
    authenticated: {
      on: {
        LOGOUT: {
          target: 'idle',
          actions: assign({
            user: null,
            attempts: 0
          })
        }
      }
    },
    locked: {
      after: {
        300000: { // 5 minutes
          target: 'idle',
          actions: assign({
            attempts: 0,
            error: null
          })
        }
      }
    }
  }
}, {
  guards: {
    tooManyAttempts: (context) => context.attempts >= 3
  },
  services: {
    authenticateUser: async (context, event) => {
      const response = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(event.credentials)
      })
      
      if (!response.ok) {
        throw new Error('Authentication failed')
      }
      
      return response.json()
    }
  }
})
```

## React Integration

### Using Machines in Components

```typescript
// components/Counter.tsx
import { useMachine } from '@xstate/react'
import { counterMachine } from '../machines/counterMachine'

export function Counter() {
  const [state, send] = useMachine(counterMachine)
  const { count, step } = state.context

  return (
    <div>
      <h2>Count: {count}</h2>
      <div>
        <button onClick={() => send('INCREMENT')}>
          +{step}
        </button>
        <button onClick={() => send('DECREMENT')}>
          -{step}
        </button>
      </div>
      <div>
        <label>
          Step:
          <input
            type="number"
            value={step}
            onChange={(e) => send({ 
              type: 'SET_STEP', 
              value: parseInt(e.target.value) || 1 
            })}
          />
        </label>
      </div>
    </div>
  )
}
```

### Authentication Component

```typescript
// components/AuthForm.tsx
import { useMachine } from '@xstate/react'
import { authMachine } from '../machines/authMachine'

export function AuthForm() {
  const [state, send] = useMachine(authMachine)
  const { user, error } = state.context

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault()
    const formData = new FormData(e.currentTarget)
    const credentials = {
      email: formData.get('email') as string,
      password: formData.get('password') as string
    }
    
    send({ type: 'LOGIN', credentials })
  }

  if (state.matches('authenticated')) {
    return (
      <div>
        <h2>Welcome, {user?.name}!</h2>
        <button onClick={() => send('LOGOUT')}>
          Logout
        </button>
      </div>
    )
  }

  if (state.matches('locked')) {
    return (
      <div>
        <h2>Account Locked</h2>
        <p>Too many failed attempts. Please try again in 5 minutes.</p>
      </div>
    )
  }

  return (
    <form onSubmit={handleSubmit}>
      <h2>Login</h2>
      {error && <div className="error">{error}</div>}
      
      <div>
        <label>
          Email:
          <input
            type="email"
            name="email"
            required
            disabled={state.matches('authenticating')}
          />
        </label>
      </div>
      
      <div>
        <label>
          Password:
          <input
            type="password"
            name="password"
            required
            disabled={state.matches('authenticating')}
          />
        </label>
      </div>
      
      <button 
        type="submit" 
        disabled={state.matches('authenticating')}
      >
        {state.matches('authenticating') ? 'Logging in...' : 'Login'}
      </button>
    </form>
  )
}
```

## Advanced Patterns

### Hierarchical States

```typescript
// machines/mediaMachine.ts
import { createMachine, assign } from 'xstate'

export const mediaMachine = createMachine({
  id: 'media',
  initial: 'loading',
  context: {
    currentTime: 0,
    duration: 0,
    volume: 1
  },
  states: {
    loading: {
      on: {
        LOADED: 'ready'
      }
    },
    ready: {
      initial: 'paused',
      states: {
        paused: {
          on: {
            PLAY: 'playing'
          }
        },
        playing: {
          on: {
            PAUSE: 'paused',
            END: 'ended'
          }
        },
        ended: {
          on: {
            PLAY: 'playing'
          }
        }
      },
      on: {
        SEEK: {
          actions: assign({
            currentTime: (_, event) => event.time
          })
        },
        VOLUME_CHANGE: {
          actions: assign({
            volume: (_, event) => event.volume
          })
        }
      }
    }
  }
})
```

### Parallel States

```typescript
// machines/appMachine.ts
import { createMachine } from 'xstate'

export const appMachine = createMachine({
  id: 'app',
  type: 'parallel',
  states: {
    auth: {
      initial: 'checking',
      states: {
        checking: {
          invoke: {
            src: 'checkAuth',
            onDone: 'authenticated',
            onError: 'unauthenticated'
          }
        },
        authenticated: {},
        unauthenticated: {}
      }
    },
    theme: {
      initial: 'light',
      states: {
        light: {
          on: { TOGGLE_THEME: 'dark' }
        },
        dark: {
          on: { TOGGLE_THEME: 'light' }
        }
      }
    },
    sidebar: {
      initial: 'closed',
      states: {
        closed: {
          on: { TOGGLE_SIDEBAR: 'open' }
        },
        open: {
          on: { TOGGLE_SIDEBAR: 'closed' }
        }
      }
    }
  }
})
```

### Actors and Communication

```typescript
// machines/chatMachine.ts
import { createMachine, spawn, assign } from 'xstate'
import { messageMachine } from './messageMachine'

export const chatMachine = createMachine({
  id: 'chat',
  initial: 'idle',
  context: {
    messages: [],
    messageActors: []
  },
  states: {
    idle: {
      on: {
        CONNECT: 'connected'
      }
    },
    connected: {
      on: {
        SEND_MESSAGE: {
          actions: assign({
            messageActors: (context, event) => [
              ...context.messageActors,
              spawn(messageMachine.withContext({
                id: event.id,
                text: event.text,
                timestamp: Date.now()
              }))
            ]
          })
        },
        DISCONNECT: 'idle'
      }
    }
  }
})
```

## Testing

### Testing Machines

```typescript
// __tests__/counterMachine.test.ts
import { interpret } from 'xstate'
import { counterMachine } from '../machines/counterMachine'

describe('Counter Machine', () => {
  test('should increment count', () => {
    const service = interpret(counterMachine).start()
    
    expect(service.state.context.count).toBe(0)
    
    service.send('INCREMENT')
    expect(service.state.context.count).toBe(1)
    
    service.stop()
  })

  test('should handle custom step', () => {
    const service = interpret(counterMachine).start()
    
    service.send({ type: 'SET_STEP', value: 5 })
    service.send('INCREMENT')
    
    expect(service.state.context.count).toBe(5)
    
    service.stop()
  })
})
```

### Testing Components

```typescript
// __tests__/Counter.test.tsx
import { render, screen, fireEvent } from '@testing-library/react'
import { Counter } from '../components/Counter'

describe('Counter Component', () => {
  test('should increment counter', () => {
    render(<Counter />)
    
    expect(screen.getByText('Count: 0')).toBeInTheDocument()
    
    fireEvent.click(screen.getByText('+1'))
    expect(screen.getByText('Count: 1')).toBeInTheDocument()
  })
})
```

## Development Tools

### XState Inspector

```typescript
// utils/inspector.ts
import { inspect } from '@xstate/inspect'

if (typeof window !== 'undefined' && process.env.NODE_ENV === 'development') {
  inspect({
    url: 'https://stately.ai/viz?inspect',
    iframe: false
  })
}

// In your component
const [state, send] = useMachine(counterMachine, {
  devTools: true // Enable dev tools
})
```

## Best Practices

### 1. Keep Machines Focused

```typescript
// Good: Focused machine
const loginMachine = createMachine({
  id: 'login',
  // Handle only login logic
})

// Good: Separate machine for different concerns
const userProfileMachine = createMachine({
  id: 'userProfile',
  // Handle only user profile logic
})
```

### 2. Use TypeScript

```typescript
// Define events
type AuthEvent = 
  | { type: 'LOGIN'; credentials: { email: string; password: string } }
  | { type: 'LOGOUT' }
  | { type: 'REFRESH_TOKEN' }

// Define context
interface AuthContext {
  user: User | null
  token: string | null
  error: string | null
}

// Type the machine
export const authMachine = createMachine<AuthContext, AuthEvent>({
  // Machine definition
})
```

### 3. Extract Services

```typescript
// services/authServices.ts
export const authServices = {
  authenticateUser: async (context: AuthContext, event: LoginEvent) => {
    // Authentication logic
  },
  refreshToken: async (context: AuthContext) => {
    // Token refresh logic
  }
}

// Use in machine
export const authMachine = createMachine({
  // ...
}, {
  services: authServices
})
```

## Performance Tips

1. **Use Selectors**: Extract only needed state parts
2. **Memoize Components**: Use React.memo for components using machines
3. **Lazy Services**: Load services only when needed
4. **Debounce Events**: Use debouncing for frequent events
5. **Optimize Context**: Keep context minimal and normalized

## Resources

- [Official XState Documentation](https://xstate.js.org/)
- [XState Visualizer](https://stately.ai/viz)
- [XState GitHub Repository](https://github.com/statelyai/xstate)
- [Stately Studio](https://stately.ai/studio)
- [XState Examples](https://github.com/statelyai/xstate/tree/main/examples)

## Migration Guide

### From useState to XState

```typescript
// Before: useState approach
const [isLoading, setIsLoading] = useState(false)
const [data, setData] = useState(null)
const [error, setError] = useState(null)

const fetchData = async () => {
  setIsLoading(true)
  setError(null)
  try {
    const result = await api.getData()
    setData(result)
  } catch (err) {
    setError(err.message)
  } finally {
    setIsLoading(false)
  }
}

// After: XState approach
const dataMachine = createMachine({
  initial: 'idle',
  states: {
    idle: {
      on: { FETCH: 'loading' }
    },
    loading: {
      invoke: {
        src: 'fetchData',
        onDone: {
          target: 'success',
          actions: assign({ data: (_, event) => event.data })
        },
        onError: {
          target: 'failure',
          actions: assign({ error: (_, event) => event.data })
        }
      }
    },
    success: {
      on: { FETCH: 'loading' }
    },
    failure: {
      on: { RETRY: 'loading' }
    }
  }
})
```

XState provides a powerful way to manage complex state logic with visual state machines that make your application's behavior predictable and easy to understand.
