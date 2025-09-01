---
description: >-
  Complete guide to Vite configuration and optimization for modern frontend
  development. Learn advanced build configurations, performance optimization,
  plugin ecosystem, and integration with React, TypeScript, and modern tools.
---

# Vite

## Introduction

Vite is a modern build tool that provides fast development server startup and optimized production builds. It leverages native ES modules during development and uses Rollup for production bundling, offering superior performance compared to traditional bundlers like Webpack.

## Use Cases

1. **React Applications**: Fast development with instant HMR and optimized builds
2. **TypeScript Projects**: Built-in TypeScript support without additional configuration
3. **Multi-Framework Development**: Support for React, Vue, Svelte, and vanilla JavaScript
4. **Library Development**: Optimized builds for npm packages and component libraries

## Installation and Setup

### 1. Create New Vite Project

```bash
# Create React + TypeScript project
pnpm create vite@latest my-react-app --template react-ts

# Navigate to project
cd my-react-app

# Install dependencies
pnpm install

# Start development server
pnpm dev
```

### 2. Add Vite to Existing Project

```bash
# Install Vite and plugins
pnpm add -D vite @vitejs/plugin-react

# For TypeScript support
pnpm add -D typescript @types/react @types/react-dom

# Create vite.config.ts
touch vite.config.ts
```

## Basic Configuration

### 1. Essential Vite Config

```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import path from 'path'

export default defineConfig({
  plugins: [react()],
  
  // Path resolution
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
      '@components': path.resolve(__dirname, './src/components'),
      '@utils': path.resolve(__dirname, './src/utils'),
      '@types': path.resolve(__dirname, './src/types'),
    },
  },
  
  // Development server configuration
  server: {
    port: 3000,
    open: true,
    host: true, // Allow external connections
    cors: true,
  },
  
  // Build configuration
  build: {
    outDir: 'dist',
    sourcemap: true,
    minify: 'terser',
    target: 'esnext',
  },
  
  // Environment variables
  define: {
    __APP_VERSION__: JSON.stringify(process.env.npm_package_version),
  },
})
```

### 2. TypeScript Integration

```typescript
// vite.config.ts with TypeScript optimizations
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { resolve } from 'path'

export default defineConfig({
  plugins: [
    react({
      // Enable React Fast Refresh
      fastRefresh: true,
      // TypeScript decorators support
      babel: {
        plugins: [
          ['@babel/plugin-proposal-decorators', { legacy: true }],
        ],
      },
    }),
  ],
  
  resolve: {
    alias: {
      '@': resolve(__dirname, 'src'),
    },
  },
  
  // TypeScript configuration
  esbuild: {
    target: 'esnext',
    format: 'esm',
  },
  
  build: {
    target: 'esnext',
    lib: {
      entry: resolve(__dirname, 'src/main.tsx'),
      formats: ['es'],
    },
  },
})
```

## Advanced Configuration

### 1. Environment-Specific Configuration

```typescript
// vite.config.ts
import { defineConfig, loadEnv } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig(({ command, mode }) => {
  // Load environment variables
  const env = loadEnv(mode, process.cwd(), '')
  
  return {
    plugins: [react()],
    
    // Conditional configuration based on command
    define: {
      __IS_DEV__: command === 'serve',
      __API_URL__: JSON.stringify(env.VITE_API_URL),
    },
    
    server: {
      // Development-only configuration
      ...(command === 'serve' && {
        proxy: {
          '/api': {
            target: env.VITE_API_URL || 'http://localhost:8000',
            changeOrigin: true,
            rewrite: (path) => path.replace(/^\/api/, ''),
          },
        },
      }),
    },
    
    build: {
      // Production optimizations
      ...(command === 'build' && {
        rollupOptions: {
          output: {
            manualChunks: {
              vendor: ['react', 'react-dom'],
              ui: ['@radix-ui/react-dialog', '@radix-ui/react-dropdown-menu'],
            },
          },
        },
      }),
    },
  }
})
```

### 2. Plugin Ecosystem Integration

```typescript
// vite.config.ts with popular plugins
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { resolve } from 'path'

// Additional plugins
import { visualizer } from 'rollup-plugin-visualizer'
import { defineConfig as defineVitestConfig } from 'vitest/config'

export default defineConfig({
  plugins: [
    react(),
    
    // Bundle analyzer
    visualizer({
      filename: 'dist/stats.html',
      open: true,
      gzipSize: true,
    }),
  ],
  
  resolve: {
    alias: {
      '@': resolve(__dirname, 'src'),
    },
  },
  
  // CSS configuration
  css: {
    modules: {
      localsConvention: 'camelCaseOnly',
    },
    preprocessorOptions: {
      scss: {
        additionalData: `@import "@/styles/variables.scss";`,
      },
    },
  },
  
  // Testing configuration (Vitest)
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['./src/test/setup.ts'],
  },
})
```

## Performance Optimization

### 1. Code Splitting and Lazy Loading

```typescript
// vite.config.ts - Advanced code splitting
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  
  build: {
    rollupOptions: {
      output: {
        manualChunks: (id) => {
          // Vendor chunks
          if (id.includes('node_modules')) {
            if (id.includes('react') || id.includes('react-dom')) {
              return 'react-vendor'
            }
            if (id.includes('@radix-ui')) {
              return 'ui-vendor'
            }
            if (id.includes('date-fns') || id.includes('lodash')) {
              return 'utils-vendor'
            }
            return 'vendor'
          }
          
          // Feature-based chunks
          if (id.includes('/src/features/auth/')) {
            return 'auth'
          }
          if (id.includes('/src/features/dashboard/')) {
            return 'dashboard'
          }
          if (id.includes('/src/components/ui/')) {
            return 'ui-components'
          }
        },
      },
    },
    
    // Chunk size warnings
    chunkSizeWarningLimit: 1000,
  },
})
```

### 2. Asset Optimization

```typescript
// vite.config.ts - Asset optimization
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  
  // Asset handling
  assetsInclude: ['**/*.woff2', '**/*.woff'],
  
  build: {
    // Asset optimization
    assetsDir: 'assets',
    assetsInlineLimit: 4096, // 4kb
    
    rollupOptions: {
      output: {
        assetFileNames: (assetInfo) => {
          const info = assetInfo.name.split('.')
          const ext = info[info.length - 1]
          
          if (/png|jpe?g|svg|gif|tiff|bmp|ico/i.test(ext)) {
            return `images/[name]-[hash][extname]`
          }
          if (/woff2?|eot|ttf|otf/i.test(ext)) {
            return `fonts/[name]-[hash][extname]`
          }
          return `assets/[name]-[hash][extname]`
        },
        chunkFileNames: 'js/[name]-[hash].js',
        entryFileNames: 'js/[name]-[hash].js',
      },
    },
  },
})
```

## Development Experience

### 1. Hot Module Replacement (HMR)

```typescript
// src/main.tsx - HMR setup
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'

const root = ReactDOM.createRoot(document.getElementById('root')!)

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)

// Hot Module Replacement
if (import.meta.hot) {
  import.meta.hot.accept('./App', (newApp) => {
    root.render(
      <React.StrictMode>
        <newApp.default />
      </React.StrictMode>
    )
  })
}
```

### 2. Environment Variables

```typescript
// vite-env.d.ts - Environment variable types
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_API_URL: string
  readonly VITE_APP_TITLE: string
  readonly VITE_ENABLE_ANALYTICS: string
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}
```

```bash
# .env.local
VITE_API_URL=http://localhost:3001/api
VITE_APP_TITLE=My Vite App
VITE_ENABLE_ANALYTICS=true
```

```typescript
// Using environment variables
const apiUrl = import.meta.env.VITE_API_URL
const appTitle = import.meta.env.VITE_APP_TITLE
const analyticsEnabled = import.meta.env.VITE_ENABLE_ANALYTICS === 'true'

// Development vs production
if (import.meta.env.DEV) {
  console.log('Development mode')
}

if (import.meta.env.PROD) {
  console.log('Production mode')
}
```

## Integration Patterns

### 1. Vite with Tailwind CSS

```bash
# Install Tailwind CSS
pnpm add -D tailwindcss postcss autoprefixer
pnpm dlx tailwindcss init -p
```

```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  css: {
    postcss: {
      plugins: [
        require('tailwindcss'),
        require('autoprefixer'),
      ],
    },
  },
})
```

### 2. Vite with PWA

```bash
# Install PWA plugin
pnpm add -D vite-plugin-pwa
```

```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { VitePWA } from 'vite-plugin-pwa'

export default defineConfig({
  plugins: [
    react(),
    VitePWA({
      registerType: 'autoUpdate',
      workbox: {
        globPatterns: ['**/*.{js,css,html,ico,png,svg}'],
      },
      manifest: {
        name: 'My Vite App',
        short_name: 'ViteApp',
        description: 'My Awesome Vite App',
        theme_color: '#ffffff',
        icons: [
          {
            src: 'pwa-192x192.png',
            sizes: '192x192',
            type: 'image/png',
          },
          {
            src: 'pwa-512x512.png',
            sizes: '512x512',
            type: 'image/png',
          },
        ],
      },
    }),
  ],
})
```

## Library Development

### 1. Building Libraries with Vite

```typescript
// vite.config.ts for library
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { resolve } from 'path'
import dts from 'vite-plugin-dts'

export default defineConfig({
  plugins: [
    react(),
    dts({
      insertTypesEntry: true,
    }),
  ],
  
  build: {
    lib: {
      entry: resolve(__dirname, 'src/index.ts'),
      name: 'MyLibrary',
      formats: ['es', 'umd'],
      fileName: (format) => `my-library.${format}.js`,
    },
    rollupOptions: {
      external: ['react', 'react-dom'],
      output: {
        globals: {
          react: 'React',
          'react-dom': 'ReactDOM',
        },
      },
    },
  },
})
```

### 2. Package.json for Library

```json
{
  "name": "my-vite-library",
  "version": "1.0.0",
  "type": "module",
  "files": [
    "dist"
  ],
  "main": "./dist/my-library.umd.js",
  "module": "./dist/my-library.es.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "import": "./dist/my-library.es.js",
      "require": "./dist/my-library.umd.js"
    }
  },
  "scripts": {
    "build": "tsc && vite build",
    "build:watch": "vite build --watch"
  }
}
```

## Best Practices

- **Fast Development**: Leverage Vite's instant server start and HMR for rapid development
- **Code Splitting**: Use dynamic imports and manual chunks for optimal bundle sizes
- **Environment Variables**: Use VITE_ prefix for client-side environment variables
- **Asset Optimization**: Configure asset handling for optimal loading performance
- **Plugin Ecosystem**: Leverage Vite's rich plugin ecosystem for additional functionality
- **TypeScript Integration**: Use built-in TypeScript support for type-safe development

## Common Pitfalls

### Issue 1: Environment Variable Access
Environment variables must be prefixed with VITE_ to be accessible in client code.

**Solution:**
```typescript
// ❌ Wrong - not accessible
const apiUrl = import.meta.env.API_URL

// ✅ Correct - accessible with VITE_ prefix
const apiUrl = import.meta.env.VITE_API_URL
```

### Issue 2: Import Path Issues
Absolute imports may not work without proper alias configuration.

**Solution:**
```typescript
// vite.config.ts
export default defineConfig({
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
})

// tsconfig.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

### Issue 3: Build Size Issues
Large bundle sizes without proper code splitting.

**Solution:**
```typescript
// Use dynamic imports for code splitting
const LazyComponent = lazy(() => import('./LazyComponent'))

// Configure manual chunks in vite.config.ts
build: {
  rollupOptions: {
    output: {
      manualChunks: {
        vendor: ['react', 'react-dom'],
      },
    },
  },
}
```

## References

{% embed url="https://vitejs.dev/guide/" %}

{% embed url="https://vitejs.dev/config/" %}

{% embed url="https://github.com/vitejs/awesome-vite" %}
