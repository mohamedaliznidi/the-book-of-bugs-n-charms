---
description: >-
  Create production-ready full-stack applications with Next.js 13+ App Router. This comprehensive recipe covers server-side rendering, API routes, authentication, database integration with Prisma, form validation, SEO optimization, and deployment strategies for modern web applications.

theme: "magic"
---

# 🏰 Full-Stack Tome: Next.js Production Recipe

*When your app needs to rule both server and client realms*

---

## 🎯 Cast This When You Need

**SSR/SSG Magic** • **API Routes** • **SEO Dominance** • **Production Ready** • **Full-Stack Power**

Perfect for: Production apps, e-commerce, blogs, dashboards, SaaS platforms, anything that needs SEO

Terrible for: Simple SPAs, static sites, when you want minimal complexity

---

## 🚀 The Royal Summoning

```bash
# Summon the full-stack kingdom
npx create-next-app@latest my-fullstack-kingdom --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"

cd my-fullstack-kingdom
npm run dev
```

**Boom!** Your full-stack empire awaits at `http://localhost:3000`

---

## 🏗️ The Kingdom Architecture

```
src/
├── app/                 # App Router (Next.js 13+)
│   ├── (auth)/         # Route groups
│   ├── api/            # API routes
│   ├── globals.css     # Global styles
│   ├── layout.tsx      # Root layout
│   └── page.tsx        # Home page
├── components/         # Reusable UI components
│   ├── ui/            # Base components
│   └── forms/         # Form components
├── lib/               # Utilities & configurations
├── hooks/             # Custom React hooks
├── types/             # TypeScript definitions
└── middleware.ts      # Next.js middleware
```

---

## 🔮 Essential Production Enchantments

### Database Magic (Choose Your Path)

#### Option A: Prisma + PostgreSQL (Most Popular)
```bash
npm install prisma @prisma/client
npm install -D @types/bcryptjs bcryptjs
npx prisma init
```

```prisma
// prisma/schema.prisma
model User {
  id        String   @id @default(cuid())
  email     String   @unique
  name      String?
  posts     Post[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Post {
  id        String   @id @default(cuid())
  title     String
  content   String?
  published Boolean  @default(false)
  author    User     @relation(fields: [authorId], references: [id])
  authorId  String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}
```

#### Option B: Drizzle ORM (Type-Safe Speed)
```bash
npm install drizzle-orm postgres
npm install -D drizzle-kit @types/pg
```

### Authentication Sorcery
```bash
npm install next-auth @auth/prisma-adapter
```

```typescript
// src/lib/auth.ts
import NextAuth from 'next-auth'
import GoogleProvider from 'next-auth/providers/google'
import { PrismaAdapter } from '@auth/prisma-adapter'
import { prisma } from './prisma'

export const { handlers, auth, signIn, signOut } = NextAuth({
  adapter: PrismaAdapter(prisma),
  providers: [
    GoogleProvider({
      clientId: process.env.GOOGLE_CLIENT_ID!,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
    }),
  ],
  callbacks: {
    session: ({ session, user }) => ({
      ...session,
      user: {
        ...session.user,
        id: user.id,
      },
    }),
  },
})
```

### Form Validation Wizardry
```bash
npm install react-hook-form @hookform/resolvers zod
```

```typescript
// src/components/forms/CreatePostForm.tsx
'use client'
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { z } from 'zod'

const createPostSchema = z.object({
  title: z.string().min(1, 'Title is required').max(100),
  content: z.string().min(10, 'Content must be at least 10 characters'),
  published: z.boolean().default(false),
})

type CreatePostData = z.infer<typeof createPostSchema>

export function CreatePostForm() {
  const { register, handleSubmit, formState: { errors, isSubmitting } } = useForm<CreatePostData>({
    resolver: zodResolver(createPostSchema),
  })

  const onSubmit = async (data: CreatePostData) => {
    const response = await fetch('/api/posts', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(data),
    })
    
    if (response.ok) {
      // Handle success
    }
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <div>
        <input
          {...register('title')}
          placeholder="Post title"
          className="w-full p-2 border rounded"
        />
        {errors.title && <p className="text-red-500 text-sm">{errors.title.message}</p>}
      </div>
      
      <div>
        <textarea
          {...register('content')}
          placeholder="Write your post..."
          className="w-full p-2 border rounded h-32"
        />
        {errors.content && <p className="text-red-500 text-sm">{errors.content.message}</p>}
      </div>
      
      <label className="flex items-center gap-2">
        <input type="checkbox" {...register('published')} />
        <span>Publish immediately</span>
      </label>
      
      <button 
        type="submit" 
        disabled={isSubmitting}
        className="px-4 py-2 bg-blue-500 text-white rounded disabled:opacity-50"
      >
        {isSubmitting ? 'Creating...' : 'Create Post'}
      </button>
    </form>
  )
}
```

---

## 🛡️ API Routes Mastery

### RESTful API Pattern
```typescript
// src/app/api/posts/route.ts
import { auth } from '@/lib/auth'
import { prisma } from '@/lib/prisma'
import { NextRequest, NextResponse } from 'next/server'

export async function GET(request: NextRequest) {
  const { searchParams } = new URL(request.url)
  const page = parseInt(searchParams.get('page') ?? '1')
  const limit = parseInt(searchParams.get('limit') ?? '10')

  try {
    const posts = await prisma.post.findMany({
      skip: (page - 1) * limit,
      take: limit,
      include: { author: { select: { name: true, email: true } } },
      orderBy: { createdAt: 'desc' },
    })

    const total = await prisma.post.count()

    return NextResponse.json({
      posts,
      pagination: {
        page,
        limit,
        total,
        pages: Math.ceil(total / limit),
      },
    })
  } catch (error) {
    return NextResponse.json({ error: 'Failed to fetch posts' }, { status: 500 })
  }
}

export async function POST(request: NextRequest) {
  const session = await auth()
  
  if (!session?.user?.id) {
    return NextResponse.json({ error: 'Unauthorized' }, { status: 401 })
  }

  try {
    const body = await request.json()
    const post = await prisma.post.create({
      data: {
        title: body.title,
        content: body.content,
        published: body.published ?? false,
        authorId: session.user.id,
      },
    })

    return NextResponse.json(post, { status: 201 })
  } catch (error) {
    return NextResponse.json({ error: 'Failed to create post' }, { status: 500 })
  }
}
```

### Dynamic Route Handler
```typescript
// src/app/api/posts/[id]/route.ts
export async function GET(
  request: NextRequest,
  { params }: { params: { id: string } }
) {
  const post = await prisma.post.findUnique({
    where: { id: params.id },
    include: { author: true },
  })

  if (!post) {
    return NextResponse.json({ error: 'Post not found' }, { status: 404 })
  }

  return NextResponse.json(post)
}
```

---

## ⚡ Performance & SEO Superpowers

### Image Optimization Magic
```typescript
// src/components/OptimizedImage.tsx
import Image from 'next/image'

interface OptimizedImageProps {
  src: string
  alt: string
  width?: number
  height?: number
  priority?: boolean
}

export function OptimizedImage({ src, alt, width = 800, height = 600, priority = false }: OptimizedImageProps) {
  return (
    <Image
      src={src}
      alt={alt}
      width={width}
      height={height}
      priority={priority}
      placeholder="blur"
      blurDataURL="data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wBDAAYEBQYFBAYGBQYHBwYIChAKCgkJChQODwwQFxQYGBcUFhYaHSUfGhsjHBYWICwgIyYnKSopGR8tMC0oMCUoKSj/2wBDAQcHBwoIChMKChMoGhYaKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCj/wAARCAAIAAoDASIAAhEBAxEB/8QAFQABAQAAAAAAAAAAAAAAAAAAAAv/xAAhEAACAQMDBQAAAAAAAAAAAAABAgMABAUGIWGRkqGx0f/EABUBAQEAAAAAAAAAAAAAAAAAAAMF/8QAGhEAAgIDAAAAAAAAAAAAAAAAAAECEgMRkf/aAAwDAQACEQMRAD8AltJagyeH0AthI5xdrLcNM91BF5pX2HaH9bcfaSXWGaRmknybtN7E2eQdKu4kZGBhx5PY83xeHzMfBVYe2B1CfAM76/yaaX0xLTe5uvkOH3FRJtQokmGVMw3WA"
      className="rounded-lg shadow-md"
      style={{
        maxWidth: '100%',
        height: 'auto',
      }}
    />
  )
}
```

### Metadata for SEO Domination
```typescript
// src/app/posts/[slug]/page.tsx
import { Metadata } from 'next'

interface Post {
  id: string
  title: string
  content: string
  slug: string
}

export async function generateMetadata({ params }: { params: { slug: string } }): Promise<Metadata> {
  const post = await getPostBySlug(params.slug)
  
  return {
    title: post.title,
    description: post.content.substring(0, 160),
    openGraph: {
      title: post.title,
      description: post.content.substring(0, 160),
      type: 'article',
      url: `https://mysite.com/posts/${post.slug}`,
    },
    twitter: {
      card: 'summary_large_image',
      title: post.title,
      description: post.content.substring(0, 160),
    },
  }
}
```

### Server Actions (The Future is Now)
```typescript
// src/app/posts/actions.ts
'use server'

import { auth } from '@/lib/auth'
import { prisma } from '@/lib/prisma'
import { revalidatePath } from 'next/cache'
import { redirect } from 'next/navigation'

export async function createPost(formData: FormData) {
  const session = await auth()
  
  if (!session?.user?.id) {
    throw new Error('Unauthorized')
  }

  const title = formData.get('title') as string
  const content = formData.get('content') as string

  const post = await prisma.post.create({
    data: {
      title,
      content,
      authorId: session.user.id,
    },
  })

  revalidatePath('/posts')
  redirect(`/posts/${post.id}`)
}
```

---

## 🔐 Environment & Security Setup

```bash
# .env.local
DATABASE_URL="postgresql://username:password@localhost:5432/myapp"
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="your-super-secret-key"
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"
UPLOADTHING_SECRET="ut-secret-key"
UPLOADTHING_APP_ID="your-app-id"
```

### Middleware for Route Protection
```typescript
// src/middleware.ts
import { auth } from '@/lib/auth'
import { NextResponse } from 'next/server'

export default auth((req) => {
  const isLoggedIn = !!req.auth
  const isAuthPage = req.nextUrl.pathname.startsWith('/auth')
  const isProtectedRoute = req.nextUrl.pathname.startsWith('/dashboard')

  if (isProtectedRoute && !isLoggedIn) {
    return NextResponse.redirect(new URL('/auth/signin', req.url))
  }

  if (isAuthPage && isLoggedIn) {
    return NextResponse.redirect(new URL('/dashboard', req.url))
  }
})

export const config = {
  matcher: ['/((?!api|_next/static|_next/image|favicon.ico).*)'],
}
```

---

## 🚀 Production Deployment Mastery

### Vercel (Recommended)
```bash
npm i -g vercel
vercel --prod
```

### Docker Deployment
```dockerfile
# Dockerfile
FROM node:18-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine AS builder
WORKDIR /app
COPY . .
COPY --from=deps /app/node_modules ./node_modules
RUN npm run build

FROM node:18-alpine AS runner
WORKDIR /app
ENV NODE_ENV production
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nextjs -u 1001
COPY --from=builder --chown=nextjs:nodejs /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json
USER nextjs
EXPOSE 3000
CMD ["npm", "start"]
```

### Performance Monitoring
```typescript
// src/lib/analytics.ts
export function trackEvent(eventName: string, properties?: Record<string, any>) {
  if (typeof window !== 'undefined') {
    // Google Analytics
    gtag('event', eventName, properties)
    
    // Custom analytics
    fetch('/api/analytics', {
      method: 'POST',
      body: JSON.stringify({ event: eventName, properties }),
    })
  }
}
```

---

## 🐛 Common Production Pitfalls

### Hydration Mismatch Hell
**Problem:** Client/server render differently
**Fix:** Use `useEffect` for client-only code or `dynamic` imports

```typescript
import dynamic from 'next/dynamic'

const ClientOnlyComponent = dynamic(
  () => import('@/components/ClientOnlyComponent'),
  { ssr: false }
)
```

### Memory Leaks in API Routes
**Problem:** Database connections not closed
**Fix:** Use connection pooling and proper cleanup

```typescript
// Good: Connection pooling with Prisma
const prisma = new PrismaClient({
  datasources: {
    db: {
      url: process.env.DATABASE_URL,
    },
  },
})
```

### Bundle Size Explosion
**Fix:** Use `next/bundle-analyzer`
```bash
npm install -D @next/bundle-analyzer
```
