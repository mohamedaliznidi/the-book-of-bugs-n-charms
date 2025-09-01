---
description: >-
  Complete guide to Core Web Vitals optimization for modern web applications.
  Learn measurement techniques, optimization strategies, and monitoring tools
  for LCP, FID, CLS, and other performance metrics in React and Next.js apps.
---

# Core Web Vitals

## Introduction

Core Web Vitals are a set of real-world, user-centered metrics that quantify key aspects of user experience. They measure loading performance, interactivity, and visual stability. Google uses these metrics as ranking factors, making them crucial for both user experience and SEO.

## Use Cases

1. **SEO Optimization**: Improve search engine rankings through better performance scores
2. **User Experience**: Enhance user satisfaction with faster, more responsive applications
3. **Business Metrics**: Reduce bounce rates and increase conversion rates
4. **Performance Monitoring**: Track and maintain application performance over time

## Core Web Vitals Metrics

### 1. Largest Contentful Paint (LCP)

LCP measures loading performance - specifically when the largest content element becomes visible.

**Target:** 2.5 seconds or less

```typescript
// Measuring LCP
function measureLCP() {
  new PerformanceObserver((entryList) => {
    const entries = entryList.getEntries()
    const lastEntry = entries[entries.length - 1]
    
    console.log('LCP:', lastEntry.startTime)
    
    // Send to analytics
    gtag('event', 'web_vitals', {
      name: 'LCP',
      value: Math.round(lastEntry.startTime),
      event_category: 'Web Vitals',
    })
  }).observe({ type: 'largest-contentful-paint', buffered: true })
}

// Optimization techniques for LCP
export function OptimizedHero() {
  return (
    <section className="hero">
      {/* Use Next.js Image with priority for hero images */}
      <Image
        src="/hero-image.jpg"
        alt="Hero image"
        width={1200}
        height={600}
        priority // This ensures the image loads immediately
        placeholder="blur"
        blurDataURL="data:image/jpeg;base64,..."
      />
      
      {/* Preload critical fonts */}
      <style jsx>{`
        @font-face {
          font-family: 'Critical Font';
          src: url('/fonts/critical.woff2') format('woff2');
          font-display: swap;
        }
      `}</style>
      
      <h1 className="hero-title">Welcome to Our Site</h1>
    </section>
  )
}
```

### 2. First Input Delay (FID) / Interaction to Next Paint (INP)

FID measures interactivity - the delay between user interaction and browser response. INP is replacing FID as a Core Web Vital.

**Target:** 100ms or less (FID), 200ms or less (INP)

```typescript
// Measuring FID
function measureFID() {
  new PerformanceObserver((entryList) => {
    for (const entry of entryList.getEntries()) {
      console.log('FID:', entry.processingStart - entry.startTime)
      
      gtag('event', 'web_vitals', {
        name: 'FID',
        value: Math.round(entry.processingStart - entry.startTime),
        event_category: 'Web Vitals',
      })
    }
  }).observe({ type: 'first-input', buffered: true })
}

// Optimization: Code splitting and lazy loading
import { lazy, Suspense } from 'react'

const HeavyComponent = lazy(() => import('./HeavyComponent'))

export function OptimizedApp() {
  return (
    <div>
      {/* Critical content loads immediately */}
      <Header />
      <MainContent />
      
      {/* Heavy components load when needed */}
      <Suspense fallback={<div>Loading...</div>}>
        <HeavyComponent />
      </Suspense>
    </div>
  )
}

// Optimization: Debounce expensive operations
import { useMemo, useState, useCallback } from 'react'
import { debounce } from 'lodash-es'

export function SearchComponent() {
  const [query, setQuery] = useState('')
  const [results, setResults] = useState([])

  const debouncedSearch = useCallback(
    debounce(async (searchQuery: string) => {
      if (searchQuery) {
        const results = await searchAPI(searchQuery)
        setResults(results)
      }
    }, 300),
    []
  )

  const handleInputChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const value = e.target.value
    setQuery(value)
    debouncedSearch(value)
  }

  return (
    <div>
      <input
        type="text"
        value={query}
        onChange={handleInputChange}
        placeholder="Search..."
      />
      <SearchResults results={results} />
    </div>
  )
}
```

### 3. Cumulative Layout Shift (CLS)

CLS measures visual stability - how much content shifts during loading.

**Target:** 0.1 or less

```typescript
// Measuring CLS
function measureCLS() {
  let clsValue = 0
  
  new PerformanceObserver((entryList) => {
    for (const entry of entryList.getEntries()) {
      if (!entry.hadRecentInput) {
        clsValue += entry.value
      }
    }
    
    console.log('CLS:', clsValue)
    
    gtag('event', 'web_vitals', {
      name: 'CLS',
      value: Math.round(clsValue * 1000),
      event_category: 'Web Vitals',
    })
  }).observe({ type: 'layout-shift', buffered: true })
}

// Optimization: Reserve space for dynamic content
export function StableLayout() {
  const [imageLoaded, setImageLoaded] = useState(false)
  
  return (
    <article className="article">
      {/* Reserve space for images */}
      <div className="image-container" style={{ aspectRatio: '16/9' }}>
        <Image
          src="/article-image.jpg"
          alt="Article image"
          fill
          style={{ objectFit: 'cover' }}
          onLoad={() => setImageLoaded(true)}
        />
      </div>
      
      {/* Reserve space for dynamic content */}
      <div className="content">
        <h1>Article Title</h1>
        <div className="ad-space" style={{ minHeight: '250px' }}>
          {/* Ad content loads here without shifting layout */}
          <AdComponent />
        </div>
        <p>Article content...</p>
      </div>
    </article>
  )
}

// CSS for stable layouts
const styles = `
  .image-container {
    position: relative;
    width: 100%;
    background-color: #f0f0f0; /* Placeholder background */
  }
  
  .ad-space {
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #f8f8f8;
    border: 1px dashed #ccc;
  }
  
  /* Prevent font loading from causing layout shift */
  .text-content {
    font-family: system-ui, -apple-system, sans-serif;
  }
  
  @font-face {
    font-family: 'Custom Font';
    src: url('/fonts/custom.woff2') format('woff2');
    font-display: swap;
    size-adjust: 100%; /* Adjust to match fallback font metrics */
  }
`
```

## Advanced Optimization Techniques

### 1. Resource Prioritization

```typescript
// app/layout.tsx - Resource hints and preloading
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <head>
        {/* Preconnect to external domains */}
        <link rel="preconnect" href="https://fonts.googleapis.com" />
        <link rel="preconnect" href="https://fonts.gstatic.com" crossOrigin="" />
        <link rel="preconnect" href="https://api.example.com" />
        
        {/* Preload critical resources */}
        <link
          rel="preload"
          href="/fonts/inter-var.woff2"
          as="font"
          type="font/woff2"
          crossOrigin=""
        />
        
        {/* DNS prefetch for third-party domains */}
        <link rel="dns-prefetch" href="https://analytics.google.com" />
        <link rel="dns-prefetch" href="https://cdn.jsdelivr.net" />
        
        {/* Critical CSS inline */}
        <style dangerouslySetInnerHTML={{
          __html: `
            body { margin: 0; font-family: system-ui, sans-serif; }
            .hero { min-height: 60vh; display: flex; align-items: center; }
          `
        }} />
      </head>
      <body>
        {children}
      </body>
    </html>
  )
}
```

### 2. Image Optimization

```typescript
// components/OptimizedImage.tsx
import Image from 'next/image'
import { useState } from 'react'

interface OptimizedImageProps {
  src: string
  alt: string
  width: number
  height: number
  priority?: boolean
  className?: string
}

export function OptimizedImage({
  src,
  alt,
  width,
  height,
  priority = false,
  className = '',
}: OptimizedImageProps) {
  const [isLoading, setIsLoading] = useState(true)

  return (
    <div className={`image-wrapper ${className}`}>
      <Image
        src={src}
        alt={alt}
        width={width}
        height={height}
        priority={priority}
        placeholder="blur"
        blurDataURL={generateBlurDataURL(width, height)}
        onLoad={() => setIsLoading(false)}
        style={{
          transition: 'opacity 0.3s ease-in-out',
          opacity: isLoading ? 0.8 : 1,
        }}
        sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
      />
    </div>
  )
}

function generateBlurDataURL(width: number, height: number): string {
  const canvas = document.createElement('canvas')
  canvas.width = width
  canvas.height = height
  
  const ctx = canvas.getContext('2d')
  if (ctx) {
    ctx.fillStyle = '#f0f0f0'
    ctx.fillRect(0, 0, width, height)
  }
  
  return canvas.toDataURL()
}
```

### 3. Bundle Optimization

```typescript
// next.config.js
const nextConfig = {
  // Enable bundle analyzer
  ...(process.env.ANALYZE === 'true' && {
    webpack: (config) => {
      config.plugins.push(
        new (require('@next/bundle-analyzer'))({
          enabled: true,
          openAnalyzer: true,
        })
      )
      return config
    },
  }),
  
  // Optimize imports
  modularizeImports: {
    'lodash': {
      transform: 'lodash/{{member}}',
    },
    '@mui/material': {
      transform: '@mui/material/{{member}}',
    },
  },
  
  // Enable SWC minification
  swcMinify: true,
  
  // Optimize CSS
  experimental: {
    optimizeCss: true,
  },
}

// Dynamic imports for code splitting
export function FeatureComponent() {
  const [showAdvanced, setShowAdvanced] = useState(false)
  
  return (
    <div>
      <BasicFeatures />
      
      {showAdvanced && (
        <Suspense fallback={<div>Loading advanced features...</div>}>
          <AdvancedFeatures />
        </Suspense>
      )}
      
      <button onClick={() => setShowAdvanced(true)}>
        Load Advanced Features
      </button>
    </div>
  )
}

const AdvancedFeatures = lazy(() => 
  import('./AdvancedFeatures').then(module => ({
    default: module.AdvancedFeatures
  }))
)
```

## Performance Monitoring

### 1. Web Vitals Library

```bash
# Install web-vitals library
pnpm add web-vitals
```

```typescript
// lib/analytics.ts
import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals'

function sendToAnalytics(metric: any) {
  // Send to Google Analytics
  if (typeof gtag !== 'undefined') {
    gtag('event', metric.name, {
      event_category: 'Web Vitals',
      value: Math.round(metric.name === 'CLS' ? metric.value * 1000 : metric.value),
      event_label: metric.id,
      non_interaction: true,
    })
  }
  
  // Send to custom analytics
  fetch('/api/analytics', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({
      metric: metric.name,
      value: metric.value,
      id: metric.id,
      url: window.location.href,
      timestamp: Date.now(),
    }),
  }).catch(console.error)
}

export function initWebVitals() {
  getCLS(sendToAnalytics)
  getFID(sendToAnalytics)
  getFCP(sendToAnalytics)
  getLCP(sendToAnalytics)
  getTTFB(sendToAnalytics)
}

// app/layout.tsx
'use client'

import { useEffect } from 'react'
import { initWebVitals } from '@/lib/analytics'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  useEffect(() => {
    initWebVitals()
  }, [])

  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

### 2. Performance API Integration

```typescript
// lib/performance.ts
export class PerformanceMonitor {
  private static instance: PerformanceMonitor
  private metrics: Map<string, number[]> = new Map()

  static getInstance(): PerformanceMonitor {
    if (!PerformanceMonitor.instance) {
      PerformanceMonitor.instance = new PerformanceMonitor()
    }
    return PerformanceMonitor.instance
  }

  measureFunction<T>(name: string, fn: () => T): T {
    const start = performance.now()
    const result = fn()
    const end = performance.now()
    
    this.recordMetric(name, end - start)
    return result
  }

  async measureAsyncFunction<T>(name: string, fn: () => Promise<T>): Promise<T> {
    const start = performance.now()
    const result = await fn()
    const end = performance.now()
    
    this.recordMetric(name, end - start)
    return result
  }

  recordMetric(name: string, value: number) {
    if (!this.metrics.has(name)) {
      this.metrics.set(name, [])
    }
    this.metrics.get(name)!.push(value)
  }

  getMetrics(name: string) {
    const values = this.metrics.get(name) || []
    return {
      count: values.length,
      average: values.reduce((a, b) => a + b, 0) / values.length,
      min: Math.min(...values),
      max: Math.max(...values),
    }
  }

  getAllMetrics() {
    const result: Record<string, any> = {}
    for (const [name] of this.metrics) {
      result[name] = this.getMetrics(name)
    }
    return result
  }
}

// Usage in components
export function DataComponent() {
  const [data, setData] = useState(null)
  const monitor = PerformanceMonitor.getInstance()

  useEffect(() => {
    const fetchData = async () => {
      const result = await monitor.measureAsyncFunction('api-fetch', async () => {
        const response = await fetch('/api/data')
        return response.json()
      })
      setData(result)
    }

    fetchData()
  }, [monitor])

  const handleExpensiveOperation = () => {
    monitor.measureFunction('expensive-operation', () => {
      // Expensive computation here
      return processLargeDataset(data)
    })
  }

  return (
    <div>
      {data ? <DataDisplay data={data} /> : <Loading />}
      <button onClick={handleExpensiveOperation}>
        Process Data
      </button>
    </div>
  )
}
```

### 3. Real User Monitoring (RUM)

```typescript
// lib/rum.ts
interface RUMData {
  url: string
  userAgent: string
  connectionType: string
  deviceMemory: number
  hardwareConcurrency: number
  metrics: {
    lcp: number
    fid: number
    cls: number
    fcp: number
    ttfb: number
  }
}

export class RealUserMonitoring {
  private data: Partial<RUMData> = {}

  constructor() {
    this.collectDeviceInfo()
    this.collectNetworkInfo()
    this.collectMetrics()
  }

  private collectDeviceInfo() {
    this.data.userAgent = navigator.userAgent
    this.data.deviceMemory = (navigator as any).deviceMemory || 0
    this.data.hardwareConcurrency = navigator.hardwareConcurrency || 0
  }

  private collectNetworkInfo() {
    const connection = (navigator as any).connection
    if (connection) {
      this.data.connectionType = connection.effectiveType || 'unknown'
    }
  }

  private collectMetrics() {
    // Collect Core Web Vitals
    import('web-vitals').then(({ getCLS, getFID, getFCP, getLCP, getTTFB }) => {
      getCLS((metric) => {
        this.data.metrics = { ...this.data.metrics, cls: metric.value }
        this.sendData()
      })
      
      getFID((metric) => {
        this.data.metrics = { ...this.data.metrics, fid: metric.value }
        this.sendData()
      })
      
      getFCP((metric) => {
        this.data.metrics = { ...this.data.metrics, fcp: metric.value }
        this.sendData()
      })
      
      getLCP((metric) => {
        this.data.metrics = { ...this.data.metrics, lcp: metric.value }
        this.sendData()
      })
      
      getTTFB((metric) => {
        this.data.metrics = { ...this.data.metrics, ttfb: metric.value }
        this.sendData()
      })
    })
  }

  private sendData() {
    if (this.isDataComplete()) {
      fetch('/api/rum', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          ...this.data,
          url: window.location.href,
          timestamp: Date.now(),
        }),
      }).catch(console.error)
    }
  }

  private isDataComplete(): boolean {
    return !!(
      this.data.metrics &&
      this.data.metrics.lcp &&
      this.data.metrics.fcp &&
      this.data.metrics.cls !== undefined
    )
  }
}

// Initialize RUM
if (typeof window !== 'undefined') {
  new RealUserMonitoring()
}
```

## Best Practices

- **Measure First**: Always measure before optimizing to identify actual bottlenecks
- **Progressive Enhancement**: Build fast baseline experiences, then enhance
- **Critical Path**: Optimize the critical rendering path for above-the-fold content
- **Resource Hints**: Use preload, prefetch, and preconnect strategically
- **Code Splitting**: Split code at route and component levels for optimal loading
- **Image Optimization**: Use modern formats, proper sizing, and lazy loading

## Common Pitfalls

### Issue 1: Over-optimization
Optimizing metrics that don't impact user experience.

**Solution:**
Focus on user-centric metrics and real-world performance:
```typescript
// ❌ Wrong - optimizing vanity metrics
const bundleSize = getBundleSize() // Not directly user-facing

// ✅ Correct - optimizing user experience
const lcp = getLCP() // Directly impacts user perception
```

### Issue 2: Layout Shift from Dynamic Content
Content loading causing visual instability.

**Solution:**
Reserve space for dynamic content:
```css
/* Reserve space for ads, images, etc. */
.ad-container {
  min-height: 250px;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

### Issue 3: Blocking JavaScript
Large JavaScript bundles blocking the main thread.

**Solution:**
Use code splitting and lazy loading:
```typescript
// Split large components
const HeavyChart = lazy(() => import('./HeavyChart'))

// Use dynamic imports for libraries
const loadChart = async () => {
  const { Chart } = await import('chart.js')
  return new Chart(ctx, config)
}
```

## References

{% embed url="https://web.dev/vitals/" %}

{% embed url="https://nextjs.org/docs/advanced-features/measuring-performance" %}

{% embed url="https://github.com/GoogleChrome/web-vitals" %}
