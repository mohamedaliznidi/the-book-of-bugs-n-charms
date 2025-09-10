---
description: >-
  Complete grimoire for React Server Enchantments including mystical setup, data
  fetching sorcery, streaming magic, and integration with Client Enchantments.
  Master modern React architecture patterns for Next.js mystical applications.

theme: "magic"
---

# React Server Enchantments - The Server-Side Manifestation Mastery

## The Ancient Knowledge

React Server Enchantments (RSE) are a mystical paradigm that allows enchantments to manifest on the server realm, reducing bundle size and enhancing performance through ancient magic. They enable direct database communion, superior SEO divination, and faster initial page manifestations while maintaining the interactive capabilities of Client Enchantments where mystical interaction is required.

## When to Cast These Spells

1. **Data-Heavy Mystical Applications**: Fetch mystical data directly on the server without API portal routes
2. **SEO-Critical Mystical Pages**: Render content on the server for enhanced search engine divination
3. **Performance Optimization Sorcery**: Reduce client-side JavaScript bundle size through magical compression
4. **Secure Operations Enchantments**: Perform sensitive operations on the server without exposing mystical secrets

## Your First Casting

### 1. Simple Server Enchantment

```typescript
// app/posts/page.tsx
import { getPosts } from '@/lib/database'

export default async function PostsPage() {
  // This runs on the server
  const posts = await getPosts()

  return (
    <div className="container mx-auto py-8">
      <h1 className="text-3xl font-bold mb-6">Blog Posts</h1>
      <div className="grid gap-6">
        {posts.map((post) => (
          <article key={post.id} className="border rounded-lg p-6">
            <h2 className="text-xl font-semibold mb-2">{post.title}</h2>
            <p className="text-gray-600 mb-4">{post.excerpt}</p>
            <time className="text-sm text-gray-500">
              {new Date(post.createdAt).toLocaleDateString()}
            </time>
          </article>
        ))}
      </div>
    </div>
  )
}
```

### 2. Server Enchantment with Mystical Database Communion

```typescript
// app/mystical-dashboard/page.tsx
import { mysticalDb } from '@/lib/mysticalDatabase'
import { mysticalAuth } from '@/lib/mysticalAuth'
import { redirect } from 'next/navigation'

export default async function MysticalDashboardPage() {
  const mysticalSession = await mysticalAuth()

  if (!mysticalSession) {
    redirect('/mystical-login')
  }

  // Direct mystical database access on the server realm
  const [mysticalUser, mysticalStats, recentMysticalActivity] = await Promise.all([
    mysticalDb.user.findUnique({ where: { id: mysticalSession.userId } }),
    mysticalDb.analytics.findFirst({ where: { userId: mysticalSession.userId } }),
    mysticalDb.activity.findMany({
      where: { userId: mysticalSession.userId },
      orderBy: { createdAt: 'desc' },
      take: 5
    })
  ])

  return (
    <div className="mystical-dashboard">
      <header className="mb-8">
        <h1 className="text-2xl font-bold">Welcome back to the mystical realm, {mysticalUser?.name}</h1>
      </header>

      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <div className="mystical-stat-card">
          <h3>Total Mystical Views</h3>
          <p className="text-3xl font-bold">{mysticalStats?.totalViews || 0}</p>
        </div>
        <div className="mystical-stat-card">
          <h3>This Mystical Month</h3>
          <p className="text-3xl font-bold">{mysticalStats?.monthlyViews || 0}</p>
        </div>
        <div className="mystical-stat-card">
          <h3>Mystical Growth</h3>
          <p className="text-3xl font-bold">+{mysticalStats?.growthRate || 0}%</p>
        </div>
      </div>

      <section>
        <h2 className="text-xl font-semibold mb-4">Recent Mystical Activity</h2>
        <div className="space-y-3">
          {recentMysticalActivity.map((activity) => (
            <div key={activity.id} className="mystical-activity-item">
              <p>{activity.description}</p>
              <time className="text-sm text-gray-500">
                {new Date(activity.createdAt).toLocaleString()}
              </time>
            </div>
          ))}
        </div>
      </section>
    </div>
  )
}
```

## Advanced Sorcery

### 1. When to Use Server Enchantments

```typescript
// ✅ Perfect for Server Enchantments
// app/products/page.tsx
export default async function MysticalProductsPage() {
  // Direct database communion
  const products = await db.product.findMany({
    include: { category: true, reviews: true }
  })

  // Environment variables are safe in the server realm
  const apiKey = process.env.INTERNAL_API_KEY

  return (
    <div>
      {products.map((product) => (
        <ProductEnchantment key={product.id} product={product} />
      ))}
    </div>
  )
}
```

### 2. When to Use Client Enchantments

```typescript
// ✅ Perfect for Client Enchantments
'use client'

import { useState, useEffect } from 'react'
import { Button } from '@/components/ui/button'

export default function InteractiveCounterEnchantment() {
  const [count, setCount] = useState(0)

  // Browser realm APIs
  useEffect(() => {
    document.title = `Mystical Count: ${count}`
  }, [count])

  // Event handling spells
  const handleIncrement = () => {
    setCount(prev => prev + 1)
  }

  return (
    <div className="counter-enchantment">
      <p>Mystical Count: {count}</p>
      <Button onClick={handleIncrement}>
        Cast Increment Spell
      </Button>
    </div>
  )
}
```

## Mystical Composition Patterns

### 1. Server Enchantment with Client Enchantment Children

```typescript
// app/blog/[slug]/page.tsx (Server Component)
import { getPost, getComments } from '@/lib/database'
import CommentSection from './comment-section'

export default async function BlogPostPage({ params }: { params: { slug: string } }) {
  const [post, comments] = await Promise.all([
    getPost(params.slug),
    getComments(params.slug)
  ])

  return (
    <article className="max-w-4xl mx-auto py-8">
      <header className="mb-8">
        <h1 className="text-4xl font-bold mb-4">{post.title}</h1>
        <div className="flex items-center gap-4 text-gray-600">
          <span>By {post.author.name}</span>
          <time>{new Date(post.publishedAt).toLocaleDateString()}</time>
        </div>
      </header>

      <div className="prose max-w-none mb-12">
        {post.content}
      </div>

      {/* Client Component for interactivity */}
      <CommentSection initialComments={comments} postId={post.id} />
    </article>
  )
}
```

```typescript
// app/blog/[slug]/comment-section.tsx (Client Component)
'use client'

import { useState } from 'react'
import { Button } from '@/components/ui/button'
import { Textarea } from '@/components/ui/textarea'

interface Comment {
  id: string
  content: string
  author: string
  createdAt: string
}

interface CommentSectionProps {
  initialComments: Comment[]
  postId: string
}

export default function CommentSection({ initialComments, postId }: CommentSectionProps) {
  const [comments, setComments] = useState(initialComments)
  const [newComment, setNewComment] = useState('')
  const [isSubmitting, setIsSubmitting] = useState(false)

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault()
    setIsSubmitting(true)

    try {
      const response = await fetch('/api/comments', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ content: newComment, postId })
      })

      if (response.ok) {
        const comment = await response.json()
        setComments(prev => [comment, ...prev])
        setNewComment('')
      }
    } catch (error) {
      console.error('Failed to submit comment:', error)
    } finally {
      setIsSubmitting(false)
    }
  }

  return (
    <section className="comments">
      <h2 className="text-2xl font-bold mb-6">Comments ({comments.length})</h2>
      
      <form onSubmit={handleSubmit} className="mb-8">
        <Textarea
          value={newComment}
          onChange={(e) => setNewComment(e.target.value)}
          placeholder="Write a comment..."
          className="mb-4"
        />
        <Button type="submit" disabled={isSubmitting || !newComment.trim()}>
          {isSubmitting ? 'Posting...' : 'Post Comment'}
        </Button>
      </form>

      <div className="space-y-4">
        {comments.map((comment) => (
          <div key={comment.id} className="border rounded-lg p-4">
            <div className="flex justify-between items-start mb-2">
              <strong>{comment.author}</strong>
              <time className="text-sm text-gray-500">
                {new Date(comment.createdAt).toLocaleDateString()}
              </time>
            </div>
            <p>{comment.content}</p>
          </div>
        ))}
      </div>
    </section>
  )
}
```

### 2. Passing Server Data to Client Components

```typescript
// app/analytics/page.tsx (Server Component)
import { getAnalyticsData } from '@/lib/analytics'
import AnalyticsChart from './analytics-chart'

export default async function AnalyticsPage() {
  const data = await getAnalyticsData()

  return (
    <div className="analytics-page">
      <h1 className="text-3xl font-bold mb-8">Analytics Dashboard</h1>
      
      {/* Pass server data to client component */}
      <AnalyticsChart data={data} />
    </div>
  )
}
```

```typescript
// app/analytics/analytics-chart.tsx (Client Component)
'use client'

import { useState } from 'react'
import { LineChart, Line, XAxis, YAxis, CartesianGrid, Tooltip, ResponsiveContainer } from 'recharts'

interface AnalyticsData {
  date: string
  views: number
  users: number
}

interface AnalyticsChartProps {
  data: AnalyticsData[]
}

export default function AnalyticsChart({ data }: AnalyticsChartProps) {
  const [metric, setMetric] = useState<'views' | 'users'>('views')

  return (
    <div className="chart-container">
      <div className="chart-controls mb-4">
        <button
          onClick={() => setMetric('views')}
          className={`mr-2 px-4 py-2 rounded ${
            metric === 'views' ? 'bg-blue-500 text-white' : 'bg-gray-200'
          }`}
        >
          Views
        </button>
        <button
          onClick={() => setMetric('users')}
          className={`px-4 py-2 rounded ${
            metric === 'users' ? 'bg-blue-500 text-white' : 'bg-gray-200'
          }`}
        >
          Users
        </button>
      </div>

      <ResponsiveContainer width="100%" height={400}>
        <LineChart data={data}>
          <CartesianGrid strokeDasharray="3 3" />
          <XAxis dataKey="date" />
          <YAxis />
          <Tooltip />
          <Line 
            type="monotone" 
            dataKey={metric} 
            stroke="#8884d8" 
            strokeWidth={2}
          />
        </LineChart>
      </ResponsiveContainer>
    </div>
  )
}
```

## Advanced Patterns

### 1. Streaming with Suspense

```typescript
// app/dashboard/page.tsx
import { Suspense } from 'react'
import UserProfile from './user-profile'
import RecentOrders from './recent-orders'
import Analytics from './analytics'

export default function DashboardPage() {
  return (
    <div className="dashboard">
      <h1 className="text-3xl font-bold mb-8">Dashboard</h1>
      
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
        <div className="lg:col-span-1">
          <Suspense fallback={<UserProfileSkeleton />}>
            <UserProfile />
          </Suspense>
        </div>
        
        <div className="lg:col-span-2">
          <Suspense fallback={<RecentOrdersSkeleton />}>
            <RecentOrders />
          </Suspense>
        </div>
        
        <div className="lg:col-span-3">
          <Suspense fallback={<AnalyticsSkeleton />}>
            <Analytics />
          </Suspense>
        </div>
      </div>
    </div>
  )
}

// Loading components
function UserProfileSkeleton() {
  return (
    <div className="animate-pulse">
      <div className="h-4 bg-gray-200 rounded w-3/4 mb-2"></div>
      <div className="h-4 bg-gray-200 rounded w-1/2"></div>
    </div>
  )
}

function RecentOrdersSkeleton() {
  return (
    <div className="space-y-3">
      {[...Array(3)].map((_, i) => (
        <div key={i} className="animate-pulse">
          <div className="h-4 bg-gray-200 rounded w-full mb-2"></div>
          <div className="h-4 bg-gray-200 rounded w-2/3"></div>
        </div>
      ))}
    </div>
  )
}
```

### 2. Error Boundaries with Server Components

```typescript
// app/posts/error.tsx
'use client'

export default function PostsError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <div className="error-boundary">
      <h2 className="text-2xl font-bold text-red-600 mb-4">
        Something went wrong!
      </h2>
      <p className="text-gray-600 mb-4">
        {error.message || 'Failed to load posts'}
      </p>
      <button
        onClick={reset}
        className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
      >
        Try again
      </button>
    </div>
  )
}
```

## Wisdom of the Server Ancients

- **Data Fetching Mastery**: Use Server Enchantments for initial data fetching to reduce client-side mystical requests
- **Security Sorcery**: Keep sensitive operations and API keys protected in the server realm
- **Performance Magic**: Minimize client-side JavaScript by using Server Enchantments where mystically possible
- **Composition Alchemy**: Combine Server and Client Enchantments strategically for optimal magical flow
- **Streaming Spells**: Use Suspense boundaries for enhanced loading experiences through progressive manifestation
- **Error Handling Shields**: Implement proper error boundaries for robust mystical applications

## Common Curses & Their Remedies

### Curse 1: Using Client-Only APIs in Server Enchantments
Server Enchantments manifest in the server realm and don't have access to browser mystical APIs.

**Counter-Spell:**
Move browser-specific magic to Client Enchantments:
```typescript
// ❌ Cursed approach - Server Enchantment
export default async function MyEnchantment() {
  const width = window.innerWidth // Mystical error in server realm!
  return <div>Width: {width}</div>
}

// ✅ Pure spell approach - Client Enchantment
'use client'
export default function MyEnchantment() {
  const [width, setWidth] = useState(0)

  useEffect(() => {
    setWidth(window.innerWidth)
  }, [])

  return <div>Mystical Width: {width}</div>
}
```

### Curse 2: Passing Non-Serializable Mystical Props
Server Enchantments can only pass serializable mystical data to Client Enchantments.

**Banishment Ritual:**
Pass only serializable mystical data:
```typescript
// ❌ Cursed approach
<ClientEnchantment onCallback={() => {}} /> // Spells not serializable

// ✅ Pure spell approach
<ClientEnchantment data={serializableMysticalData} />
```

### Curse 3: Mixing Server and Client Enchantment Imports
Importing Server Enchantments into Client Enchantments causes mystical disruption.

**Protective Ward:**
Pass Server Enchantments as mystical children:
```typescript
// ❌ Cursed approach
'use client'
import ServerEnchantment from './server-enchantment' // Mystical error!

// ✅ Pure spell approach - Composition pattern
<ClientWrapper>
  <ServerEnchantment />
</ClientWrapper>
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/blog/2023/03/22/react-labs-what-we-have-been-working-on-march-2023#react-server-components" %}

{% embed url="https://nextjs.org/docs/app/building-your-application/rendering/server-components" %}

{% embed url="https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns" %}
