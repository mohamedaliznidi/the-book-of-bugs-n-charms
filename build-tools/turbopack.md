---
description: >-
  Complete grimoire for Turbopack Accelerated Build Sorcery, Vercel's next-generation
  mystical bundler for Next.js. Master setup enchantments, configuration spells,
  performance optimization magic, and migration rituals for ultra-fast development
  and build time acceleration.
---

# Turbopack Accelerated Build Sorcery

## The Ancient Knowledge

Turbopack is Vercel's next-generation mystical bundler built in Rust, designed to replace Webpack in Next.js applications through advanced magical compilation. It provides significantly faster build times, improved development experience, and better performance through incremental compilation sorcery and advanced caching enchantments.

## When to Cast These Spells

1. **Next.js Application Enchantments**: Ultra-fast development server and optimized production build magic
2. **Large Mystical Codebases**: Improved build performance for applications with thousands of mystical modules
3. **Development Experience Enhancement**: Instant updates and faster Hot Module Replacement (HMR) magic
4. **Monorepo Project Sorcery**: Efficient handling of complex mystical project structures

## Setup and Mystical Configuration

### 1. Enable Turbopack Sorcery in Next.js

```bash
# Create new Next.js project with Turbopack magic
pnpm create next-app@latest my-turbo-app --typescript --tailwind --eslint --app

# Navigate to mystical project
cd my-turbo-app

# Enable Turbopack sorcery in development
pnpm dev --turbo
```

### 2. Package.json Mystical Scripts

```json
{
  "scripts": {
    "dev": "next dev --turbo",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  }
}
```

### 3. Next.js Mystical Configuration

```javascript
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    turbo: {
      // Turbopack mystical configuration options
      rules: {
        // Custom loader enchantment rules
        '*.svg': {
          loaders: ['@svgr/webpack'],
          as: '*.js',
        },
      },
      resolveAlias: {
        // Mystical path aliases
        '@': './src',
        '@components': './src/components',
        '@utils': './src/utils',
      },
    },
  },
}

module.exports = nextConfig
```

## Advanced Mystical Configuration

### 1. Custom Loader Enchantments and Rules

```javascript
// next.config.js
const nextConfig = {
  experimental: {
    turbo: {
      rules: {
        // SVG handling
        '*.svg': {
          loaders: ['@svgr/webpack'],
          as: '*.js',
        },
        
        // Custom CSS processing
        '*.module.css': {
          loaders: [
            {
              loader: 'css-loader',
              options: {
                modules: {
                  localIdentName: '[name]__[local]___[hash:base64:5]',
                },
              },
            },
          ],
          as: '*.css',
        },
        
        // GraphQL files
        '*.graphql': {
          loaders: ['graphql-tag/loader'],
          as: '*.js',
        },
        
        // YAML files
        '*.yaml': {
          loaders: ['yaml-loader'],
          as: '*.json',
        },
      },
      
      resolveAlias: {
        '@': './src',
        '@components': './src/components',
        '@hooks': './src/hooks',
        '@utils': './src/utils',
        '@types': './src/types',
        '@styles': './src/styles',
      },
      
      resolveExtensions: [
        '.mdx',
        '.tsx',
        '.ts',
        '.jsx',
        '.js',
        '.mjs',
        '.json',
      ],
    },
  },
}
```

### 2. Environment-Specific Mystical Configuration

```javascript
// next.config.js
const nextConfig = {
  experimental: {
    turbo: {
      // Development-specific optimizations
      ...(process.env.NODE_ENV === 'development' && {
        memoryLimit: 4096, // 4GB memory limit
        loaderThreads: 4,   // Number of loader threads
      }),
      
      rules: {
        // Conditional rules based on environment
        '*.test.{ts,tsx,js,jsx}': process.env.NODE_ENV === 'test' ? {
          loaders: ['babel-loader'],
          as: '*.js',
        } : undefined,
      },
    },
  },
  
  // Environment variables
  env: {
    TURBOPACK_ENABLED: process.env.NODE_ENV === 'development' ? 'true' : 'false',
  },
}
```

## Performance Optimization Spells

### 1. Memory and Threading Mystical Configuration

```javascript
// next.config.js
const nextConfig = {
  experimental: {
    turbo: {
      // Memory management
      memoryLimit: 8192, // 8GB for large projects
      
      // Threading configuration
      loaderThreads: Math.max(1, require('os').cpus().length - 1),
      
      // Caching strategies
      cache: {
        // Cache directory
        cacheDirectory: '.turbo',
        
        // Cache invalidation
        buildId: process.env.BUILD_ID || 'development',
      },
    },
  },
}
```

### 2. Module Resolution Optimization Magic

```javascript
// next.config.js
const nextConfig = {
  experimental: {
    turbo: {
      resolveAlias: {
        // Optimize common imports
        'react': require.resolve('react'),
        'react-dom': require.resolve('react-dom'),
        'next': require.resolve('next'),
        
        // Project aliases
        '@': './src',
        '@components': './src/components',
        '@utils': './src/utils',
      },
      
      // Module resolution extensions
      resolveExtensions: [
        '.tsx',
        '.ts',
        '.jsx',
        '.js',
        '.json',
      ],
      
      // Resolve modules configuration
      resolveModules: [
        'node_modules',
        './src',
      ],
    },
  },
}
```

## Integration Mystical Patterns

### 1. Turbopack with TypeScript Divination

```typescript
// turbo.d.ts - Type definitions for Turbopack
declare module '*.svg' {
  import { FC, SVGProps } from 'react'
  const content: FC<SVGProps<SVGElement>>
  export default content
}

declare module '*.graphql' {
  import { DocumentNode } from 'graphql'
  const Schema: DocumentNode
  export default Schema
}

declare module '*.yaml' {
  const content: Record<string, any>
  export default content
}
```

```javascript
// next.config.js
const nextConfig = {
  experimental: {
    turbo: {
      rules: {
        '*.ts': {
          loaders: [
            {
              loader: 'builtin:swc-loader',
              options: {
                jsc: {
                  parser: {
                    syntax: 'typescript',
                    tsx: false,
                    decorators: true,
                  },
                  transform: {
                    react: {
                      runtime: 'automatic',
                    },
                  },
                },
              },
            },
          ],
          as: '*.js',
        },
      },
    },
  },
}
```

### 2. Turbopack with Tailwind CSS Styling Magic

```javascript
// next.config.js
const nextConfig = {
  experimental: {
    turbo: {
      rules: {
        '*.css': {
          loaders: [
            {
              loader: 'builtin:css-loader',
              options: {
                postcss: {
                  plugins: [
                    'tailwindcss',
                    'autoprefixer',
                  ],
                },
              },
            },
          ],
          as: '*.css',
        },
      },
    },
  },
}
```

### 3. Turbopack with MDX Content Magic

```bash
# Install MDX dependencies
pnpm add @next/mdx @mdx-js/loader @mdx-js/react
```

```javascript
// next.config.js
const withMDX = require('@next/mdx')({
  extension: /\.mdx?$/,
  options: {
    remarkPlugins: [],
    rehypePlugins: [],
  },
})

const nextConfig = {
  experimental: {
    turbo: {
      rules: {
        '*.mdx': {
          loaders: [
            {
              loader: '@mdx-js/loader',
              options: {
                providerImportSource: '@mdx-js/react',
              },
            },
          ],
          as: '*.js',
        },
      },
    },
  },
  pageExtensions: ['ts', 'tsx', 'js', 'jsx', 'md', 'mdx'],
}

module.exports = withMDX(nextConfig)
```

## Development Mystical Experience

### 1. Hot Module Replacement (HMR) Magic

```typescript
// components/Counter.tsx
'use client'

import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)

  return (
    <div className="p-4">
      <h2 className="text-2xl font-bold mb-4">Counter: {count}</h2>
      <button
        onClick={() => setCount(count + 1)}
        className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
      >
        Increment
      </button>
    </div>
  )
}

// Turbopack automatically handles HMR magic for this component
// Changes will be reflected instantly without page refresh through mystical updates
```

### 2. Error Handling and Mystical Debugging

```javascript
// next.config.js
const nextConfig = {
  experimental: {
    turbo: {
      // Enhanced error reporting
      logLevel: 'info', // 'error' | 'warn' | 'info' | 'debug' | 'trace'
      
      // Source maps for debugging
      devtool: 'cheap-module-source-map',
      
      // Error overlay configuration
      overlay: {
        errors: true,
        warnings: false,
      },
    },
  },
}
```

## Migration from Webpack Sorcery

### 1. Webpack to Turbopack Mystical Migration

```javascript
// Before (webpack.config.js)
module.exports = {
  module: {
    rules: [
      {
        test: /\.svg$/,
        use: ['@svgr/webpack'],
      },
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader', 'postcss-loader'],
      },
    ],
  },
  resolve: {
    alias: {
      '@': path.resolve(__dirname, 'src'),
    },
  },
}

// After (next.config.js with Turbopack)
const nextConfig = {
  experimental: {
    turbo: {
      rules: {
        '*.svg': {
          loaders: ['@svgr/webpack'],
          as: '*.js',
        },
        '*.css': {
          loaders: [
            {
              loader: 'builtin:css-loader',
              options: {
                postcss: {
                  plugins: ['tailwindcss', 'autoprefixer'],
                },
              },
            },
          ],
          as: '*.css',
        },
      },
      resolveAlias: {
        '@': './src',
      },
    },
  },
}
```

### 2. Common Migration Mystical Issues

```javascript
// next.config.js - Migration helpers
const nextConfig = {
  experimental: {
    turbo: {
      // Handle webpack-specific loaders
      rules: {
        // Replace file-loader
        '\\.(png|jpe?g|gif|webp)$': {
          loaders: ['builtin:file-loader'],
          as: '*.js',
        },
        
        // Replace url-loader
        '\\.(woff2?|eot|ttf|otf)$': {
          loaders: [
            {
              loader: 'builtin:url-loader',
              options: {
                limit: 8192,
              },
            },
          ],
          as: '*.js',
        },
      },
    },
  },
  
  // Webpack fallback for unsupported features
  webpack: (config, { dev, isServer }) => {
    // Only use webpack for specific cases
    if (!dev || isServer) {
      // Custom webpack configuration for production/server
      return config
    }
    return config
  },
}
```

## Monitoring and Analytics Divination

### 1. Build Performance Mystical Monitoring

```javascript
// next.config.js
const nextConfig = {
  experimental: {
    turbo: {
      // Performance monitoring
      profile: process.env.NODE_ENV === 'development',
      
      // Build analytics
      analytics: {
        enabled: true,
        buildId: process.env.BUILD_ID,
      },
    },
  },
  
  // Custom build analysis
  webpack: (config, { buildId, dev }) => {
    if (!dev) {
      // Production build analysis
      config.plugins.push(
        new (require('webpack-bundle-analyzer').BundleAnalyzerPlugin)({
          analyzerMode: 'static',
          openAnalyzer: false,
        })
      )
    }
    return config
  },
}
```

### 2. Development Mystical Metrics

```typescript
// utils/performance.ts
export function measureTurbopackPerformance() {
  if (process.env.NODE_ENV === 'development') {
    // Measure HMR performance
    if (module.hot) {
      const start = performance.now()
      
      module.hot.accept(() => {
        const end = performance.now()
        console.log(`HMR update took ${end - start}ms`)
      })
    }
    
    // Measure initial load time
    window.addEventListener('load', () => {
      const navigation = performance.getEntriesByType('navigation')[0] as PerformanceNavigationTiming
      console.log(`Page load time: ${navigation.loadEventEnd - navigation.fetchStart}ms`)
    })
  }
}
```

## Wisdom of the Turbopack Ancients

- **Incremental Mystical Adoption**: Start with development mode before moving to production realm
- **Memory Management Magic**: Configure appropriate memory limits for your mystical project size
- **Caching Strategy Sorcery**: Leverage Turbopack's advanced caching for faster mystical rebuilds
- **Module Resolution Enchantments**: Optimize import paths and aliases for better magical performance
- **Error Handling Divination**: Use enhanced error reporting for better debugging mystical experience
- **Migration Planning Rituals**: Gradually migrate webpack-specific configurations through careful spells

## Common Curses & Their Remedies

### Curse 1: Unsupported Webpack Mystical Loaders
Some webpack loaders may not have Turbopack mystical equivalents.

**Counter-Spell:**
Use Turbopack's built-in loaders or create custom mystical rules:
```javascript
// Use built-in loaders when available
rules: {
  '*.css': {
    loaders: ['builtin:css-loader'], // Instead of css-loader
    as: '*.css',
  },
}
```

### Curse 2: Memory Drain Issues with Large Mystical Projects
Turbopack may consume significant mystical memory on large codebases.

**Counter-Spell:**
Configure memory limits and optimize mystical module resolution:
```javascript
experimental: {
  turbo: {
    memoryLimit: 8192, // Increase memory limit
    loaderThreads: 2,  // Reduce threads if needed
  },
}
```

### Curse 3: HMR Magic Not Working
Hot Module Replacement magic may not work with certain mystical file types.

**Counter-Spell:**
Ensure proper loader configuration and mystical file extensions:
```javascript
rules: {
  '*.tsx': {
    loaders: ['builtin:swc-loader'],
    as: '*.js',
  },
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://turbo.build/pack" %}

{% embed url="https://nextjs.org/docs/architecture/turbopack" %}

{% embed url="https://turbo.build/pack/docs" %}
