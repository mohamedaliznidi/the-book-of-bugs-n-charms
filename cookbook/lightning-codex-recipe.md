---
description: >-
  Achieve maximum development speed with React and Vite. This recipe focuses on rapid prototyping, instant hot reload, modern bundling, TypeScript integration, testing with Vitest, and lightning-fast deployment strategies for building React applications at unprecedented speed.

theme: "magic"
---

# ⚡ Lightning Codex: React + Vite Speed Ritual

*For when you need to summon React apps at the speed of lightning*

---

## 🎯 Cast This When You Need

**Maximum Dev Speed** • **Instant Hot Reload** • **Modern Bundle Magic** • **TypeScript Power**

Perfect for: Prototypes, SPAs, component playgrounds, startup MVPs, learning React patterns

Terrible for: SSR needs, heavy SEO requirements, static content sites

---

## ⚡ The 60-Second Summoning

```bash
# One spell to rule them all
npm create vite@latest my-lightning-app -- --template react-ts && cd my-lightning-app && npm i && npm run dev
```

**BOOM!** Your dev server is running on `http://localhost:5173` with HMR faster than you can say "webpack"

---

## 🧪 The Secret Sauce Setup

### Step 1: Supercharge Your Vite Config

```javascript
// vite.config.ts - Your speed enhancement potion
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@hooks': path.resolve(__dirname, './src/hooks'),
      '@utils': path.resolve(__dirname, './src/utils'),
    }
  },
  server: {
    port: 3000,
    open: true, // Auto-open browser like magic
  }
})
```

### Step 2: The Quality Control Enchantment

Create `.eslintrc.cjs` for instant code quality:

```javascript
module.exports = {
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
    'plugin:react-hooks/recommended',
  ],
  rules: {
    'react-hooks/exhaustive-deps': 'warn',
    '@typescript-eslint/no-unused-vars': ['error', { argsIgnorePattern: '^_' }],
  }
}
```

### Step 3: The One-Command Test Setup

```bash
# Install the testing trinity
npm i -D vitest @testing-library/react @testing-library/jest-dom jsdom
```

Add to `package.json`:
```json
{
  "scripts": {
    "test": "vitest",
    "test:ui": "vitest --ui"
  }
}
```

---

## 🎨 Styling Superpowers

### Option A: Tailwind CSS (Utility-First Magic)
```bash
npm i -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### Option B: CSS Modules (Scoped Styling Sorcery)
```css
/* Button.module.css */
.magicalButton {
  @apply px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600 transition-colors;
}
```

### Option C: Styled Components (CSS-in-JS Wizardry)
```bash
npm i styled-components
npm i -D @types/styled-components
```

---

## 🏗️ The Perfect Project Structure

```
src/
├── components/          # Reusable UI spells
│   ├── ui/             # Base components (Button, Input, etc.)
│   └── features/       # Feature-specific components
├── hooks/              # Custom React hooks
├── utils/              # Pure utility functions
├── types/              # TypeScript type definitions
├── assets/             # Images, icons, fonts
└── __tests__/          # Test files
```

---

## 🚀 Power-User Productivity Hacks

### Instant Component Generation
```bash
# Create this script: scripts/component.js
const fs = require('fs')
const componentName = process.argv[2]

const template = `import React from 'react'

interface ${componentName}Props {
  // Define your props here
}

const ${componentName}: React.FC<${componentName}Props> = () => {
  return (
    <div>
      <h1>${componentName} works!</h1>
    </div>
  )
}

export default ${componentName}`

fs.mkdirSync(`src/components/${componentName}`, { recursive: true })
fs.writeFileSync(`src/components/${componentName}/${componentName}.tsx`, template)
console.log(`✨ Component ${componentName} created!`)
```

Run with: `node scripts/component.js MyNewComponent`

### Environment Variables Magic
```bash
# .env.local
VITE_API_URL=http://localhost:8000/api
VITE_APP_TITLE=My Lightning App
```

```typescript
// src/config.ts
export const config = {
  apiUrl: import.meta.env.VITE_API_URL,
  appTitle: import.meta.env.VITE_APP_TITLE,
} as const
```

---

## 🧪 Advanced Patterns & Tricks

### Smart Error Boundaries
```typescript
// src/components/ErrorBoundary.tsx
import React from 'react'

class ErrorBoundary extends React.Component<
  { children: React.ReactNode },
  { hasError: boolean }
> {
  constructor(props: any) {
    super(props)
    this.state = { hasError: false }
  }

  static getDerivedStateFromError() {
    return { hasError: true }
  }

  render() {
    if (this.state.hasError) {
      return (
        <div className="p-8 text-center">
          <h2>🔥 Something went wrong!</h2>
          <button 
            onClick={() => this.setState({ hasError: false })}
            className="mt-4 px-4 py-2 bg-blue-500 text-white rounded"
          >
            Try again
          </button>
        </div>
      )
    }

    return this.props.children
  }
}
```

### Custom Hook Factory
```typescript
// src/hooks/useLocalStorage.ts
import { useState, useEffect } from 'react'

export function useLocalStorage<T>(key: string, defaultValue: T) {
  const [value, setValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key)
      return item ? JSON.parse(item) : defaultValue
    } catch {
      return defaultValue
    }
  })

  useEffect(() => {
    try {
      window.localStorage.setItem(key, JSON.stringify(value))
    } catch (error) {
      console.error(`Error saving to localStorage:`, error)
    }
  }, [key, value])

  return [value, setValue] as const
}
```

---

## 🐛 Common Lightning Bugs & Fixes

### Bug: "Process is not defined"
**Fix:** Add to `vite.config.ts`:
```javascript
define: {
  global: 'globalThis',
}
```

### Bug: Slow TypeScript checking
**Fix:** Use `vite-plugin-checker`:
```bash
npm i -D vite-plugin-checker
```

### Bug: Import alias not working in tests
**Fix:** Update `vitest.config.ts`:
```javascript
import { defineConfig } from 'vitest/config'

export default defineConfig({
  test: {
    alias: {
      '@': path.resolve(__dirname, './src')
    }
  }
})
```

---

## 🚀 Deployment Speed Runs

### Vercel (Fastest)
```bash
npm i -g vercel
vercel --prod
```

### Netlify (Drag & Drop)
1. `npm run build`
2. Drag `dist/` folder to Netlify
3. ✨ Done!

### GitHub Pages (Free Forever)
```bash
npm i -D gh-pages
```
Add to `package.json`:
```json
{
  "homepage": "https://yourusername.github.io/your-repo",
  "scripts": {
    "deploy": "npm run build && gh-pages -d dist"
  }
}
```

---

## 🎭 Pro Tips from the Trenches

1. **Use `React.memo()` for expensive components** - Wrap components that receive complex props
2. **Leverage Vite's `import.meta.glob()`** - Dynamically import multiple files
3. **Create a `useDebounce` hook** - Essential for search inputs and API calls
4. **Use `ErrorBoundary` + `Suspense`** - Better user experience during loading/errors
5. **Keep bundle size under 200KB gzipped** - Use `npm run build -- --analyze` to check

