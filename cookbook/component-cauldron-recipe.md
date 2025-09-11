---
description: >-
  Master the art of building scalable design systems and component libraries with Storybook. This comprehensive recipe covers design tokens, component variants, visual testing, accessibility, and team collaboration workflows for creating maintainable UI components that scale across projects and teams.

theme: "magic"
---

# 🧪 Component Cauldron: Storybook & Design System Recipe

*When your components need to be crafted with precision and shared across realms*

---

## 🎯 Cast This When You Need

**Component Library** • **Design System** • **Team Collaboration** • **Visual Testing** • **Documentation Magic**

Perfect for: Design systems, component libraries, team collaboration, visual regression testing, component documentation

Terrible for: Simple projects, solo development, when you want to move fast without documentation

---

## ⚡ The Design System Summoning

```bash
# Create the component kingdom
npx storybook@latest init

# Or start from scratch with Vite + Storybook
npm create vite@latest my-design-system -- --template react-ts
cd my-design-system
npm install

# Add Storybook superpowers
npx storybook@latest init

# Install design system essentials
npm install -D @storybook/addon-essentials @storybook/addon-docs
npm install -D @storybook/addon-a11y @storybook/addon-viewport
npm install -D @storybook/addon-controls @storybook/addon-actions
npm install -D chromatic @storybook/test-runner

# Design tokens and styling
npm install class-variance-authority clsx tailwind-merge
npm install @radix-ui/react-slot lucide-react

# Start the component laboratory
npm run storybook
```

**Behold!** Your component laboratory opens at `http://localhost:6006`

---

## 🏗️ The Design System Architecture

```
src/
├── components/          # Component library
│   ├── ui/             # Base UI components
│   │   ├── Button/
│   │   ├── Input/
│   │   └── Card/
│   ├── layout/         # Layout components
│   └── feedback/       # Feedback components
├── tokens/             # Design tokens
│   ├── colors.ts
│   ├── typography.ts
│   └── spacing.ts
├── styles/             # Global styles
├── stories/            # Storybook stories
└── tests/              # Component tests
.storybook/             # Storybook configuration
├── main.ts
├── preview.ts
└── manager.ts
```

---

## 🎨 Design Tokens Foundation

### Color System Mastery
```typescript
// src/tokens/colors.ts
export const colors = {
  // Brand colors
  brand: {
    50: '#eff6ff',
    100: '#dbeafe',
    200: '#bfdbfe',
    300: '#93c5fd',
    400: '#60a5fa',
    500: '#3b82f6', // Primary brand color
    600: '#2563eb',
    700: '#1d4ed8',
    800: '#1e40af',
    900: '#1e3a8a',
    950: '#172554',
  },
  
  // Semantic colors
  success: {
    50: '#f0fdf4',
    500: '#22c55e',
    900: '#14532d',
  },
  
  warning: {
    50: '#fffbeb',
    500: '#f59e0b',
    900: '#78350f',
  },
  
  error: {
    50: '#fef2f2',
    500: '#ef4444',
    900: '#7f1d1d',
  },
  
  // Neutral palette
  gray: {
    50: '#f9fafb',
    100: '#f3f4f6',
    200: '#e5e7eb',
    300: '#d1d5db',
    400: '#9ca3af',
    500: '#6b7280',
    600: '#4b5563',
    700: '#374151',
    800: '#1f2937',
    900: '#111827',
    950: '#030712',
  },
} as const

export type ColorScale = keyof typeof colors
export type ColorShade = keyof typeof colors.brand
```

### Typography System
```typescript
// src/tokens/typography.ts
export const typography = {
  fontFamily: {
    sans: ['Inter', 'system-ui', 'sans-serif'],
    mono: ['JetBrains Mono', 'Monaco', 'Consolas', 'monospace'],
  },
  
  fontSize: {
    xs: ['0.75rem', { lineHeight: '1rem' }],
    sm: ['0.875rem', { lineHeight: '1.25rem' }],
    base: ['1rem', { lineHeight: '1.5rem' }],
    lg: ['1.125rem', { lineHeight: '1.75rem' }],
    xl: ['1.25rem', { lineHeight: '1.75rem' }],
    '2xl': ['1.5rem', { lineHeight: '2rem' }],
    '3xl': ['1.875rem', { lineHeight: '2.25rem' }],
    '4xl': ['2.25rem', { lineHeight: '2.5rem' }],
    '5xl': ['3rem', { lineHeight: '1' }],
    '6xl': ['3.75rem', { lineHeight: '1' }],
  },
  
  fontWeight: {
    thin: '100',
    light: '300',
    normal: '400',
    medium: '500',
    semibold: '600',
    bold: '700',
    black: '900',
  },
  
  letterSpacing: {
    tighter: '-0.05em',
    tight: '-0.025em',
    normal: '0em',
    wide: '0.025em',
    wider: '0.05em',
    widest: '0.1em',
  },
} as const
```

### Spacing & Layout Tokens
```typescript
// src/tokens/spacing.ts
export const spacing = {
  0: '0px',
  px: '1px',
  0.5: '0.125rem',
  1: '0.25rem',
  1.5: '0.375rem',
  2: '0.5rem',
  2.5: '0.625rem',
  3: '0.75rem',
  3.5: '0.875rem',
  4: '1rem',
  5: '1.25rem',
  6: '1.5rem',
  7: '1.75rem',
  8: '2rem',
  9: '2.25rem',
  10: '2.5rem',
  11: '2.75rem',
  12: '3rem',
  14: '3.5rem',
  16: '4rem',
  20: '5rem',
  24: '6rem',
  28: '7rem',
  32: '8rem',
  36: '9rem',
  40: '10rem',
  44: '11rem',
  48: '12rem',
  52: '13rem',
  56: '14rem',
  60: '15rem',
  64: '16rem',
  72: '18rem',
  80: '20rem',
  96: '24rem',
} as const

export const borderRadius = {
  none: '0px',
  sm: '0.125rem',
  base: '0.25rem',
  md: '0.375rem',
  lg: '0.5rem',
  xl: '0.75rem',
  '2xl': '1rem',
  '3xl': '1.5rem',
  full: '9999px',
} as const
```

---

## 🧱 Component Building Blocks

### Button Component with Variants
```typescript
// src/components/ui/Button/Button.tsx
import React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { Slot } from '@radix-ui/react-slot'
import { clsx } from 'clsx'

const buttonVariants = cva(
  // Base styles
  'inline-flex items-center justify-center rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50',
  {
    variants: {
      variant: {
        primary: 'bg-brand-500 text-white hover:bg-brand-600 focus-visible:ring-brand-500',
        secondary: 'bg-gray-100 text-gray-900 hover:bg-gray-200 focus-visible:ring-gray-500',
        outline: 'border border-gray-300 bg-transparent text-gray-700 hover:bg-gray-50 focus-visible:ring-gray-500',
        ghost: 'bg-transparent text-gray-700 hover:bg-gray-100 focus-visible:ring-gray-500',
        danger: 'bg-error-500 text-white hover:bg-error-600 focus-visible:ring-error-500',
        success: 'bg-success-500 text-white hover:bg-success-600 focus-visible:ring-success-500',
      },
      size: {
        xs: 'h-7 px-2 text-xs',
        sm: 'h-8 px-3 text-sm',
        md: 'h-10 px-4 text-sm',
        lg: 'h-12 px-6 text-base',
        xl: 'h-14 px-8 text-lg',
      },
      fullWidth: {
        true: 'w-full',
      },
    },
    defaultVariants: {
      variant: 'primary',
      size: 'md',
    },
  }
)

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean
  loading?: boolean
  leftIcon?: React.ReactNode
  rightIcon?: React.ReactNode
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ 
    className, 
    variant, 
    size, 
    fullWidth, 
    asChild = false, 
    loading = false,
    leftIcon,
    rightIcon,
    children,
    disabled,
    ...props 
  }, ref) => {
    const Comp = asChild ? Slot : 'button'
    
    return (
      <Comp
        className={clsx(buttonVariants({ variant, size, fullWidth, className }))}
        ref={ref}
        disabled={disabled || loading}
        {...props}
      >
        {loading && (
          <svg
            className="animate-spin -ml-1 mr-2 h-4 w-4"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              className="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              strokeWidth="4"
            />
            <path
              className="opacity-75"
              fill="currentColor"
              d="m4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
            />
          </svg>
        )}
        {!loading && leftIcon && (
          <span className="mr-2">{leftIcon}</span>
        )}
        {children}
        {!loading && rightIcon && (
          <span className="ml-2">{rightIcon}</span>
        )}
      </Comp>
    )
  }
)

Button.displayName = 'Button'

export { Button, buttonVariants }
```

### Advanced Input Component
```typescript
// src/components/ui/Input/Input.tsx
import React from 'react'
import { cva, type VariantProps } from 'class-variance-authority'
import { clsx } from 'clsx'
import { Eye, EyeOff, AlertCircle, CheckCircle } from 'lucide-react'

const inputVariants = cva(
  'flex w-full rounded-md border bg-white px-3 py-2 text-sm transition-colors file:border-0 file:bg-transparent file:text-sm file:font-medium placeholder:text-gray-500 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:cursor-not-allowed disabled:opacity-50',
  {
    variants: {
      variant: {
        default: 'border-gray-300 focus-visible:ring-brand-500',
        error: 'border-error-500 focus-visible:ring-error-500',
        success: 'border-success-500 focus-visible:ring-success-500',
      },
      size: {
        sm: 'h-8 px-2 text-xs',
        md: 'h-10 px-3 text-sm',
        lg: 'h-12 px-4 text-base',
      },
    },
    defaultVariants: {
      variant: 'default',
      size: 'md',
    },
  }
)

export interface InputProps
  extends React.InputHTMLAttributes<HTMLInputElement>,
    VariantProps<typeof inputVariants> {
  label?: string
  helperText?: string
  error?: string
  success?: string
  leftIcon?: React.ReactNode
  rightIcon?: React.ReactNode
  showPasswordToggle?: boolean
}

const Input = React.forwardRef<HTMLInputElement, InputProps>(
  ({
    className,
    variant,
    size,
    type,
    label,
    helperText,
    error,
    success,
    leftIcon,
    rightIcon,
    showPasswordToggle,
    id,
    ...props
  }, ref) => {
    const [showPassword, setShowPassword] = React.useState(false)
    const [internalType, setInternalType] = React.useState(type)

    React.useEffect(() => {
      if (showPasswordToggle && type === 'password') {
        setInternalType(showPassword ? 'text' : 'password')
      }
    }, [showPassword, showPasswordToggle, type])

    // Determine variant based on validation state
    const inputVariant = error ? 'error' : success ? 'success' : variant

    const inputId = id || `input-${React.useId()}`

    return (
      <div className="w-full">
        {label && (
          <label
            htmlFor={inputId}
            className="block text-sm font-medium text-gray-700 mb-1"
          >
            {label}
          </label>
        )}
        
        <div className="relative">
          {leftIcon && (
            <div className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400">
              {leftIcon}
            </div>
          )}
          
          <input
            type={internalType}
            className={clsx(
              inputVariants({ variant: inputVariant, size, className }),
              leftIcon && 'pl-10',
              (rightIcon || showPasswordToggle || error || success) && 'pr-10'
            )}
            ref={ref}
            id={inputId}
            {...props}
          />
          
          <div className="absolute right-3 top-1/2 transform -translate-y-1/2 flex items-center space-x-1">
            {error && (
              <AlertCircle className="h-4 w-4 text-error-500" />
            )}
            {success && !error && (
              <CheckCircle className="h-4 w-4 text-success-500" />
            )}
            {showPasswordToggle && type === 'password' && (
              <button
                type="button"
                onClick={() => setShowPassword(!showPassword)}
                className="text-gray-400 hover:text-gray-600 focus:outline-none"
              >
                {showPassword ? (
                  <EyeOff className="h-4 w-4" />
                ) : (
                  <Eye className="h-4 w-4" />
                )}
              </button>
            )}
            {rightIcon && !error && !success && !showPasswordToggle && (
              <div className="text-gray-400">{rightIcon}</div>
            )}
          </div>
        </div>
        
        {(error || success || helperText) && (
          <div className="mt-1 text-xs">
            {error && (
              <p className="text-error-500">{error}</p>
            )}
            {success && !error && (
              <p className="text-success-500">{success}</p>
            )}
            {helperText && !error && !success && (
              <p className="text-gray-500">{helperText}</p>
            )}
          </div>
        )}
      </div>
    )
  }
)

Input.displayName = 'Input'

export { Input, inputVariants }
```

---

## 📚 Storybook Stories Mastery

### Button Stories with All Variants
```typescript
// src/stories/Button.stories.tsx
import type { Meta, StoryObj } from '@storybook/react'
import { fn } from '@storybook/test'
import { Button } from '../components/ui/Button/Button'
import { Download, Plus, ArrowRight } from 'lucide-react'

const meta = {
  title: 'UI/Button',
  component: Button,
  parameters: {
    layout: 'centered',
    docs: {
      description: {
        component: 'A versatile button component with multiple variants, sizes, and states. Built with accessibility and keyboard navigation in mind.',
      },
    },
  },
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'outline', 'ghost', 'danger', 'success'],
      description: 'The visual variant of the button',
    },
    size: {
      control: 'select',
      options: ['xs', 'sm', 'md', 'lg', 'xl'],
      description: 'The size of the button',
    },
    fullWidth: {
      control: 'boolean',
      description: 'Whether the button should take full width',
    },
    loading: {
      control: 'boolean',
      description: 'Show loading spinner and disable button',
    },
    disabled: {
      control: 'boolean',
      description: 'Disable the button',
    },
  },
  args: {
    onClick: fn(),
    children: 'Button',
  },
} satisfies Meta<typeof Button>

export default meta
type Story = StoryObj<typeof meta>

// Basic variants
export const Primary: Story = {
  args: {
    variant: 'primary',
    children: 'Primary Button',
  },
}

export const Secondary: Story = {
  args: {
    variant: 'secondary',
    children: 'Secondary Button',
  },
}

export const Outline: Story = {
  args: {
    variant: 'outline',
    children: 'Outline Button',
  },
}

// With icons
export const WithLeftIcon: Story = {
  args: {
    leftIcon: <Plus className="h-4 w-4" />,
    children: 'Add Item',
  },
}

export const WithRightIcon: Story = {
  args: {
    rightIcon: <ArrowRight className="h-4 w-4" />,
    children: 'Continue',
  },
}

export const IconOnly: Story = {
  args: {
    children: <Download className="h-4 w-4" />,
    size: 'md',
    'aria-label': 'Download',
  },
}

// States
export const Loading: Story = {
  args: {
    loading: true,
    children: 'Loading...',
  },
}

export const Disabled: Story = {
  args: {
    disabled: true,
    children: 'Disabled Button',
  },
}

// Sizes showcase
export const AllSizes: Story = {
  render: () => (
    <div className="flex items-center gap-4">
      <Button size="xs">Extra Small</Button>
      <Button size="sm">Small</Button>
      <Button size="md">Medium</Button>
      <Button size="lg">Large</Button>
      <Button size="xl">Extra Large</Button>
    </div>
  ),
  parameters: {
    docs: {
      description: {
        story: 'All available button sizes displayed together for comparison.',
      },
    },
  },
}

// Variants showcase
export const AllVariants: Story = {
  render: () => (
    <div className="flex flex-wrap gap-4">
      <Button variant="primary">Primary</Button>
      <Button variant="secondary">Secondary</Button>
      <Button variant="outline">Outline</Button>
      <Button variant="ghost">Ghost</Button>
      <Button variant="danger">Danger</Button>
      <Button variant="success">Success</Button>
    </div>
  ),
  parameters: {
    docs: {
      description: {
        story: 'All available button variants displayed together.',
      },
    },
  },
}

// Real-world examples
export const LoginForm: Story = {
  render: () => (
    <div className="w-80 space-y-4">
      <Button variant="primary" fullWidth>
        Sign In
      </Button>
      <Button variant="outline" fullWidth>
        Sign In with Google
      </Button>
      <Button variant="ghost" fullWidth>
        Create Account
      </Button>
    </div>
  ),
  parameters: {
    docs: {
      description: {
        story: 'Example usage in a login form context.',
      },
    },
  },
}
```

### Input Stories with Validation States
```typescript
// src/stories/Input.stories.tsx
import type { Meta, StoryObj } from '@storybook/react'
import { Input } from '../components/ui/Input/Input'
import { Search, Mail, Lock, User } from 'lucide-react'

const meta = {
  title: 'UI/Input',
  component: Input,
  parameters: {
    layout: 'centered',
    docs: {
      description: {
        component: 'A flexible input component with validation states, icons, and helper text support.',
      },
    },
  },
  tags: ['autodocs'],
  argTypes: {
    variant: {
      control: 'select',
      options: ['default', 'error', 'success'],
    },
    size: {
      control: 'select',
      options: ['sm', 'md', 'lg'],
    },
    type: {
      control: 'select',
      options: ['text', 'email', 'password', 'number', 'tel', 'url'],
    },
  },
} satisfies Meta<typeof Input>

export default meta
type Story = StoryObj<typeof meta>

export const Default: Story = {
  args: {
    placeholder: 'Enter text...',
  },
}

export const WithLabel: Story = {
  args: {
    label: 'Email Address',
    placeholder: 'john@example.com',
    type: 'email',
  },
}

export const WithHelperText: Story = {
  args: {
    label: 'Username',
    placeholder: 'Enter username',
    helperText: 'Must be at least 3 characters long',
  },
}

export const WithLeftIcon: Story = {
  args: {
    label: 'Search',
    placeholder: 'Search users...',
    leftIcon: <Search className="h-4 w-4" />,
  },
}

export const WithError: Story = {
  args: {
    label: 'Email',
    placeholder: 'john@example.com',
    type: 'email',
    error: 'Please enter a valid email address',
    leftIcon: <Mail className="h-4 w-4" />,
  },
}

export const WithSuccess: Story = {
  args: {
    label: 'Username',
    placeholder: 'Enter username',
    success: 'Username is available!',
    leftIcon: <User className="h-4 w-4" />,
  },
}

export const PasswordWithToggle: Story = {
  args: {
    label: 'Password',
    type: 'password',
    placeholder: 'Enter password',
    showPasswordToggle: true,
    leftIcon: <Lock className="h-4 w-4" />,
  },
}

// Form example
export const LoginForm: Story = {
  render: () => (
    <div className="w-80 space-y-4">
      <Input
        label="Email"
        type="email"
        placeholder="john@example.com"
        leftIcon={<Mail className="h-4 w-4" />}
      />
      <Input
        label="Password"
        type="password"
        placeholder="Enter password"
        showPasswordToggle
        leftIcon={<Lock className="h-4 w-4" />}
      />
      <Input
        label="Confirm Password"
        type="password"
        placeholder="Confirm password"
        error="Passwords do not match"
        leftIcon={<Lock className="h-4 w-4" />}
      />
    </div>
  ),
  parameters: {
    docs: {
      description: {
        story: 'Example login form showing various input states.',
      },
    },
  },
}
```

---

## 🎭 Advanced Storybook Configuration

### Enhanced Storybook Setup
```typescript
// .storybook/main.ts
import type { StorybookConfig } from '@storybook/react-vite'

const config: StorybookConfig = {
  stories: ['../src/**/*.stories.@(js|jsx|mjs|ts|tsx)'],
  addons: [
    '@storybook/addon-essentials',
    '@storybook/addon-docs',
    '@storybook/addon-controls',
    '@storybook/addon-actions',
    '@storybook/addon-viewport',
    '@storybook/addon-a11y',
    '@storybook/addon-measure',
    '@storybook/addon-outline',
    {
      name: '@storybook/addon-styling-webpack',
      options: {
        rules: [
          {
            test: /\.css$/,
            sideEffects: true,
            use: [
              require.resolve('style-loader'),
              {
                loader: require.resolve('css-loader'),
                options: {
                  importLoaders: 1,
                },
              },
              {
                loader: require.resolve('postcss-loader'),
                options: {
                  implementation: require.resolve('postcss'),
                },
              },
            ],
          },
        ],
      },
    },
  ],
  framework: {
    name: '@storybook/react-vite',
    options: {},
  },
  docs: {
    autodocs: 'tag',
    defaultName: 'Documentation',
  },
  typescript: {
    check: false,
    reactDocgen: 'react-docgen-typescript',
    reactDocgenTypescriptOptions: {
      shouldExtractLiteralValuesFromEnum: true,
      propFilter: (prop) => {
        if (prop.parent) {
          return !prop.parent.fileName.includes('node_modules')
        }
        return true
      },
    },
  },
}

export default config
```

### Global Theming & Parameters
```typescript
// .storybook/preview.ts
import type { Preview } from '@storybook/react'
import '../src/styles/globals.css'

const preview: Preview = {
  parameters: {
    actions: { argTypesRegex: '^on[A-Z].*' },
    controls: {
      matchers: {
        color: /(background|color)$/i,
        date: /Date$/i,
      },
    },
    docs: {
      toc: true,
    },
    viewport: {
      viewports: {
        mobile: {
          name: 'Mobile',
          styles: {
            width: '375px',
            height: '667px',
          },
        },
        tablet: {
          name: 'Tablet',
          styles: {
            width: '768px',
            height: '1024px',
          },
        },
        desktop: {
          name: 'Desktop',
          styles: {
            width: '1200px',
            height: '800px',
          },
        },
      },
    },
    backgrounds: {
      default: 'light',
      values: [
        {
          name: 'light',
          value: '#ffffff',
        },
        {
          name: 'dark',
          value: '#1a1a1a',
        },
        {
          name: 'gray',
          value: '#f8f9fa',
        },
      ],
    },
    layout: 'centered',
  },
  globalTypes: {
    theme: {
      description: 'Global theme for components',
      defaultValue: 'light',
      toolbar: {
        title: 'Theme',
        icon: 'circlehollow',
        items: [
          { value: 'light', icon: 'circlehollow', title: 'Light' },
          { value: 'dark', icon: 'circle', title: 'Dark' },
        ],
      },
    },
  },
  decorators: [
    (Story, context) => {
      const theme = context.globals.theme || 'light'
      return (
        <div className={theme === 'dark' ? 'dark' : ''} style={{ minHeight: '100vh' }}>
          <div className="bg-white dark:bg-gray-900 text-gray-900 dark:text-white p-4">
            <Story />
          </div>
        </div>
      )
    },
  ],
}

export default preview
```

---

## 🧪 Visual Testing & Quality Assurance

### Chromatic Integration
```bash
# Install Chromatic for visual testing
npm install -D chromatic

# Add to package.json scripts
{
  "scripts": {
    "chromatic": "npx chromatic --project-token=PROJECT_TOKEN",
    "chromatic:ci": "npx chromatic --exit-zero-on-changes"
  }
}
```

### Accessibility Testing Setup
```typescript
// src/tests/accessibility.test.tsx
import { render } from '@testing-library/react'
import { axe, toHaveNoViolations } from 'jest-axe'
import { Button } from '../components/ui/Button/Button'
import { Input } from '../components/ui/Input/Input'

expect.extend(toHaveNoViolations)

describe('Accessibility Tests', () => {
  test('Button should not have accessibility violations', async () => {
    const { container } = render(<Button>Test Button</Button>)
    const results = await axe(container)
    expect(results).toHaveNoViolations()
  })

  test('Input should not have accessibility violations', async () => {
    const { container } = render(
      <Input 
        label="Test Input" 
        placeholder="Enter text" 
        id="test-input"
      />
    )
    const results = await axe(container)
    expect(results).toHaveNoViolations()
  })

  test('Button with only icon should have aria-label', async () => {
    const { container } = render(
      <Button aria-label="Download">
        <svg>...</svg>
      </Button>
    )
    const results = await axe(container)
    expect(results).toHaveNoViolations()
  })
})
```

### Component Testing with Storybook
```typescript
// src/tests/storybook.test.tsx
import { composeStories } from '@storybook/react'
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import * as ButtonStories from '../stories/Button.stories'

const { Primary, Loading, WithLeftIcon } = composeStories(ButtonStories)

describe('Button Storybook Tests', () => {
  test('Primary button renders correctly', () => {
    render(<Primary />)
    expect(screen.getByRole('button')).toBeInTheDocument()
    expect(screen.getByRole('button')).toHaveTextContent('Primary Button')
    expect(screen.getByRole('button')).toHaveClass('bg-brand-500')
  })

  test('Loading button shows spinner and is disabled', () => {
    render(<Loading />)
    const button = screen.getByRole('button')

    expect(button).toBeDisabled()
    expect(button).toHaveTextContent('Loading...')
    expect(screen.getByRole('button')).toContainHTML('svg')
  })

  test('Button with left icon renders icon correctly', () => {
    render(<WithLeftIcon />)
    const button = screen.getByRole('button')

    expect(button).toHaveTextContent('Add Item')
    expect(button.querySelector('svg')).toBeInTheDocument()
  })

  test('Button handles click events', async () => {
    const user = userEvent.setup()
    render(<Primary />)
    const button = screen.getByRole('button')

    await user.click(button)
    // Verify the onClick handler was called (if using jest.fn())
    expect(Primary.args?.onClick).toHaveBeenCalledTimes(1)
  })

  test('Button supports keyboard navigation', async () => {
    const user = userEvent.setup()
    render(<Primary />)
    const button = screen.getByRole('button')

    button.focus()
    expect(button).toHaveFocus()

    await user.keyboard('{Enter}')
    expect(Primary.args?.onClick).toHaveBeenCalled()
  })
})
```

### Visual Regression Testing
```typescript
// src/tests/visual-regression.test.tsx
import { test, expect } from '@playwright/test'

test.describe('Button Visual Tests', () => {
  test('Button variants look correct', async ({ page }) => {
    await page.goto('/iframe.html?id=ui-button--all-variants')

    // Wait for fonts to load
    await page.waitForLoadState('networkidle')

    // Take screenshot of all button variants
    await expect(page.locator('[data-testid="all-variants"]')).toHaveScreenshot('button-variants.png')
  })

  test('Button states look correct', async ({ page }) => {
    await page.goto('/iframe.html?id=ui-button--primary')

    const button = page.locator('button')

    // Default state
    await expect(button).toHaveScreenshot('button-default.png')

    // Hover state
    await button.hover()
    await expect(button).toHaveScreenshot('button-hover.png')

    // Focus state
    await button.focus()
    await expect(button).toHaveScreenshot('button-focus.png')
  })

  test('Button sizes are consistent', async ({ page }) => {
    await page.goto('/iframe.html?id=ui-button--all-sizes')

    await expect(page.locator('[data-testid="all-sizes"]')).toHaveScreenshot('button-sizes.png')
  })
})
```

### Integration Testing with MSW
```typescript
// src/tests/integration.test.tsx
import { render, screen, waitFor } from '@testing-library/react'
import userEvent from '@testing-library/user-event'
import { rest } from 'msw'
import { setupServer } from 'msw/node'
import { NewsletterSignup } from '../components/NewsletterSignup'

const server = setupServer(
  rest.post('/api/newsletter', (req, res, ctx) => {
    return res(ctx.json({ success: true }))
  })
)

beforeAll(() => server.listen())
afterEach(() => server.resetHandlers())
afterAll(() => server.close())

describe('Newsletter Signup Integration', () => {
  test('successfully submits newsletter signup', async () => {
    const user = userEvent.setup()
    render(<NewsletterSignup />)

    const emailInput = screen.getByPlaceholderText('Enter your email')
    const submitButton = screen.getByRole('button', { name: /subscribe/i })

    await user.type(emailInput, 'test@example.com')
    await user.click(submitButton)

    await waitFor(() => {
      expect(screen.getByText('✅ Successfully subscribed!')).toBeInTheDocument()
    })
  })

  test('handles API errors gracefully', async () => {
    server.use(
      rest.post('/api/newsletter', (req, res, ctx) => {
        return res(ctx.status(500), ctx.json({ error: 'Server error' }))
      })
    )

    const user = userEvent.setup()
    render(<NewsletterSignup />)

    const emailInput = screen.getByPlaceholderText('Enter your email')
    const submitButton = screen.getByRole('button', { name: /subscribe/i })

    await user.type(emailInput, 'test@example.com')
    await user.click(submitButton)

    await waitFor(() => {
      expect(screen.getByText('❌ Something went wrong. Try again.')).toBeInTheDocument()
    })
  })
})
```