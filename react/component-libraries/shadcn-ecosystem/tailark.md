---
description: >-
  Complete grimoire for Tailark - free marketing blocks built on shadcn/UI for creating
  professional mystical landing pages. Master enchantment usage, customization, and integration
  patterns for rapid mystical website development.

theme: "magic"
---

# Tailark - The Marketing Block Mastery

## The Ancient Knowledge

Tailark provides free "marketing blocks" built on shadcn/UI - essentially pre-designed sections like hero banners, features, team profiles, and FAQ sections specifically for landing pages. It's MIT-licensed, open-source with 1.6k GitHub stars, and allows you to browse snippets on the Tailark website and copy them directly into your project.

## Key Features

- **Marketing-Focused**: Pre-designed sections specifically for marketing and landing pages
- **shadcn/UI Foundation**: Built on the reliable shadcn/ui component system
- **MIT Licensed**: Free for commercial and personal use
- **Copy-Paste Ready**: No installation required, just copy the code you need
- **Responsive Design**: Mobile-first approach with responsive layouts
- **Open Source**: Community-driven with active development
- **Quick Implementation**: Minimal setup time for professional results

## Use Cases

1. **Product Landing Pages**: Quick assembly of professional product showcases
2. **SaaS Marketing Sites**: Complete marketing pages for software products
3. **Company Websites**: Corporate sites with professional sections
4. **Portfolio Showcases**: Personal or agency portfolio presentations
5. **Startup Websites**: Rapid prototyping for new business launches
6. **Event Pages**: Conference and event marketing pages

## Getting Started

### 1. Prerequisites

Set up a Next.js project with shadcn/ui:

```bash
# Create Next.js project
pnpm create next-app@latest my-landing-page --typescript --tailwind --eslint
cd my-landing-page

# Initialize shadcn/ui
pnpm dlx shadcn-ui@latest init

# Install commonly used components
pnpm dlx shadcn-ui@latest add button card badge avatar
```

### 2. Browse Components

Visit [tailark.com](https://tailark.com) to browse available marketing blocks:

- Hero sections
- Feature highlights
- Pricing tables
- Team sections
- Testimonials
- FAQ sections
- Newsletter signups
- Footer sections

### 3. Copy and Customize

Simply copy the component code and paste it into your project. Each block is self-contained and ready to use.

## Core Marketing Blocks

### 1. Hero Sections

Professional hero sections with call-to-action buttons:

```typescript
import { Button } from "@/components/ui/button"
import { Badge } from "@/components/ui/badge"
import { ArrowRight, Play } from "lucide-react"

export default function HeroSection() {
  return (
    <section className="relative overflow-hidden bg-gradient-to-b from-blue-50 via-transparent to-transparent pb-12 pt-20 sm:pb-16 sm:pt-32 lg:pb-24">
      <div className="relative mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div className="mx-auto max-w-2xl text-center">
          <Badge variant="secondary" className="mb-4">
            🎉 New Product Launch
          </Badge>
          <h1 className="text-4xl font-bold tracking-tight text-gray-900 sm:text-6xl">
            Build your next
            <span className="text-blue-600"> SaaS product </span>
            in record time
          </h1>
          <p className="mt-6 text-lg leading-8 text-gray-600">
            Stop wasting time on boilerplate code. Our pre-built components help you 
            ship faster and focus on what matters - your unique value proposition.
          </p>
          <div className="mt-10 flex items-center justify-center gap-x-6">
            <Button size="lg" className="gap-2">
              Start Building
              <ArrowRight className="h-4 w-4" />
            </Button>
            <Button variant="outline" size="lg" className="gap-2">
              <Play className="h-4 w-4" />
              Watch Demo
            </Button>
          </div>
          <div className="mt-10 flex items-center justify-center gap-x-6 text-sm text-gray-500">
            <span>✓ No credit card required</span>
            <span>✓ 14-day free trial</span>
            <span>✓ Cancel anytime</span>
          </div>
        </div>
      </div>
    </section>
  )
}
```

### 2. Feature Sections

Highlight product features with icons and descriptions:

```typescript
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"
import { Zap, Shield, Smartphone, Globe, Users, BarChart } from "lucide-react"

const features = [
  {
    icon: Zap,
    title: "Lightning Fast",
    description: "Optimized for performance with sub-second load times and efficient caching.",
    badge: "Performance"
  },
  {
    icon: Shield,
    title: "Enterprise Security",
    description: "Bank-grade security with end-to-end encryption and compliance certifications.",
    badge: "Security"
  },
  {
    icon: Smartphone,
    title: "Mobile First",
    description: "Responsive design that works perfectly on all devices and screen sizes.",
    badge: "Design"
  },
  {
    icon: Globe,
    title: "Global CDN",
    description: "Worldwide content delivery network for optimal performance everywhere.",
    badge: "Infrastructure"
  },
  {
    icon: Users,
    title: "Team Collaboration",
    description: "Built-in tools for seamless team collaboration and project management.",
    badge: "Productivity"
  },
  {
    icon: BarChart,
    title: "Advanced Analytics",
    description: "Comprehensive insights and reporting to track your success metrics.",
    badge: "Analytics"
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
            Powerful features designed to help you build, launch, and scale your business
          </p>
        </div>
        <div className="mx-auto mt-16 max-w-2xl sm:mt-20 lg:mt-24 lg:max-w-none">
          <div className="grid max-w-xl grid-cols-1 gap-8 lg:max-w-none lg:grid-cols-3">
            {features.map((feature) => (
              <Card key={feature.title} className="border-0 shadow-none">
                <CardHeader className="text-center">
                  <div className="mx-auto flex h-16 w-16 items-center justify-center rounded-lg bg-blue-600">
                    <feature.icon className="h-8 w-8 text-white" />
                  </div>
                  <Badge variant="secondary" className="mx-auto mt-4 w-fit">
                    {feature.badge}
                  </Badge>
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

### 3. Pricing Section

Professional pricing tables with feature comparisons:

```typescript
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"
import { Check, X } from "lucide-react"

const plans = [
  {
    name: "Starter",
    price: "$9",
    description: "Perfect for individuals and small projects",
    features: [
      { name: "Up to 3 projects", included: true },
      { name: "5GB storage", included: true },
      { name: "Email support", included: true },
      { name: "Basic analytics", included: true },
      { name: "Custom domain", included: false },
      { name: "Priority support", included: false },
      { name: "Advanced analytics", included: false },
      { name: "Team collaboration", included: false }
    ],
    popular: false,
    cta: "Start Free Trial"
  },
  {
    name: "Professional",
    price: "$29",
    description: "Best for growing businesses and teams",
    features: [
      { name: "Unlimited projects", included: true },
      { name: "50GB storage", included: true },
      { name: "Priority email support", included: true },
      { name: "Advanced analytics", included: true },
      { name: "Custom domain", included: true },
      { name: "Team collaboration", included: true },
      { name: "API access", included: true },
      { name: "White-label options", included: false }
    ],
    popular: true,
    cta: "Start Free Trial"
  },
  {
    name: "Enterprise",
    price: "$99",
    description: "For large organizations with advanced needs",
    features: [
      { name: "Unlimited everything", included: true },
      { name: "500GB storage", included: true },
      { name: "24/7 phone support", included: true },
      { name: "Custom analytics", included: true },
      { name: "Multiple custom domains", included: true },
      { name: "Advanced team features", included: true },
      { name: "Full API access", included: true },
      { name: "White-label options", included: true }
    ],
    popular: false,
    cta: "Contact Sales"
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
            Choose the perfect plan for your needs. Upgrade or downgrade at any time.
          </p>
        </div>
        <div className="mx-auto mt-16 grid max-w-lg grid-cols-1 gap-8 lg:max-w-4xl lg:grid-cols-3">
          {plans.map((plan) => (
            <Card key={plan.name} className={`relative ${plan.popular ? 'border-blue-600 shadow-lg scale-105' : ''}`}>
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
                  {plan.features.map((feature, index) => (
                    <li key={index} className="flex items-center gap-3">
                      {feature.included ? (
                        <Check className="h-4 w-4 text-green-600 flex-shrink-0" />
                      ) : (
                        <X className="h-4 w-4 text-gray-400 flex-shrink-0" />
                      )}
                      <span className={`text-sm ${feature.included ? 'text-gray-900' : 'text-gray-500'}`}>
                        {feature.name}
                      </span>
                    </li>
                  ))}
                </ul>
              </CardContent>
              <CardFooter>
                <Button 
                  className="w-full" 
                  variant={plan.popular ? "default" : "outline"}
                >
                  {plan.cta}
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

### 4. Team Section

Showcase team members with professional layouts:

```typescript
import { Card, CardContent } from "@/components/ui/card"
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar"
import { Badge } from "@/components/ui/badge"
import { Linkedin, Twitter, Github } from "lucide-react"

const team = [
  {
    name: "Sarah Johnson",
    role: "CEO & Founder",
    image: "/team/sarah.jpg",
    bio: "Former VP of Engineering at TechCorp with 15+ years of experience building scalable products.",
    skills: ["Leadership", "Strategy", "Product"],
    social: {
      linkedin: "https://linkedin.com/in/sarahjohnson",
      twitter: "https://twitter.com/sarahjohnson",
      github: "https://github.com/sarahjohnson"
    }
  },
  {
    name: "Michael Chen",
    role: "CTO",
    image: "/team/michael.jpg",
    bio: "Full-stack engineer passionate about creating elegant solutions to complex problems.",
    skills: ["React", "Node.js", "AWS"],
    social: {
      linkedin: "https://linkedin.com/in/michaelchen",
      twitter: "https://twitter.com/michaelchen",
      github: "https://github.com/michaelchen"
    }
  },
  {
    name: "Emily Rodriguez",
    role: "Head of Design",
    image: "/team/emily.jpg",
    bio: "Award-winning designer with expertise in user experience and brand development.",
    skills: ["UI/UX", "Branding", "Figma"],
    social: {
      linkedin: "https://linkedin.com/in/emilyrodriguez",
      twitter: "https://twitter.com/emilyrodriguez"
    }
  }
]

export default function TeamSection() {
  return (
    <section className="py-24 sm:py-32">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div className="mx-auto max-w-2xl text-center">
          <h2 className="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">
            Meet our team
          </h2>
          <p className="mt-6 text-lg leading-8 text-gray-600">
            We're a dynamic group of individuals who are passionate about what we do.
          </p>
        </div>
        <div className="mx-auto mt-16 grid max-w-2xl grid-cols-1 gap-8 lg:max-w-none lg:grid-cols-3">
          {team.map((member) => (
            <Card key={member.name} className="text-center">
              <CardContent className="pt-6">
                <Avatar className="mx-auto h-24 w-24 mb-4">
                  <AvatarImage src={member.image} alt={member.name} />
                  <AvatarFallback>{member.name.split(' ').map(n => n[0]).join('')}</AvatarFallback>
                </Avatar>
                <h3 className="text-xl font-semibold text-gray-900">{member.name}</h3>
                <p className="text-blue-600 font-medium mb-3">{member.role}</p>
                <p className="text-gray-600 text-sm mb-4">{member.bio}</p>
                <div className="flex flex-wrap justify-center gap-2 mb-4">
                  {member.skills.map((skill) => (
                    <Badge key={skill} variant="secondary" className="text-xs">
                      {skill}
                    </Badge>
                  ))}
                </div>
                <div className="flex justify-center space-x-3">
                  {member.social.linkedin && (
                    <a href={member.social.linkedin} className="text-gray-400 hover:text-blue-600">
                      <Linkedin className="h-5 w-5" />
                    </a>
                  )}
                  {member.social.twitter && (
                    <a href={member.social.twitter} className="text-gray-400 hover:text-blue-400">
                      <Twitter className="h-5 w-5" />
                    </a>
                  )}
                  {member.social.github && (
                    <a href={member.social.github} className="text-gray-400 hover:text-gray-900">
                      <Github className="h-5 w-5" />
                    </a>
                  )}
                </div>
              </CardContent>
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

Populate Tailark blocks with real data from APIs:

```typescript
"use client"

import { useEffect, useState } from "react"
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"

interface Project {
  id: number
  name: string
  description: string
  language: string
  stars: number
  url: string
}

export default function DynamicProjectShowcase() {
  const [projects, setProjects] = useState<Project[]>([])
  const [loading, setLoading] = useState(true)

  useEffect(() => {
    // Fetch projects from GitHub API
    fetch('https://api.github.com/users/vercel/repos?sort=stars&per_page=6')
      .then(res => res.json())
      .then(data => {
        const formattedProjects = data.map((repo: any) => ({
          id: repo.id,
          name: repo.name,
          description: repo.description || 'No description available',
          language: repo.language || 'Unknown',
          stars: repo.stargazers_count,
          url: repo.html_url
        }))
        setProjects(formattedProjects)
        setLoading(false)
      })
      .catch(error => {
        console.error('Error fetching projects:', error)
        setLoading(false)
      })
  }, [])

  if (loading) {
    return (
      <section className="py-24">
        <div className="text-center">Loading projects...</div>
      </section>
    )
  }

  return (
    <section className="py-24 sm:py-32">
      <div className="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
        <div className="mx-auto max-w-2xl text-center mb-16">
          <h2 className="text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">
            Featured Open Source Projects
          </h2>
          <p className="mt-6 text-lg leading-8 text-gray-600">
            Explore our most popular repositories and contributions to the community
          </p>
        </div>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {projects.map((project) => (
            <Card key={project.id} className="hover:shadow-lg transition-shadow">
              <CardHeader>
                <div className="flex justify-between items-start">
                  <CardTitle className="text-lg">{project.name}</CardTitle>
                  <Badge variant="secondary">⭐ {project.stars}</Badge>
                </div>
                <CardDescription>{project.description}</CardDescription>
              </CardHeader>
              <CardContent>
                <div className="flex justify-between items-center">
                  <Badge variant="outline">{project.language}</Badge>
                  <Button variant="outline" size="sm" asChild>
                    <a href={project.url} target="_blank" rel="noopener noreferrer">
                      View Project
                    </a>
                  </Button>
                </div>
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
2. **Content Strategy**: Replace placeholder text with compelling, benefit-focused copy
3. **Performance**: Optimize images and use Next.js Image component
4. **SEO**: Add proper meta tags, structured data, and semantic HTML
5. **Accessibility**: Ensure proper heading hierarchy and alt text
6. **Mobile Experience**: Test on various devices and screen sizes

## Common Pitfalls

### Issue 1: Generic Content
Using placeholder content without customization.

**Solution:**
```typescript
// Replace generic content with specific, benefit-focused copy
const heroContent = {
  badge: "🚀 Now Available",
  headline: "Transform Your Business with AI-Powered Analytics",
  subheadline: "Get actionable insights from your data in minutes, not months. Join 10,000+ companies already using our platform.",
  cta: "Start Free Trial",
  benefits: ["No setup required", "Cancel anytime", "24/7 support"]
}
```

### Issue 2: Poor Visual Hierarchy
Inconsistent spacing and typography.

**Solution:**
```typescript
// Use consistent spacing and typography scales
<div className="space-y-16"> {/* Consistent section spacing */}
  <h1 className="text-4xl sm:text-6xl font-bold"> {/* Responsive typography */}
    Your Headline
  </h1>
  <p className="text-lg leading-8 text-gray-600 max-w-2xl"> {/* Readable body text */}
    Your description
  </p>
</div>
```

### Issue 3: Missing Call-to-Actions
Weak or missing conversion elements.

**Solution:**
```typescript
// Strong, action-oriented CTAs
<div className="flex flex-col sm:flex-row gap-4 justify-center">
  <Button size="lg" className="bg-blue-600 hover:bg-blue-700">
    Start Building Today
  </Button>
  <Button variant="outline" size="lg">
    See Live Demo
  </Button>
</div>
```

## References

{% embed url="https://tailark.com/" %}

{% embed url="https://github.com/tailark/tailark" %}

{% embed url="https://ui.shadcn.com/" %}
