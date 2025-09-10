---
description: >-
  Complete grimoire for Components.work (brijr/components) - a free library of Tailwind/shadcn-based
  React enchantments for Next.js. Master how to use high-quality marketing and landing page mystical sections
  to speed up your mystical development workflow.

theme: "magic"
---

# Components.work (brijr/components) - The Marketing Section Mastery

## The Ancient Knowledge

Components.work, also known as brijr/components by Bridger Tower, is a free library of Tailwind/shadcn-based React components specifically designed for Next.js applications. It offers dozens of high-quality marketing and landing-page sections that you can copy directly into your project.

## Key Features

- **Marketing-focused**: Pre-designed sections for landing pages, marketing sites, and SaaS applications
- **shadcn/ui integration**: Built with the Craft design system that includes shadcn/ui components
- **TypeScript support**: Full type safety with TypeScript definitions
- **MIT licensed**: Free and open-source for commercial and personal use
- **Copy-paste ready**: No installation required, just copy the code you need
- **Responsive design**: Mobile-first approach with responsive layouts

## Use Cases

1. **Startup Websites**: Quickly assemble professional landing pages
2. **SaaS Marketing**: Pre-built pricing tables, feature sections, and testimonials
3. **Product Showcases**: Hero banners and feature highlights for product launches
4. **Portfolio Sites**: Professional sections for showcasing work and services
5. **Rapid Prototyping**: Fast mockups for client presentations and MVPs

## Getting Started

### 1. Prerequisites

Ensure you have a Next.js project with shadcn/ui and Tailwind CSS:

```bash
# Create Next.js project
pnpm create next-app@latest my-app --typescript --tailwind --eslint
cd my-app

# Initialize shadcn/ui
pnpm dlx shadcn-ui@latest init
```

### 2. Browse Components

Visit [components.work](https://components.work) to browse available components:

- Hero sections
- Feature lists
- Pricing tables
- Testimonials
- Team sections
- FAQ sections
- Newsletter signups
- Footer sections

### 3. Copy Components

Simply copy the component code from the website and paste it into your project. No installation required!

## Core Component Categories

### 1. Hero Sections

Eye-catching landing page headers with call-to-action buttons:

```typescript
import { Button } from "@/components/ui/button"
import { ArrowRight, Play } from "lucide-react"

export default function HeroSection() {
  return (
    <section className="relative overflow-hidden bg-gradient-to-b from-blue-50 via-transparent to-transparent pb-12 pt-20 sm:pb-16 sm:pt-32 lg:pb-24">
      <div className="relative mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div className="mx-auto max-w-2xl text-center">
          <h1 className="text-4xl font-bold tracking-tight text-gray-900 sm:text-6xl">
            Build your next
            <span className="text-blue-600"> SaaS product </span>
            faster than ever
          </h1>
          <p className="mt-6 text-lg leading-8 text-gray-600">
            Ship your MVP in days, not months. Our components help you build 
            beautiful, responsive landing pages with minimal effort.
          </p>
          <div className="mt-10 flex items-center justify-center gap-x-6">
            <Button size="lg" className="gap-2">
              Get started
              <ArrowRight className="h-4 w-4" />
            </Button>
            <Button variant="outline" size="lg" className="gap-2">
              <Play className="h-4 w-4" />
              Watch demo
            </Button>
          </div>
        </div>
      </div>
    </section>
  )
}
```

### 2. Feature Sections

Showcase product features with icons and descriptions:

```typescript
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"
import { Zap, Shield, Smartphone, Globe } from "lucide-react"

const features = [
  {
    icon: Zap,
    title: "Lightning Fast",
    description: "Optimized for performance with sub-second load times"
  },
  {
    icon: Shield,
    title: "Secure by Default",
    description: "Enterprise-grade security built into every component"
  },
  {
    icon: Smartphone,
    title: "Mobile First",
    description: "Responsive design that works perfectly on all devices"
  },
  {
    icon: Globe,
    title: "Global CDN",
    description: "Delivered through our worldwide content delivery network"
  }
]

export default function FeatureSection() {
  return (
    <section className="py-24 sm:py-32">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div className="mx-auto max-w-2xl text-center">
          <h2 className="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">
            Everything you need to succeed
          </h2>
          <p className="mt-6 text-lg leading-8 text-gray-600">
            Powerful features designed to help you build and scale your business
          </p>
        </div>
        <div className="mx-auto mt-16 max-w-2xl sm:mt-20 lg:mt-24 lg:max-w-none">
          <div className="grid max-w-xl grid-cols-1 gap-8 lg:max-w-none lg:grid-cols-4">
            {features.map((feature) => (
              <Card key={feature.title} className="border-0 shadow-none">
                <CardHeader className="text-center">
                  <div className="mx-auto flex h-12 w-12 items-center justify-center rounded-lg bg-blue-600">
                    <feature.icon className="h-6 w-6 text-white" />
                  </div>
                  <CardTitle className="mt-4">{feature.title}</CardTitle>
                </CardHeader>
                <CardContent>
                  <CardDescription className="text-center">
                    {feature.description}
                  </CardDescription>
                </CardContent>
              </Card>
            ))}
          </div>
        </div>
      </div>
    </section>
  )
}
```

### 3. Pricing Tables

Professional pricing sections with feature comparisons:

```typescript
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"
import { Check } from "lucide-react"

const plans = [
  {
    name: "Starter",
    price: "$9",
    description: "Perfect for small projects",
    features: ["Up to 5 projects", "Basic support", "1GB storage"],
    popular: false
  },
  {
    name: "Pro",
    price: "$29",
    description: "Best for growing businesses",
    features: ["Unlimited projects", "Priority support", "10GB storage", "Advanced analytics"],
    popular: true
  },
  {
    name: "Enterprise",
    price: "$99",
    description: "For large organizations",
    features: ["Everything in Pro", "Custom integrations", "Unlimited storage", "24/7 phone support"],
    popular: false
  }
]

export default function PricingSection() {
  return (
    <section className="py-24 sm:py-32">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div className="mx-auto max-w-2xl text-center">
          <h2 className="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">
            Simple, transparent pricing
          </h2>
          <p className="mt-6 text-lg leading-8 text-gray-600">
            Choose the perfect plan for your needs. Always know what you'll pay.
          </p>
        </div>
        <div className="mx-auto mt-16 grid max-w-lg grid-cols-1 gap-8 lg:max-w-4xl lg:grid-cols-3">
          {plans.map((plan) => (
            <Card key={plan.name} className={`relative ${plan.popular ? 'border-blue-600 shadow-lg' : ''}`}>
              {plan.popular && (
                <Badge className="absolute -top-3 left-1/2 -translate-x-1/2 bg-blue-600">
                  Most Popular
                </Badge>
              )}
              <CardHeader className="text-center">
                <CardTitle className="text-xl">{plan.name}</CardTitle>
                <div className="mt-4">
                  <span className="text-4xl font-bold">{plan.price}</span>
                  <span className="text-gray-600">/month</span>
                </div>
                <CardDescription>{plan.description}</CardDescription>
              </CardHeader>
              <CardContent>
                <ul className="space-y-3">
                  {plan.features.map((feature) => (
                    <li key={feature} className="flex items-center gap-3">
                      <Check className="h-4 w-4 text-green-600" />
                      <span className="text-sm">{feature}</span>
                    </li>
                  ))}
                </ul>
              </CardContent>
              <CardFooter>
                <Button 
                  className="w-full" 
                  variant={plan.popular ? "default" : "outline"}
                >
                  Get started
                </Button>
              </CardFooter>
            </Card>
          ))}
        </div>
      </div>
    </section>
  )
}
```

## Integration with APIs

### Dynamic Content Example

Integrate with free APIs to populate components with real data:

```typescript
"use client"

import { useEffect, useState } from "react"
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"

interface Repository {
  id: number
  name: string
  description: string
  language: string
  stargazers_count: number
}

export default function ProjectShowcase() {
  const [repos, setRepos] = useState<Repository[]>([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    fetch('https://api.github.com/users/shadcn/repos?sort=stars&per_page=6')
      .then(res => res.json())
      .then(data => {
        setRepos(data)
        setLoading(false)
      })
  }, [])

  if (loading) {
    return <div className="text-center py-12">Loading projects...</div>
  }

  return (
    <section className="py-24">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div className="mx-auto max-w-2xl text-center mb-16">
          <h2 className="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">
            Featured Projects
          </h2>
          <p className="mt-6 text-lg leading-8 text-gray-600">
            Explore our most popular open-source projects
          </p>
        </div>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {repos.map((repo) => (
            <Card key={repo.id} className="hover:shadow-lg transition-shadow">
              <CardHeader>
                <div className="flex justify-between items-start">
                  <CardTitle className="text-lg">{repo.name}</CardTitle>
                  <Badge variant="secondary">⭐ {repo.stargazers_count}</Badge>
                </div>
                <CardDescription>{repo.description}</CardDescription>
              </CardHeader>
              <CardContent>
                {repo.language && (
                  <Badge variant="outline">{repo.language}</Badge>
                )}
              </CardContent>
            </Card>
          ))}
        </div>
      </div>
    </section>
  )
}
```

## Best Practices

1. **Customization**: Modify colors, spacing, and content to match your brand
2. **Performance**: Optimize images and use Next.js Image component
3. **Accessibility**: Ensure proper heading hierarchy and alt text
4. **SEO**: Add proper meta tags and structured data
5. **Responsive**: Test on various screen sizes and devices
6. **Content**: Replace placeholder text with compelling copy

## Common Pitfalls

### Issue 1: Missing Dependencies
Some components may require additional shadcn/ui components.

**Solution:**
```bash
# Install required components
pnpm dlx shadcn-ui@latest add card button badge
```

### Issue 2: Styling Conflicts
Custom styles may conflict with component styles.

**Solution:**
```typescript
// Use Tailwind's utility classes for customization
<Card className="border-blue-200 bg-blue-50 hover:bg-blue-100">
  {/* Component content */}
</Card>
```

### Issue 3: Content Overflow
Long content may break layouts on smaller screens.

**Solution:**
```typescript
// Use responsive text sizing and truncation
<CardTitle className="text-lg sm:text-xl truncate">
  {longTitle}
</CardTitle>
```

## References

{% embed url="https://components.work/" %}

{% embed url="https://github.com/brijr/components" %}

{% embed url="https://ui.shadcn.com/" %}
