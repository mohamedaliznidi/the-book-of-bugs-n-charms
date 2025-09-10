---
description: >-
  Master the mystical arts of React Testing Library patterns and sacred practices.
  Learn effective testing enchantments, query methods, user interaction spells, and
  accessibility-focused divination for modern React component manifestations.

theme: "magic"
---

# The React Testing Library Enchantment Grimoire

## The Ancient Knowledge

React Testing Library is a mystical testing utility that encourages testing React components in a way that resembles how users interact with them in the real world. It focuses on testing behavior rather than implementation details, promoting more maintainable and reliable testing enchantments that mirror actual user experiences.

## When to Cast These Spells

1. **Component Behavior Testing Rituals**: Test how components respond to user interactions and mystical events
2. **Accessibility Testing Divination**: Ensure components are accessible to all users through proper mystical attributes
3. **Integration Testing Sorcery**: Test component interactions and data flow between mystical realms
4. **User Experience Testing Enchantments**: Verify the user journey through your application manifestations

## Your First Casting

### 1. Testing Philosophy Enchantments

```typescript
// ❌ Wrong - Testing implementation details
import { shallow } from 'enzyme'

it('updates state when button is clicked', () => {
  const wrapper = shallow(<Counter />)
  wrapper.find('button').simulate('click')
  expect(wrapper.state('count')).toBe(1)
})

// ✅ Correct - Testing user behavior
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'

it('increments counter when button is clicked', async () => {
  const user = userEvent.setup()
  render(<Counter />)
  
  const button = screen.getByRole('button', { name: /increment/i })
  await user.click(button)
  
  expect(screen.getByText('Count: 1')).toBeInTheDocument()
})
```

### 2. Query Priority Hierarchy

```typescript
// Priority order for queries (most to least preferred)

// 1. Accessible to everyone (screen readers, etc.)
screen.getByRole('button', { name: /submit/i })
screen.getByLabelText(/username/i)
screen.getByPlaceholderText(/enter username/i)
screen.getByText(/welcome/i)

// 2. Semantic queries
screen.getByDisplayValue(/john doe/i)

// 3. Test IDs (last resort)
screen.getByTestId('submit-button')
```

## Advanced Sorcery

### 1. Query Types Enchantments

```typescript
import { render, screen } from '@testing-library/react'

function UserProfile({ user }: { user: { name: string; email: string } }) {
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
      <button>Edit Profile</button>
    </div>
  )
}

describe('Query methods', () => {
  const mockUser = { name: 'John Doe', email: 'john@example.com' }

  beforeEach(() => {
    render(<UserProfile user={mockUser} />)
  })

  it('demonstrates getBy queries - throws if not found', () => {
    // Single element, throws if not found or multiple found
    expect(screen.getByText('John Doe')).toBeInTheDocument()
    expect(screen.getByRole('button', { name: /edit profile/i })).toBeInTheDocument()
  })

  it('demonstrates queryBy queries - returns null if not found', () => {
    // Single element, returns null if not found
    expect(screen.queryByText('Jane Doe')).toBeNull()
    expect(screen.queryByText('John Doe')).toBeInTheDocument()
  })

  it('demonstrates findBy queries - async, waits for element', async () => {
    // Async, waits for element to appear
    const heading = await screen.findByText('John Doe')
    expect(heading).toBeInTheDocument()
  })

  it('demonstrates getAllBy queries - multiple elements', () => {
    render(
      <div>
        <button>Button 1</button>
        <button>Button 2</button>
      </div>
    )
    
    const buttons = screen.getAllByRole('button')
    expect(buttons).toHaveLength(3) // Including the Edit Profile button
  })
})
```

### 2. Query Options Spells

```typescript
describe('Query options', () => {
  it('uses exact matching', () => {
    render(<button>Save Changes</button>)
    
    // Exact match
    expect(screen.getByText('Save Changes', { exact: true })).toBeInTheDocument()
    
    // Partial match (default)
    expect(screen.getByText('Save')).toBeInTheDocument()
  })

  it('uses custom text matching', () => {
    render(<p>The quick brown fox jumps over the lazy dog</p>)
    
    // String match
    expect(screen.getByText('quick brown fox')).toBeInTheDocument()
    
    // Regex match
    expect(screen.getByText(/quick.*fox/i)).toBeInTheDocument()
    
    // Function match
    expect(screen.getByText((content, element) => {
      return content.includes('fox') && element?.tagName.toLowerCase() === 'p'
    })).toBeInTheDocument()
  })

  it('uses role options', () => {
    render(
      <form>
        <label htmlFor="username">Username</label>
        <input id="username" type="text" />
        <button type="submit">Submit</button>
      </form>
    )
    
    // Role with name
    expect(screen.getByRole('textbox', { name: /username/i })).toBeInTheDocument()
    expect(screen.getByRole('button', { name: /submit/i })).toBeInTheDocument()
  })
})
```

## Master's Rituals

### 1. User Events Enchantments

```typescript
import userEvent from '@testing-library/user-event'

describe('User interactions', () => {
  it('handles click events', async () => {
    const handleClick = vi.fn()
    const user = userEvent.setup()
    
    render(<button onClick={handleClick}>Click me</button>)
    
    await user.click(screen.getByRole('button'))
    expect(handleClick).toHaveBeenCalledTimes(1)
  })

  it('handles keyboard input', async () => {
    const user = userEvent.setup()
    
    render(<input placeholder="Enter text" />)
    
    const input = screen.getByPlaceholderText('Enter text')
    await user.type(input, 'Hello, World!')
    
    expect(input).toHaveValue('Hello, World!')
  })

  it('handles form submission', async () => {
    const handleSubmit = vi.fn()
    const user = userEvent.setup()
    
    render(
      <form onSubmit={handleSubmit}>
        <input name="username" placeholder="Username" />
        <input name="password" type="password" placeholder="Password" />
        <button type="submit">Login</button>
      </form>
    )
    
    await user.type(screen.getByPlaceholderText('Username'), 'john')
    await user.type(screen.getByPlaceholderText('Password'), 'password123')
    await user.click(screen.getByRole('button', { name: /login/i }))
    
    expect(handleSubmit).toHaveBeenCalled()
  })

  it('handles keyboard navigation', async () => {
    const user = userEvent.setup()
    
    render(
      <div>
        <button>First</button>
        <button>Second</button>
        <button>Third</button>
      </div>
    )
    
    const firstButton = screen.getByRole('button', { name: 'First' })
    firstButton.focus()
    
    await user.keyboard('{Tab}')
    expect(screen.getByRole('button', { name: 'Second' })).toHaveFocus()
    
    await user.keyboard('{Tab}')
    expect(screen.getByRole('button', { name: 'Third' })).toHaveFocus()
  })
})
```

### 2. Complex Interaction Sorcery

```typescript
describe('Complex user interactions', () => {
  it('handles drag and drop', async () => {
    const user = userEvent.setup()
    
    render(
      <div>
        <div data-testid="draggable" draggable>Drag me</div>
        <div data-testid="droppable">Drop here</div>
      </div>
    )
    
    const draggable = screen.getByTestId('draggable')
    const droppable = screen.getByTestId('droppable')
    
    await user.dragAndDrop(draggable, droppable)
    
    // Verify the drag and drop behavior
  })

  it('handles file upload', async () => {
    const user = userEvent.setup()
    const file = new File(['hello'], 'hello.png', { type: 'image/png' })
    
    render(<input type="file" />)
    
    const input = screen.getByRole('textbox', { hidden: true })
    await user.upload(input, file)
    
    expect(input.files[0]).toBe(file)
    expect(input.files).toHaveLength(1)
  })

  it('handles hover interactions', async () => {
    const user = userEvent.setup()
    
    render(
      <div>
        <button>Hover me</button>
        <div data-testid="tooltip" style={{ display: 'none' }}>
          Tooltip content
        </div>
      </div>
    )
    
    const button = screen.getByRole('button')
    await user.hover(button)
    
    // Verify hover effects
  })
})
```

### 3. Async Testing Divination

#### 1. Waiting for Elements to Manifest

```typescript
import { waitFor, waitForElementToBeRemoved } from '@testing-library/react'

describe('Async testing', () => {
  it('waits for elements to appear', async () => {
    function AsyncComponent() {
      const [loading, setLoading] = useState(true)
      const [data, setData] = useState(null)
      
      useEffect(() => {
        setTimeout(() => {
          setData('Loaded data')
          setLoading(false)
        }, 1000)
      }, [])
      
      if (loading) return <div>Loading...</div>
      return <div>{data}</div>
    }
    
    render(<AsyncComponent />)
    
    expect(screen.getByText('Loading...')).toBeInTheDocument()
    
    // Wait for the loading to disappear
    await waitForElementToBeRemoved(() => screen.queryByText('Loading...'))
    
    // Wait for the data to appear
    expect(await screen.findByText('Loaded data')).toBeInTheDocument()
  })

  it('waits for state changes', async () => {
    function Counter() {
      const [count, setCount] = useState(0)
      
      const increment = () => {
        setTimeout(() => setCount(c => c + 1), 100)
      }
      
      return (
        <div>
          <span>Count: {count}</span>
          <button onClick={increment}>Increment</button>
        </div>
      )
    }
    
    const user = userEvent.setup()
    render(<Counter />)
    
    await user.click(screen.getByRole('button'))
    
    await waitFor(() => {
      expect(screen.getByText('Count: 1')).toBeInTheDocument()
    })
  })

  it('handles API calls', async () => {
    function UserList() {
      const [users, setUsers] = useState([])
      const [loading, setLoading] = useState(true)
      
      useEffect(() => {
        fetch('/api/users')
          .then(res => res.json())
          .then(data => {
            setUsers(data)
            setLoading(false)
          })
      }, [])
      
      if (loading) return <div>Loading users...</div>
      
      return (
        <ul>
          {users.map(user => (
            <li key={user.id}>{user.name}</li>
          ))}
        </ul>
      )
    }
    
    // Mock the API call
    global.fetch = vi.fn().mockResolvedValue({
      json: () => Promise.resolve([
        { id: 1, name: 'John Doe' },
        { id: 2, name: 'Jane Smith' }
      ])
    })
    
    render(<UserList />)
    
    expect(screen.getByText('Loading users...')).toBeInTheDocument()
    
    await waitFor(() => {
      expect(screen.getByText('John Doe')).toBeInTheDocument()
      expect(screen.getByText('Jane Smith')).toBeInTheDocument()
    })
  })
})
```

### 4. Accessibility Testing Enchantments

#### 1. ARIA and Semantic Testing Spells

```typescript
describe('Accessibility testing', () => {
  it('tests ARIA labels and roles', () => {
    render(
      <nav aria-label="Main navigation">
        <ul>
          <li><a href="/">Home</a></li>
          <li><a href="/about">About</a></li>
          <li><a href="/contact">Contact</a></li>
        </ul>
      </nav>
    )
    
    expect(screen.getByRole('navigation', { name: 'Main navigation' })).toBeInTheDocument()
    expect(screen.getByRole('link', { name: 'Home' })).toBeInTheDocument()
  })

  it('tests form accessibility', () => {
    render(
      <form>
        <label htmlFor="email">Email Address</label>
        <input
          id="email"
          type="email"
          required
          aria-describedby="email-error"
        />
        <div id="email-error" role="alert">
          Please enter a valid email address
        </div>
        <button type="submit">Submit</button>
      </form>
    )
    
    expect(screen.getByLabelText('Email Address')).toBeInTheDocument()
    expect(screen.getByRole('alert')).toBeInTheDocument()
    expect(screen.getByRole('button', { name: 'Submit' })).toBeInTheDocument()
  })

  it('tests keyboard navigation', async () => {
    const user = userEvent.setup()
    
    render(
      <div>
        <button>First</button>
        <a href="#" tabIndex={0}>Link</a>
        <input type="text" />
        <button>Last</button>
      </div>
    )
    
    // Test tab order
    await user.tab()
    expect(screen.getByRole('button', { name: 'First' })).toHaveFocus()
    
    await user.tab()
    expect(screen.getByRole('link')).toHaveFocus()
    
    await user.tab()
    expect(screen.getByRole('textbox')).toHaveFocus()
    
    await user.tab()
    expect(screen.getByRole('button', { name: 'Last' })).toHaveFocus()
  })
})
```

#### 2. Screen Reader Testing Divination

```typescript
describe('Screen reader compatibility', () => {
  it('provides proper heading structure', () => {
    render(
      <article>
        <h1>Main Article Title</h1>
        <section>
          <h2>Section Title</h2>
          <h3>Subsection Title</h3>
          <p>Content goes here</p>
        </section>
      </article>
    )
    
    expect(screen.getByRole('heading', { level: 1, name: 'Main Article Title' })).toBeInTheDocument()
    expect(screen.getByRole('heading', { level: 2, name: 'Section Title' })).toBeInTheDocument()
    expect(screen.getByRole('heading', { level: 3, name: 'Subsection Title' })).toBeInTheDocument()
  })

  it('provides alternative text for images', () => {
    render(
      <div>
        <img src="profile.jpg" alt="John Doe's profile picture" />
        <img src="decorative.jpg" alt="" role="presentation" />
      </div>
    )
    
    expect(screen.getByAltText("John Doe's profile picture")).toBeInTheDocument()
    // Decorative images should not be found by screen readers
    expect(screen.queryByAltText('decorative.jpg')).not.toBeInTheDocument()
  })
})
```

### 5. Custom Render Functions Sorcery

#### 1. Render with Providers Enchantments

```typescript
// test-utils.tsx
import { render, RenderOptions } from '@testing-library/react'
import { ReactElement } from 'react'
import { BrowserRouter } from 'react-router-dom'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { ThemeProvider } from '../contexts/ThemeContext'

interface CustomRenderOptions extends Omit<RenderOptions, 'wrapper'> {
  initialEntries?: string[]
  queryClient?: QueryClient
}

function AllTheProviders({ children }: { children: React.ReactNode }) {
  const queryClient = new QueryClient({
    defaultOptions: {
      queries: { retry: false },
      mutations: { retry: false },
    },
  })

  return (
    <BrowserRouter>
      <QueryClientProvider client={queryClient}>
        <ThemeProvider>
          {children}
        </ThemeProvider>
      </QueryClientProvider>
    </BrowserRouter>
  )
}

function customRender(
  ui: ReactElement,
  options?: CustomRenderOptions
) {
  return render(ui, { wrapper: AllTheProviders, ...options })
}

// Re-export everything
export * from '@testing-library/react'
export { customRender as render }
```

#### 2. Using Custom Render Spells

```typescript
// Component.test.tsx
import { render, screen } from '../test-utils' // Use custom render

describe('Component with providers', () => {
  it('renders with all providers', () => {
    render(<MyComponent />)
    // Component now has access to Router, React Query, Theme, etc.
    expect(screen.getByText('Component content')).toBeInTheDocument()
  })
})
```

## Wisdom of the Ancients

- **Query by Accessibility**: Use queries that match how users and assistive technologies interact with your mystical components
- **Test User Behavior**: Focus on what users see and do, not implementation details or internal component sorcery
- **Async Patterns**: Use proper async utilities for testing dynamic content manifestations
- **Accessibility First**: Ensure your tests verify accessibility features and mystical attributes
- **Custom Utilities**: Create reusable test utilities for common patterns and enchantments
- **Meaningful Assertions**: Write assertions that verify the actual user experience and interface behavior

## Common Curses & Their Remedies

### Curse 1: The Implementation Details Trap
Testing internal state or methods instead of user behavior, leading to brittle and meaningless tests.

**Counter-Spell:**
Focus on user-visible behavior and mystical interactions:
```typescript
// ❌ Wrong - testing implementation
expect(component.state.isOpen).toBe(true)

// ✅ Correct - testing user-visible behavior
expect(screen.getByText('Modal is open')).toBeInTheDocument()
```

### Curse 2: The Async Operations Timing Hex
Tests failing because they don't wait for async operations to complete their mystical manifestations.

**Counter-Spell:**
Use proper async utilities for reliable timing enchantments:
```typescript
// ❌ Wrong - not waiting
expect(screen.getByText('Loaded')).toBeInTheDocument()

// ✅ Correct - waiting for element
expect(await screen.findByText('Loaded')).toBeInTheDocument()
```

### Curse 3: The Poor Query Selection Curse
Using test IDs when semantic queries would work better, missing opportunities for accessibility validation.

**Counter-Spell:**
Prefer semantic queries that mirror user interactions:
```typescript
// ❌ Less preferred - test ID
screen.getByTestId('submit-button')

// ✅ Better - semantic query
screen.getByRole('button', { name: /submit/i })
```

## Sacred Texts & Mystical Sources

{% embed url="https://testing-library.com/docs/react-testing-library/intro/" %}

{% embed url="https://testing-library.com/docs/queries/about/" %}

{% embed url="https://testing-library.com/docs/user-event/intro/" %}
