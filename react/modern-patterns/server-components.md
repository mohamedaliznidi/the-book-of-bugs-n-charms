---
description: >-
  Complete guide to React Server Components including setup, data fetching,
  streaming, and integration with Client Components. Learn modern React
  architecture patterns for Next.js applications.
---

# React Server Components

## Introduction

React Server Components (RSC) are a new paradigm that allows components to render on the server, reducing bundle size and improving performance. They enable direct database access, better SEO, and faster initial page loads while maintaining the interactive capabilities of Client Components where needed.

## Use Cases

1. **Data-Heavy Applications**: Fetch data directly on the server without API routes
2. **SEO-Critical Pages**: Render content on the server for better search engine optimization
3. **Performance Optimization**: Reduce client-side JavaScript bundle size
4. **Secure Operations**: Perform sensitive operations on the server without exposing secrets

## Basic Server Component

### 1. Simple Server Component

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

### 2. Server Component with Database Access

```typescript
// app/dashboard/page.tsx
import { db } from '@/lib/database'
import { auth } from '@/lib/auth'
import { redirect } from 'next/navigation'

export default async function DashboardPage() {
  const session = await auth()
  
  if (!session) {
    redirect('/login')
  }

  // Direct database access on the server
  const [user, stats, recentActivity] = await Promise.all([
    db.user.findUnique({ where: { id: session.userId } }),
    db.analytics.findFirst({ where: { userId: session.userId } }),
    db.activity.findMany({ 
      where: { userId: session.userId },
      orderBy: { createdAt: 'desc' },
      take: 5
    })
  ])

  return (
    <div className="dashboard">
      <header className="mb-8">
        <h1 className="text-2xl font-bold">Welcome back, {user?.name}</h1>
      </header>
      
      <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-8">
        <div className="stat-card">
          <h3>Total Views</h3>
          <p className="text-3xl font-bold">{stats?.totalViews || 0}</p>
        </div>
        <div className="stat-card">
          <h3>This Month</h3>
          <p className="text-3xl font-bold">{stats?.monthlyViews || 0}</p>
        </div>
        <div className="stat-card">
          <h3>Growth</h3>
          <p className="text-3xl font-bold">+{stats?.growthRate || 0}%</p>
        </div>
      </div>

      <section>
        <h2 className="text-xl font-semibold mb-4">Recent Activity</h2>
        <div className="space-y-3">
          {recentActivity.map((activity) => (
            <div key={activity.id} className="activity-item">
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

## Client vs Server Components

### 1. When to Use Server Components

```typescript
// ✅ Good for Server Components
// app/products/page.tsx
export default async function ProductsPage() {
  // Direct database access
  const products = await db.product.findMany({
    include: { category: true, reviews: true }
  })

  // Environment variables are safe here
  const apiKey = process.env.INTERNAL_API_KEY

  return (
    <div>
      {products.map((product) => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  )
}
```

### 2. When to Use Client Components

```typescript
// ✅ Good for Client Components
'use client'

import { useState, useEffect } from 'react'
import { Button } from '@/components/ui/button'

export default function InteractiveCounter() {
  const [count, setCount] = useState(0)

  // Browser APIs
  useEffect(() => {
    document.title = `Count: ${count}`
  }, [count])

  // Event handlers
  const handleIncrement = () => {
    setCount(prev => prev + 1)
  }

  return (
    <div className="counter">
      <p>Count: {count}</p>
      <Button onClick={handleIncrement}>
        Increment
      </Button>
    </div>
  )
}
```

## Composition Patterns

### 1. Server Component with Client Component Children

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

## Best Practices

- **Data Fetching**: Use Server Components for initial data fetching to reduce client-side requests
- **Security**: Keep sensitive operations and API keys on the server
- **Performance**: Minimize client-side JavaScript by using Server Components where possible
- **Composition**: Combine Server and Client Components strategically
- **Streaming**: Use Suspense boundaries for better loading experiences
- **Error Handling**: Implement proper error boundaries for robust applications

## Common Pitfalls

### Issue 1: Using Client-Only APIs in Server Components
Server Components run on the server and don't have access to browser APIs.

**Solution:**
Move browser-specific code to Client Components:
```typescript
// ❌ Wrong - Server Component
export default async function MyComponent() {
  const width = window.innerWidth // Error!
  return <div>Width: {width}</div>
}

// ✅ Correct - Client Component
'use client'
export default function MyComponent() {
  const [width, setWidth] = useState(0)
  
  useEffect(() => {
    setWidth(window.innerWidth)
  }, [])
  
  return <div>Width: {width}</div>
}
```

### Issue 2: Passing Non-Serializable Props
Server Components can only pass serializable data to Client Components.

**Solution:**
Pass only serializable data:
```typescript
// ❌ Wrong
<ClientComponent onCallback={() => {}} /> // Functions not serializable

// ✅ Correct
<ClientComponent data={serializableData} />
```

### Issue 3: Mixing Server and Client Component Imports
Importing Server Components into Client Components causes issues.

**Solution:**
Pass Server Components as children:
```typescript
// ❌ Wrong
'use client'
import ServerComponent from './server-component' // Error!

// ✅ Correct - Composition pattern
<ClientWrapper>
  <ServerComponent />
</ClientWrapper>
```

## References

{% embed url="https://react.dev/blog/2023/03/22/react-labs-what-we-have-been-working-on-march-2023#react-server-components" %}

{% embed url="https://nextjs.org/docs/app/building-your-application/rendering/server-components" %}

{% embed url="https://nextjs.org/docs/app/building-your-application/rendering/composition-patterns" %}
