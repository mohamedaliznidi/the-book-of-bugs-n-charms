---
description: >-
  Achieve perfect performance with Astro static site generation. This recipe covers content collections, island architecture, minimal JavaScript delivery, SEO optimization, image optimization, analytics integration, and deployment strategies for lightning-fast content-focused websites.

theme: "magic"
---

# 🌟 Static Sorcery: Astro Content Recipe

*When speed is everything and JavaScript is optional*

---

## 🎯 Cast This When You Need

**Lightning Speed** • **Perfect SEO** • **Minimal JS** • **Content Focus** • **100% Lighthouse Scores**

Perfect for: Blogs, documentation sites, marketing pages, portfolios, content-heavy sites, maximum performance

Terrible for: Interactive dashboards, real-time apps, complex state management, when you need heavy client-side logic

---

## ⚡ The Zero-JS Summoning

```bash
# Create your content kingdom
npm create astro@latest my-content-empire -- --template blog --typescript strict --no-install

cd my-content-empire
npm install

# Add superpowers
npm install @astrojs/tailwind @astrojs/mdx @astrojs/sitemap @astrojs/rss
npm install @astrojs/image sharp

# Launch at light speed
npm run dev
```

**Witness!** Your blazing-fast site appears at `http://localhost:4321`

---

## 🏗️ The Content Architecture

```
src/
├── components/          # Reusable Astro components
│   ├── BaseHead.astro  # SEO meta tags
│   ├── Header.astro    # Site header
│   └── BlogPost.astro  # Blog post layout
├── content/            # Content collections
│   ├── blog/          # Blog posts (MDX)
│   ├── docs/          # Documentation
│   └── config.ts      # Content schema
├── layouts/           # Page layouts
│   ├── BaseLayout.astro
│   └── BlogPost.astro
├── pages/             # File-based routing
│   ├── index.astro    # Homepage
│   ├── blog/          # Blog routes
│   └── [...slug].astro # Dynamic routes
├── styles/            # Global styles
└── utils/             # Helper functions
```

---

## 📝 Content Collections Mastery

### Define Your Content Schema
```typescript
// src/content/config.ts
import { defineCollection, z } from 'astro:content'

const blogCollection = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    pubDate: z.date(),
    updatedDate: z.date().optional(),
    heroImage: z.string().optional(),
    tags: z.array(z.string()),
    author: z.object({
      name: z.string(),
      avatar: z.string().url(),
      bio: z.string(),
    }),
    draft: z.boolean().default(false),
    featured: z.boolean().default(false),
  })
})

const docsCollection = defineCollection({
  type: 'content',
  schema: z.object({
    title: z.string(),
    description: z.string(),
    order: z.number(),
    category: z.enum(['getting-started', 'guides', 'reference']),
    lastModified: z.date(),
  })
})

export const collections = {
  'blog': blogCollection,
  'docs': docsCollection,
}
```

### Content Creation Magic
```markdown
---
# src/content/blog/astro-performance-secrets.mdx
title: "Astro Performance Secrets: How We Hit 100% Lighthouse Scores"
description: "Discover the techniques we use to achieve perfect performance with Astro"
pubDate: 2024-01-15
heroImage: "/blog-images/performance-secrets.jpg"
tags: ["performance", "astro", "web-vitals"]
author:
  name: "Performance Wizard"
  avatar: "https://example.com/avatar.jpg"
  bio: "Obsessed with making the web faster"
featured: true
---

import { Image } from 'astro:assets'
import CodeBlock from '../../components/CodeBlock.astro'
import PerformanceChart from '../../components/PerformanceChart.jsx'

# The Quest for Perfect Performance

Here's how we achieved **100% Lighthouse scores** consistently:

## 1. Image Optimization Sorcery

<Image 
  src="/performance-before-after.jpg" 
  alt="Performance comparison" 
  width={800} 
  height={400} 
  loading="lazy"
/>

## 2. Critical CSS Inlining

<CodeBlock lang="css">
/* Only critical styles are inlined */
body { font-family: system-ui; }
.hero { display: grid; }
</CodeBlock>

## 3. Interactive Islands

<PerformanceChart client:visible data={performanceData} />

The key is using `client:visible` only when needed!
```

---

## 🎨 Component Island Architecture

### Static Components (Zero JS)
```astro
---
// src/components/Hero.astro
interface Props {
  title: string
  subtitle: string
  backgroundImage?: string
}

const { title, subtitle, backgroundImage } = Astro.props
---

<section class="hero" style={backgroundImage ? `background-image: url(${backgroundImage})` : ''}>
  <div class="hero-content">
    <h1 class="hero-title">{title}</h1>
    <p class="hero-subtitle">{subtitle}</p>
    <div class="hero-actions">
      <slot name="actions" />
    </div>
  </div>
</section>

<style>
  .hero {
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 70vh;
    background-size: cover;
    background-position: center;
    position: relative;
  }
  
  .hero-content {
    text-align: center;
    max-width: 600px;
    padding: 2rem;
  }
  
  .hero-title {
    font-size: clamp(2.5rem, 5vw, 4rem);
    font-weight: 900;
    line-height: 1.1;
    margin-bottom: 1rem;
  }
</style>
```

### Interactive Islands (Selective JS)
```jsx
// src/components/NewsletterSignup.jsx
import { useState } from 'react'

export default function NewsletterSignup() {
  const [email, setEmail] = useState('')
  const [status, setStatus] = useState('idle')

  const handleSubmit = async (e) => {
    e.preventDefault()
    setStatus('loading')
    
    try {
      const response = await fetch('/api/newsletter', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ email }),
      })
      
      if (response.ok) {
        setStatus('success')
        setEmail('')
      } else {
        setStatus('error')
      }
    } catch (error) {
      setStatus('error')
    }
  }

  return (
    <div className="newsletter-signup">
      <h3>Stay Updated! 📧</h3>
      <form onSubmit={handleSubmit}>
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          placeholder="Enter your email"
          required
        />
        <button 
          type="submit" 
          disabled={status === 'loading'}
        >
          {status === 'loading' ? 'Subscribing...' : 'Subscribe'}
        </button>
      </form>
      
      {status === 'success' && (
        <p className="success">✅ Successfully subscribed!</p>
      )}
      {status === 'error' && (
        <p className="error">❌ Something went wrong. Try again.</p>
      )}
    </div>
  )
}
```

### Using Islands in Pages
```astro
---
// src/pages/blog/[...slug].astro
import { getCollection } from 'astro:content'
import BaseLayout from '../../layouts/BaseLayout.astro'
import NewsletterSignup from '../../components/NewsletterSignup.jsx'
import TableOfContents from '../../components/TableOfContents.astro'
import ReadingProgress from '../../components/ReadingProgress.jsx'

export async function getStaticPaths() {
  const blogEntries = await getCollection('blog')
  return blogEntries.map(entry => ({
    params: { slug: entry.slug },
    props: { entry },
  }))
}

const { entry } = Astro.props
const { Content, headings } = await entry.render()
---

<BaseLayout title={entry.data.title} description={entry.data.description}>
  <!-- Static component - no JS -->
  <TableOfContents headings={headings} />
  
  <!-- Interactive component - JS only loads when visible -->
  <ReadingProgress client:visible />
  
  <article>
    <Content />
    
    <!-- Interactive component - JS loads on idle -->
    <NewsletterSignup client:idle />
  </article>
</BaseLayout>
```

---

## 🚀 Performance Optimization Mastery

### Image Optimization Magic
```astro
---
// src/components/OptimizedImage.astro
import { Image } from 'astro:assets'

interface Props {
  src: string
  alt: string
  width: number
  height: number
  loading?: 'lazy' | 'eager'
  sizes?: string
  quality?: number
}

const { 
  src, 
  alt, 
  width, 
  height, 
  loading = 'lazy', 
  sizes = '(max-width: 768px) 100vw, 50vw',
  quality = 80 
} = Astro.props
---

<Image
  src={src}
  alt={alt}
  width={width}
  height={height}
  loading={loading}
  sizes={sizes}
  quality={quality}
  format="webp"
  fallbackFormat="jpg"
  class="optimized-image"
/>

<style>
  .optimized-image {
    height: auto;
    border-radius: 8px;
    box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1);
    transition: transform 0.2s ease-in-out;
  }
  
  .optimized-image:hover {
    transform: scale(1.02);
  }
</style>
```

### Critical CSS Inlining Strategy
```astro
---
// src/layouts/BaseLayout.astro
interface Props {
  title: string
  description: string
  image?: string
}

const { title, description, image = '/default-og-image.jpg' } = Astro.props
const canonicalURL = new URL(Astro.url.pathname, Astro.site)
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    
    <!-- Critical CSS inlined -->
    <style>
      /* Critical above-the-fold styles */
      body { 
        font-family: system-ui, -apple-system, sans-serif; 
        line-height: 1.6; 
        margin: 0; 
      }
      .header { 
        background: #ffffff; 
        border-bottom: 1px solid #e5e7eb; 
        position: sticky; 
        top: 0; 
        z-index: 50; 
      }
      .hero { 
        min-height: 60vh; 
        display: flex; 
        align-items: center; 
      }
    </style>
    
    <!-- SEO Magic -->
    <title>{title}</title>
    <meta name="description" content={description} />
    <link rel="canonical" href={canonicalURL} />
    
    <!-- Open Graph -->
    <meta property="og:type" content="website" />
    <meta property="og:url" content={canonicalURL} />
    <meta property="og:title" content={title} />
    <meta property="og:description" content={description} />
    <meta property="og:image" content={new URL(image, Astro.url)} />
    
    <!-- Twitter Card -->
    <meta property="twitter:card" content="summary_large_image" />
    <meta property="twitter:url" content={canonicalURL} />
    <meta property="twitter:title" content={title} />
    <meta property="twitter:description" content={description} />
    <meta property="twitter:image" content={new URL(image, Astro.url)} />
    
    <!-- Preload critical resources -->
    <link rel="preload" href="/fonts/inter-var.woff2" as="font" type="font/woff2" crossorigin />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    
    <!-- Non-critical CSS loaded async -->
    <link rel="preload" href="/styles/global.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
    <noscript><link rel="stylesheet" href="/styles/global.css"></noscript>
  </head>
  
  <body>
    <slot />
    
    <!-- Analytics script loads last -->
    <script>
      // Minimal analytics tracking
      if ('navigator' in window && 'sendBeacon' in navigator) {
        window.addEventListener('beforeunload', () => {
          navigator.sendBeacon('/api/analytics', JSON.stringify({
            url: location.pathname,
            referrer: document.referrer,
            timestamp: Date.now()
          }))
        })
      }
    </script>
  </body>
</html>
```

---

## 🔥 Advanced Content Patterns

### Dynamic Content with View Transitions
```astro
---
// src/pages/blog/[...page].astro
import { getCollection } from 'astro:content'
import BaseLayout from '../../layouts/BaseLayout.astro'
import BlogCard from '../../components/BlogCard.astro'

export async function getStaticPaths({ paginate }) {
  const allPosts = await getCollection('blog')
  const publishedPosts = allPosts
    .filter(post => !post.data.draft)
    .sort((a, b) => b.data.pubDate.valueOf() - a.data.pubDate.valueOf())
  
  return paginate(publishedPosts, { pageSize: 6 })
}

const { page } = Astro.props
---

<BaseLayout title="Blog" description="Latest articles and insights">
  <div class="blog-grid">
    {page.data.map((post) => (
      <BlogCard
        href={`/blog/${post.slug}/`}
        title={post.data.title}
        description={post.data.description}
        pubDate={post.data.pubDate}
        heroImage={post.data.heroImage}
        tags={post.data.tags}
        transition:name={`post-${post.slug}`}
      />
    ))}
  </div>
  
  <!-- Pagination with View Transitions -->
  <nav class="pagination">
    {page.url.prev && (
      <a href={page.url.prev} class="pagination-link">
        ← Previous
      </a>
    )}
    <span class="pagination-info">
      Page {page.currentPage} of {page.lastPage}
    </span>
    {page.url.next && (
      <a href={page.url.next} class="pagination-link">
        Next →
      </a>
    )}
  </nav>
</BaseLayout>

<style>
  .blog-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
    gap: 2rem;
    margin: 2rem 0;
  }
  
  .pagination {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin: 3rem 0;
    padding: 1rem 0;
    border-top: 1px solid #e5e7eb;
  }
  
  .pagination-link {
    padding: 0.75rem 1.5rem;
    background: #f3f4f6;
    border-radius: 0.5rem;
    text-decoration: none;
    transition: background-color 0.2s;
  }
  
  .pagination-link:hover {
    background: #e5e7eb;
  }
</style>
```

### Content Search & Filtering
```astro
---
// src/components/BlogSearch.astro
import { getCollection } from 'astro:content'

const allPosts = await getCollection('blog')
const allTags = [...new Set(allPosts.flatMap(post => post.data.tags))]
---

<div class="search-filters">
  <input 
    type="text" 
    id="search-input" 
    placeholder="Search articles..."
    class="search-input"
  />
  
  <select id="tag-filter" class="tag-filter">
    <option value="">All topics</option>
    {allTags.map(tag => (
      <option value={tag}>{tag}</option>
    ))}
  </select>
  
  <select id="sort-filter" class="sort-filter">
    <option value="newest">Newest first</option>
    <option value="oldest">Oldest first</option>
    <option value="title">Alphabetical</option>
  </select>
</div>

<script>
  const searchInput = document.getElementById('search-input')
  const tagFilter = document.getElementById('tag-filter')
  const sortFilter = document.getElementById('sort-filter')
  const articles = document.querySelectorAll('[data-article]')

  function filterArticles() {
    const searchTerm = searchInput.value.toLowerCase()
    const selectedTag = tagFilter.value
    const sortBy = sortFilter.value

    let visibleArticles = Array.from(articles).filter(article => {
      const title = article.dataset.title.toLowerCase()
      const tags = article.dataset.tags.split(',')
      
      const matchesSearch = title.includes(searchTerm)
      const matchesTag = !selectedTag || tags.includes(selectedTag)
      
      return matchesSearch && matchesTag
    })

    // Sort articles
    visibleArticles.sort((a, b) => {
      switch (sortBy) {
        case 'oldest':
          return new Date(a.dataset.date) - new Date(b.dataset.date)
        case 'title':
          return a.dataset.title.localeCompare(b.dataset.title)
        default: // newest
          return new Date(b.dataset.date) - new Date(a.dataset.date)
      }
    })

    // Hide all articles
    articles.forEach(article => article.style.display = 'none')
    
    // Show filtered articles
    visibleArticles.forEach((article, index) => {
      article.style.display = 'block'
      article.style.order = index
    })
  }

  searchInput.addEventListener('input', filterArticles)
  tagFilter.addEventListener('change', filterArticles)
  sortFilter.addEventListener('change', filterArticles)
</script>

<style>
  .search-filters {
    display: flex;
    gap: 1rem;
    margin: 2rem 0;
    flex-wrap: wrap;
  }
  
  .search-input {
    flex: 1;
    min-width: 250px;
    padding: 0.75rem;
    border: 1px solid #d1d5db;
    border-radius: 0.5rem;
    font-size: 1rem;
  }
  
  .tag-filter,
  .sort-filter {
    padding: 0.75rem;
    border: 1px solid #d1d5db;
    border-radius: 0.5rem;
    background: white;
    font-size: 1rem;
  }
  
  @media (max-width: 768px) {
    .search-filters {
      flex-direction: column;
    }
    
    .search-input {
      min-width: auto;
    }
  }
</style>
```

---

## 📊 Analytics & Performance Monitoring

### Lightweight Analytics
```typescript
// src/utils/analytics.ts
interface AnalyticsEvent {
  event: string
  page: string
  timestamp: number
  userAgent?: string
  referrer?: string
}

export class Analytics {
  private static queue: AnalyticsEvent[] = []
  private static endpoint = '/api/analytics'

  static track(event: string, data: Record<string, any> = {}) {
    const analyticsEvent: AnalyticsEvent = {
      event,
      page: location.pathname,
      timestamp: Date.now(),
      userAgent: navigator.userAgent,
      referrer: document.referrer,
      ...data,
    }

    this.queue.push(analyticsEvent)
    this.flush()
  }

  static trackPageView() {
    this.track('page_view', {
      title: document.title,
      url: location.href,
    })
  }

  static trackClick(element: string) {
    this.track('click', { element })
  }

  static trackScroll(percentage: number) {
    this.track('scroll', { percentage })
  }

  private static async flush() {
    if (this.queue.length === 0) return

    const events = [...this.queue]
    this.queue = []

    try {
      if ('sendBeacon' in navigator) {
        navigator.sendBeacon(this.endpoint, JSON.stringify(events))
      } else {
        await fetch(this.endpoint, {
          method: 'POST',
          body: JSON.stringify(events),
          keepalive: true,
        })
      }
    } catch (error) {
      // Re-queue failed events
      this.queue.unshift(...events)
    }
  }
}

// Auto-track page views
if (typeof window !== 'undefined') {
  Analytics.trackPageView()
  
  // Track scroll depth
  let maxScroll = 0
  window.addEventListener('scroll', () => {
    const scrollPercent = Math.round(
      (window.scrollY / (document.body.scrollHeight - window.innerHeight)) * 100
    )
    
    if (scrollPercent > maxScroll && scrollPercent % 25 === 0) {
      maxScroll = scrollPercent
      Analytics.trackScroll(scrollPercent)
    }
  })
}
```

### Core Web Vitals Monitoring
```astro
---
// src/components/WebVitals.astro
---

<script>
  // Core Web Vitals monitoring
  import { getCLS, getFID, getFCP, getLCP, getTTFB } from 'web-vitals'

  function sendToAnalytics(metric) {
    fetch('/api/web-vitals', {
      method: 'POST',
      body: JSON.stringify({
        name: metric.name,
        value: metric.value,
        id: metric.id,
        url: location.pathname,
        timestamp: Date.now(),
      }),
      keepalive: true,
    })
  }

  getCLS(sendToAnalytics)
  getFID(sendToAnalytics)
  getFCP(sendToAnalytics)
  getLCP(sendToAnalytics)
  getTTFB(sendToAnalytics)
</script>
```

---

## 🚀 Deployment & CDN Optimization

### Astro Config for Maximum Performance
```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config'
import tailwind from '@astrojs/tailwind'
import mdx from '@astrojs/mdx'
import sitemap from '@astrojs/sitemap'
import compress from 'astro-compress'

export default defineConfig({
  site: 'https://my-blazing-site.com',
  integrations: [
    tailwind(),
    mdx(),
    sitemap(),
    compress({
      CSS: true,
      HTML: {
        removeAttributeQuotes: false,
        collapseWhitespace: true,
        removeComments: true,
      },
      Image: {
        webp: true,
        avif: true,
      },
      JavaScript: true,
      SVG: true,
    }),
  ],
  build: {
    inlineStylesheets: 'auto',
    assets: '_assets',
  },
  vite: {
    build: {
      rollupOptions: {
        output: {
          assetFileNames: 'assets/[name].[hash][extname]',
          chunkFileNames: 'assets/[name].[hash].js',
          entryFileNames: 'assets/[name].[hash].js',
        },
      },
    },
  },
  experimental: {
    contentCollectionCache: true,
    viewTransitions: true,
  },
})
```

### Netlify Deployment with Edge Functions
```toml
# netlify.toml
[build]
  publish = "dist"
  command = "npm run build"

[build.environment]
  NODE_VERSION = "18"

[[headers]]
  for = "/*"
  [headers.values]
    X-Frame-Options = "DENY"
    X-XSS-Protection = "1; mode=block"
    X-Content-Type-Options = "nosniff"
    Cache-Control = "public, max-age=31536000, immutable"

[[headers]]
  for = "*.html"
  [headers.values]
    Cache-Control = "public, max-age=0, must-revalidate"

[[headers]]
  for = "/api/*"
  [headers.values]
    Cache-Control = "public, max-age=300"

[[redirects]]
  from = "/blog/old-post"
  to = "/blog/new-post"
  status = 301

# Edge functions for dynamic content
[[edge_functions]]
  function = "analytics"
  path = "/api/analytics"
```

### Cloudflare Pages with Workers
```javascript
// functions/api/newsletter.js
export async function onRequestPost({ request, env }) {
  try {
    const { email } = await request.json()
    
    // Validate email
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    if (!emailRegex.test(email)) {
      return new Response(
        JSON.stringify({ error: 'Invalid email address' }),
        { status: 400, headers: { 'Content-Type': 'application/json' } }
      )
    }

    // Add to mailing list (replace with your service)
    const response = await fetch('https://api.mailchimp.com/3.0/lists/LIST_ID/members', {
      method: 'POST',
      headers: {
        'Authorization': `Bearer ${env.MAILCHIMP_API_KEY}`,
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        email_address: email,
        status: 'subscribed',
      }),
    })

    if (response.ok) {
      return new Response(
        JSON.stringify({ success: true }),
        { status: 200, headers: { 'Content-Type': 'application/json' } }
      )
    } else {
      throw new Error('Failed to subscribe')
    }
  } catch (error) {
    return new Response(
      JSON.stringify({ error: 'Subscription failed' }),
      { status: 500, headers: { 'Content-Type': 'application/json' } }
    )
  }
}
```

---

## 🐛 Performance Debugging & Optimization

### Bundle Size Analysis
```bash
# Install analyzer
npm install -D @astrojs/bundler-analyzer

# Analyze your build
npm run build -- --analyze
```

### Lighthouse CI Integration
```json
{
  "scripts": {
    "lighthouse": "lhci autorun",
    "lighthouse:mobile": "lhci autorun --config=lighthouse.mobile.json"
  }
}
```

```json
// lighthouse.config.json
{
  "ci": {
    "collect": {
      "staticDistDir": "./dist",
      "numberOfRuns": 3
    },
    "assert": {
      "assertions": {
        "categories:performance": ["warn", {"minScore": 0.9}],
        "categories:accessibility": ["error", {"minScore": 0.9}],
        "categories:best-practices": ["error", {"minScore": 0.9}],
        "categories:seo": ["error", {"minScore": 0.9}]
      }
    }
  }
}
```

### Performance Budget Monitoring
```javascript
// performance-budget.js
const fs = require('fs')
const path = require('path')

const BUDGET = {
  html: 50 * 1024,      // 50KB
  css: 100 * 1024,      // 100KB  
  js: 150 * 1024,       // 150KB
  images: 500 * 1024,   // 500KB per image
  fonts: 200 * 1024,    // 200KB total fonts
}

function checkBudget(distDir) {
  const violations = []
  
  // Check HTML files
  const htmlFiles = fs.readdirSync(distDir).filter(f => f.endsWith('.html'))
  htmlFiles.forEach(file => {
    const size = fs.statSync(path.join(distDir, file)).size
    if (size > BUDGET.html) {
      violations.push(`HTML file ${file} exceeds budget: ${size} bytes`)
    }
  })
  
  // Check CSS bundle
  const cssDir = path.join(distDir, '_astro')
  if (fs.existsSync(cssDir)) {
    const cssFiles = fs.readdirSync(cssDir).filter(f => f.endsWith('.css'))
    const totalCSS = cssFiles.reduce((total, file) => {
      return total + fs.statSync(path.join(cssDir, file)).size
    }, 0)
    
    if (totalCSS > BUDGET.css) {
      violations.push(`Total CSS exceeds budget: ${totalCSS} bytes`)
    }
  }
  
  if (violations.length > 0) {
    console.error('❌ Performance Budget Violations:')
    violations.forEach(v => console.error(`   ${v}`))
    process.exit(1)
  } else {
    console.log('✅ All files within performance budget!')
  }
}

checkBudget('./dist')
```
