---
description: >-
  Complete guide to React Suspense and streaming patterns for better user
  experience. Learn how to implement loading states, error boundaries, and
  progressive rendering in modern React applications.
---

# Suspense and Streaming

## Introduction

React Suspense allows components to "wait" for something before rendering, providing a declarative way to handle loading states. Combined with streaming, it enables progressive rendering where parts of the UI can be displayed as soon as they're ready, improving perceived performance and user experience.

## Use Cases

1. **Progressive Loading**: Display content as it becomes available
2. **Better UX**: Provide smooth loading experiences without layout shifts
3. **Code Splitting**: Load components and routes on demand
4. **Data Fetching**: Handle async data loading with declarative loading states

## Basic Suspense Usage

### 1. Simple Suspense Boundary

```typescript
// app/posts/page.tsx
import { Suspense } from 'react'
import PostsList from './posts-list'

export default function PostsPage() {
  return (
    <div className="container mx-auto py-8">
      <h1 className="text-3xl font-bold mb-8">Blog Posts</h1>
      
      <Suspense fallback={<PostsLoading />}>
        <PostsList />
      </Suspense>
    </div>
  )
}

function PostsLoading() {
  return (
    <div className="space-y-4">
      {[...Array(3)].map((_, i) => (
        <div key={i} className="animate-pulse">
          <div className="h-4 bg-gray-200 rounded w-3/4 mb-2"></div>
          <div className="h-4 bg-gray-200 rounded w-1/2 mb-4"></div>
          <div className="h-20 bg-gray-200 rounded"></div>
        </div>
      ))}
    </div>
  )
}
```

### 2. Multiple Suspense Boundaries

```typescript
// app/dashboard/page.tsx
import { Suspense } from 'react'
import UserProfile from './user-profile'
import RecentActivity from './recent-activity'
import Analytics from './analytics'

export default function DashboardPage() {
  return (
    <div className="dashboard-grid">
      <header className="col-span-full">
        <h1 className="text-3xl font-bold">Dashboard</h1>
      </header>

      {/* Each section loads independently */}
      <section className="profile-section">
        <Suspense fallback={<ProfileSkeleton />}>
          <UserProfile />
        </Suspense>
      </section>

      <section className="activity-section">
        <Suspense fallback={<ActivitySkeleton />}>
          <RecentActivity />
        </Suspense>
      </section>

      <section className="analytics-section">
        <Suspense fallback={<AnalyticsSkeleton />}>
          <Analytics />
        </Suspense>
      </section>
    </div>
  )
}
```

## Streaming with Next.js

### 1. Route-Level Streaming

```typescript
// app/products/loading.tsx
export default function ProductsLoading() {
  return (
    <div className="container mx-auto py-8">
      <div className="animate-pulse">
        <div className="h-8 bg-gray-200 rounded w-1/4 mb-8"></div>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {[...Array(6)].map((_, i) => (
            <div key={i} className="border rounded-lg p-4">
              <div className="h-48 bg-gray-200 rounded mb-4"></div>
              <div className="h-4 bg-gray-200 rounded w-3/4 mb-2"></div>
              <div className="h-4 bg-gray-200 rounded w-1/2"></div>
            </div>
          ))}
        </div>
      </div>
    </div>
  )
}
```

### 2. Component-Level Streaming

```typescript
// app/blog/[slug]/page.tsx
import { Suspense } from 'react'
import { getPost } from '@/lib/database'
import Comments from './comments'
import RelatedPosts from './related-posts'

export default async function BlogPostPage({ params }: { params: { slug: string } }) {
  // This loads immediately
  const post = await getPost(params.slug)

  return (
    <article className="max-w-4xl mx-auto py-8">
      {/* Post content renders immediately */}
      <header className="mb-8">
        <h1 className="text-4xl font-bold mb-4">{post.title}</h1>
        <div className="text-gray-600">
          <span>By {post.author.name}</span>
          <span className="mx-2">•</span>
          <time>{new Date(post.publishedAt).toLocaleDateString()}</time>
        </div>
      </header>

      <div className="prose max-w-none mb-12">
        {post.content}
      </div>

      {/* These sections stream in as they're ready */}
      <div className="grid grid-cols-1 lg:grid-cols-3 gap-8">
        <div className="lg:col-span-2">
          <Suspense fallback={<CommentsSkeleton />}>
            <Comments postId={post.id} />
          </Suspense>
        </div>
        
        <aside>
          <Suspense fallback={<RelatedPostsSkeleton />}>
            <RelatedPosts postId={post.id} />
          </Suspense>
        </aside>
      </div>
    </article>
  )
}
```

## Advanced Streaming Patterns

### 1. Nested Suspense Boundaries

```typescript
// app/ecommerce/page.tsx
import { Suspense } from 'react'
import FeaturedProducts from './featured-products'
import ProductCategories from './product-categories'
import UserRecommendations from './user-recommendations'

export default function EcommercePage() {
  return (
    <div className="ecommerce-page">
      {/* Hero section loads immediately */}
      <section className="hero">
        <h1 className="text-5xl font-bold">Welcome to Our Store</h1>
        <p className="text-xl">Discover amazing products</p>
      </section>

      {/* Featured products stream first */}
      <section className="featured-section">
        <Suspense fallback={<FeaturedProductsSkeleton />}>
          <FeaturedProducts />
        </Suspense>
      </section>

      {/* Categories and recommendations stream independently */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
        <section>
          <Suspense fallback={<CategoriesSkeleton />}>
            <ProductCategories />
            
            {/* Nested suspense for subcategories */}
            <Suspense fallback={<SubcategoriesSkeleton />}>
              <PopularSubcategories />
            </Suspense>
          </Suspense>
        </section>

        <section>
          <Suspense fallback={<RecommendationsSkeleton />}>
            <UserRecommendations />
          </Suspense>
        </section>
      </div>
    </div>
  )
}
```

### 2. Conditional Suspense

```typescript
// app/profile/page.tsx
import { Suspense } from 'react'
import { auth } from '@/lib/auth'
import UserProfile from './user-profile'
import PublicProfile from './public-profile'

export default async function ProfilePage({ searchParams }: { searchParams: { userId?: string } }) {
  const session = await auth()
  const isOwnProfile = !searchParams.userId || searchParams.userId === session?.userId

  return (
    <div className="profile-page">
      {isOwnProfile ? (
        // Own profile - show detailed information
        <Suspense fallback={<DetailedProfileSkeleton />}>
          <UserProfile userId={session!.userId} />
        </Suspense>
      ) : (
        // Public profile - show limited information
        <Suspense fallback={<PublicProfileSkeleton />}>
          <PublicProfile userId={searchParams.userId!} />
        </Suspense>
      )}
    </div>
  )
}
```

## Error Handling with Suspense

### 1. Error Boundaries

```typescript
// app/posts/error.tsx
'use client'

import { useEffect } from 'react'
import { Button } from '@/components/ui/button'

export default function PostsError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  useEffect(() => {
    // Log error to monitoring service
    console.error('Posts error:', error)
  }, [error])

  return (
    <div className="error-container">
      <div className="text-center py-12">
        <h2 className="text-2xl font-bold text-red-600 mb-4">
          Oops! Something went wrong
        </h2>
        <p className="text-gray-600 mb-6">
          We couldn't load the posts. Please try again.
        </p>
        <div className="space-x-4">
          <Button onClick={reset} variant="default">
            Try Again
          </Button>
          <Button onClick={() => window.location.href = '/'} variant="outline">
            Go Home
          </Button>
        </div>
      </div>
    </div>
  )
}
```

### 2. Graceful Fallbacks

```typescript
// components/safe-suspense.tsx
import { Suspense, ReactNode } from 'react'

interface SafeSuspenseProps {
  children: ReactNode
  fallback: ReactNode
  errorFallback?: ReactNode
}

export function SafeSuspense({ children, fallback, errorFallback }: SafeSuspenseProps) {
  return (
    <Suspense fallback={fallback}>
      <ErrorBoundary fallback={errorFallback}>
        {children}
      </ErrorBoundary>
    </Suspense>
  )
}

function ErrorBoundary({ children, fallback }: { children: ReactNode; fallback?: ReactNode }) {
  // This would be implemented with a proper error boundary
  // For Next.js, use the error.tsx file pattern instead
  return <>{children}</>
}
```

## Performance Optimization

### 1. Skeleton Components

```typescript
// components/skeletons.tsx
export function PostSkeleton() {
  return (
    <div className="animate-pulse border rounded-lg p-6">
      <div className="h-4 bg-gray-200 rounded w-3/4 mb-2"></div>
      <div className="h-4 bg-gray-200 rounded w-1/2 mb-4"></div>
      <div className="space-y-2">
        <div className="h-3 bg-gray-200 rounded"></div>
        <div className="h-3 bg-gray-200 rounded w-5/6"></div>
        <div className="h-3 bg-gray-200 rounded w-4/6"></div>
      </div>
    </div>
  )
}

export function ProductGridSkeleton() {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {[...Array(6)].map((_, i) => (
        <div key={i} className="animate-pulse">
          <div className="h-48 bg-gray-200 rounded-lg mb-4"></div>
          <div className="h-4 bg-gray-200 rounded w-3/4 mb-2"></div>
          <div className="h-4 bg-gray-200 rounded w-1/2"></div>
        </div>
      ))}
    </div>
  )
}
```

### 2. Progressive Enhancement

```typescript
// app/search/page.tsx
import { Suspense } from 'react'
import SearchResults from './search-results'
import SearchFilters from './search-filters'

export default function SearchPage({ searchParams }: { searchParams: { q?: string } }) {
  const query = searchParams.q

  return (
    <div className="search-page">
      <header className="mb-8">
        <h1 className="text-3xl font-bold">
          {query ? `Search results for "${query}"` : 'Search'}
        </h1>
      </header>

      <div className="grid grid-cols-1 lg:grid-cols-4 gap-8">
        {/* Filters load immediately for better UX */}
        <aside className="lg:col-span-1">
          <SearchFilters />
        </aside>

        {/* Results stream in based on query */}
        <main className="lg:col-span-3">
          {query ? (
            <Suspense fallback={<SearchResultsSkeleton />}>
              <SearchResults query={query} />
            </Suspense>
          ) : (
            <div className="text-center py-12 text-gray-500">
              Enter a search term to see results
            </div>
          )}
        </main>
      </div>
    </div>
  )
}
```

## Best Practices

- **Granular Boundaries**: Use multiple small Suspense boundaries instead of one large one
- **Meaningful Fallbacks**: Create skeleton components that match the expected content layout
- **Error Handling**: Always provide error boundaries for robust user experience
- **Progressive Loading**: Load critical content first, then enhance with additional features
- **Avoid Waterfalls**: Structure data fetching to minimize sequential dependencies
- **Consistent Loading States**: Use consistent skeleton designs across your application

## Common Pitfalls

### Issue 1: Suspense Boundary Too High
Placing Suspense boundaries too high in the component tree blocks entire sections.

**Solution:**
Use granular Suspense boundaries:
```typescript
// ❌ Wrong - blocks entire page
<Suspense fallback={<PageSkeleton />}>
  <Header />
  <MainContent />
  <Sidebar />
</Suspense>

// ✅ Correct - granular loading
<Header />
<Suspense fallback={<ContentSkeleton />}>
  <MainContent />
</Suspense>
<Suspense fallback={<SidebarSkeleton />}>
  <Sidebar />
</Suspense>
```

### Issue 2: Poor Skeleton Design
Generic loading spinners don't provide good user experience.

**Solution:**
Create content-aware skeletons:
```typescript
// ❌ Wrong - generic spinner
<div className="spinner">Loading...</div>

// ✅ Correct - content-aware skeleton
<div className="animate-pulse">
  <div className="h-8 bg-gray-200 rounded w-1/4 mb-4"></div>
  <div className="h-4 bg-gray-200 rounded w-3/4 mb-2"></div>
  <div className="h-4 bg-gray-200 rounded w-1/2"></div>
</div>
```

### Issue 3: Missing Error Boundaries
Suspense without error handling leads to poor user experience.

**Solution:**
Always provide error boundaries:
```typescript
// Use Next.js error.tsx files or custom error boundaries
// app/posts/error.tsx
'use client'
export default function PostsError({ error, reset }) {
  return <ErrorDisplay error={error} onRetry={reset} />
}
```

## References

{% embed url="https://react.dev/reference/react/Suspense" %}

{% embed url="https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming" %}

{% embed url="https://nextjs.org/docs/app/building-your-application/routing/error-handling" %}
