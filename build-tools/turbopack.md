---
description: >-
  Complete guide to Turbopack, Vercel's next-generation bundler for Next.js.
  Learn setup, configuration, performance optimization, and migration strategies
  for ultra-fast development and build times.
---

# Turbopack

## Introduction

Turbopack is Vercel's next-generation bundler built in Rust, designed to replace Webpack in Next.js applications. It provides significantly faster build times, improved development experience, and better performance through incremental compilation and advanced caching strategies.

## Use Cases

1. **Next.js Applications**: Ultra-fast development server and optimized production builds
2. **Large Codebases**: Improved build performance for applications with thousands of modules
3. **Development Experience**: Instant updates and faster Hot Module Replacement (HMR)
4. **Monorepo Projects**: Efficient handling of complex project structures

## Setup and Configuration

### 1. Enable Turbopack in Next.js

```bash
# Create new Next.js project with Turbopack
pnpm create next-app@latest my-turbo-app --typescript --tailwind --eslint --app

# Navigate to project
cd my-turbo-app

# Enable Turbopack in development
pnpm dev --turbo
```

### 2. Package.json Scripts

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

### 3. Next.js Configuration

```javascript
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    turbo: {
      // Turbopack configuration options
      rules: {
        // Custom loader rules
        '*.svg': {
          loaders: ['@svgr/webpack'],
          as: '*.js',
        },
      },
      resolveAlias: {
        // Path aliases
        '@': './src',
        '@components': './src/components',
        '@utils': './src/utils',
      },
    },
  },
}

module.exports = nextConfig
```

## Advanced Configuration

### 1. Custom Loaders and Rules

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

### 2. Environment-Specific Configuration

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

## Performance Optimization

### 1. Memory and Threading Configuration

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

### 2. Module Resolution Optimization

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

## Integration Patterns

### 1. Turbopack with TypeScript

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

### 2. Turbopack with Tailwind CSS

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

### 3. Turbopack with MDX

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

## Development Experience

### 1. Hot Module Replacement (HMR)

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

// Turbopack automatically handles HMR for this component
// Changes will be reflected instantly without page refresh
```

### 2. Error Handling and Debugging

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

## Migration from Webpack

### 1. Webpack to Turbopack Migration

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

### 2. Common Migration Issues

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

## Monitoring and Analytics

### 1. Build Performance Monitoring

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

### 2. Development Metrics

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

## Best Practices

- **Incremental Adoption**: Start with development mode before moving to production
- **Memory Management**: Configure appropriate memory limits for your project size
- **Caching Strategy**: Leverage Turbopack's advanced caching for faster rebuilds
- **Module Resolution**: Optimize import paths and aliases for better performance
- **Error Handling**: Use enhanced error reporting for better debugging experience
- **Migration Planning**: Gradually migrate webpack-specific configurations

## Common Pitfalls

### Issue 1: Unsupported Webpack Loaders
Some webpack loaders may not have Turbopack equivalents.

**Solution:**
Use Turbopack's built-in loaders or create custom rules:
```javascript
// Use built-in loaders when available
rules: {
  '*.css': {
    loaders: ['builtin:css-loader'], // Instead of css-loader
    as: '*.css',
  },
}
```

### Issue 2: Memory Issues with Large Projects
Turbopack may consume significant memory on large codebases.

**Solution:**
Configure memory limits and optimize module resolution:
```javascript
experimental: {
  turbo: {
    memoryLimit: 8192, // Increase memory limit
    loaderThreads: 2,  // Reduce threads if needed
  },
}
```

### Issue 3: HMR Not Working
Hot Module Replacement may not work with certain file types.

**Solution:**
Ensure proper loader configuration and file extensions:
```javascript
rules: {
  '*.tsx': {
    loaders: ['builtin:swc-loader'],
    as: '*.js',
  },
}
```

## References

{% embed url="https://turbo.build/pack" %}

{% embed url="https://nextjs.org/docs/architecture/turbopack" %}

{% embed url="https://turbo.build/pack/docs" %}
