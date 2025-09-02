---
description: >-
  Complete guide to Magic UI - an open-source library of 150+ animated React components
  designed to complement shadcn/ui. Learn Framer Motion integration, component usage,
  and best practices for building engaging interfaces.
---

# Magic UI

## Introduction

Magic UI is an open-source library of animated React components specifically designed to complement shadcn/ui. With over 150 prebuilt components and effects, it uses Framer Motion for smooth transitions and has gained significant popularity with 18.6k GitHub stars for building engaging landing pages and product sites.

## Key Features

- **150+ Components**: Extensive collection of animated UI elements
- **Framer Motion Powered**: Smooth, performant animations
- **shadcn/ui Compatible**: Seamless integration with existing shadcn/ui projects
- **Copy-Paste Ready**: No complex installation, just copy what you need
- **TypeScript Support**: Full type safety and IntelliSense
- **Open Source**: MIT licensed and community-driven
- **Performance Optimized**: Efficient animations with minimal bundle impact

## Use Cases

1. **Landing Pages**: Eye-catching hero sections and interactive elements
2. **Product Showcases**: Animated feature highlights and demonstrations
3. **Portfolio Sites**: Dynamic project galleries and skill presentations
4. **Marketing Sites**: Engaging user interactions for better conversions
5. **SaaS Applications**: Polished interfaces with professional animations
6. **Interactive Dashboards**: Animated charts and data visualizations

## Installation and Setup

### 1. Prerequisites

Ensure you have a Next.js project with shadcn/ui and Framer Motion:

```bash
# Create Next.js project
pnpm create next-app@latest my-app --typescript --tailwind --eslint
cd my-app

# Initialize shadcn/ui
pnpm dlx shadcn-ui@latest init

# Install Framer Motion
pnpm add framer-motion
```

### 2. Install Magic UI

```bash
# Install via npm
npm install magicui

# Or copy components directly from magicui.design
```

### 3. Configure Tailwind CSS

Add Magic UI to your `tailwind.config.js`:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
    './app/**/*.{js,ts,jsx,tsx,mdx}',
    './node_modules/magicui/**/*.{js,ts,jsx,tsx}', // Add this line
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

## Core Components

### 1. Animated Text Effects

Create engaging text animations:

```typescript
import { TextGenerateEffect } from "magicui/text-generate-effect"
import { TypewriterEffect } from "magicui/typewriter-effect"

export default function AnimatedTextDemo() {
  const words = [
    { text: "Build" },
    { text: "amazing" },
    { text: "websites" },
    { text: "with" },
    { text: "Magic", className: "text-blue-500 dark:text-blue-500" },
    { text: "UI." },
  ]

  return (
    <div className="space-y-8">
      <div className="text-center">
        <TextGenerateEffect 
          words="Magic UI makes your components come alive with beautiful animations"
          className="text-2xl font-bold"
        />
      </div>
      
      <div className="text-center">
        <TypewriterEffect 
          words={words}
          className="text-3xl font-bold"
          cursorClassName="bg-blue-500"
        />
      </div>
    </div>
  )
}
```

### 2. Interactive Cards

Animated card components with hover effects:

```typescript
import { HoverCard } from "magicui/hover-card"
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"

const features = [
  {
    title: "Lightning Fast",
    description: "Optimized animations that don't compromise performance",
    icon: "⚡",
    color: "from-yellow-400 to-orange-500"
  },
  {
    title: "Easy Integration",
    description: "Works seamlessly with your existing shadcn/ui components",
    icon: "🔧",
    color: "from-blue-400 to-purple-500"
  },
  {
    title: "Fully Customizable",
    description: "Modify animations and styles to match your brand",
    icon: "🎨",
    color: "from-green-400 to-blue-500"
  }
]

export default function InteractiveCards() {
  return (
    <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
      {features.map((feature, index) => (
        <HoverCard key={index} className="h-full">
          <Card className="h-full transition-all duration-300 hover:shadow-lg">
            <CardHeader>
              <div className={`w-12 h-12 rounded-lg bg-gradient-to-r ${feature.color} flex items-center justify-center text-2xl mb-4`}>
                {feature.icon}
              </div>
              <CardTitle>{feature.title}</CardTitle>
            </CardHeader>
            <CardContent>
              <CardDescription>{feature.description}</CardDescription>
              <Badge variant="secondary" className="mt-4">
                New Feature
              </Badge>
            </CardContent>
          </Card>
        </HoverCard>
      ))}
    </div>
  )
}
```

### 3. Animated Backgrounds

Dynamic background effects for visual appeal:

```typescript
import { ParticlesBackground } from "magicui/particles-background"
import { GridBackground } from "magicui/grid-background"
import { Button } from "@/components/ui/button"

export default function HeroWithBackground() {
  return (
    <section className="relative min-h-screen flex items-center justify-center overflow-hidden">
      {/* Animated Background */}
      <ParticlesBackground
        className="absolute inset-0"
        particleCount={50}
        particleColor="#3b82f6"
        connectionColor="#3b82f6"
      />
      
      {/* Alternative: Grid Background */}
      {/* <GridBackground className="absolute inset-0" /> */}
      
      {/* Content */}
      <div className="relative z-10 text-center max-w-4xl mx-auto px-4">
        <h1 className="text-4xl md:text-6xl font-bold mb-6 bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
          Welcome to the Future
        </h1>
        <p className="text-xl text-gray-600 mb-8 max-w-2xl mx-auto">
          Experience the next generation of web interfaces with beautiful animations
          and seamless interactions.
        </p>
        <div className="space-x-4">
          <Button size="lg" className="bg-gradient-to-r from-blue-600 to-purple-600">
            Get Started
          </Button>
          <Button variant="outline" size="lg">
            Learn More
          </Button>
        </div>
      </div>
    </section>
  )
}
```

### 4. Image Carousels

Animated image galleries and carousels:

```typescript
import { ImageCarousel } from "magicui/image-carousel"
import { Card, CardContent } from "@/components/ui/card"

const images = [
  {
    src: "/project1.jpg",
    alt: "Project 1",
    title: "E-commerce Platform",
    description: "Modern shopping experience"
  },
  {
    src: "/project2.jpg",
    alt: "Project 2", 
    title: "Dashboard Analytics",
    description: "Data visualization tool"
  },
  {
    src: "/project3.jpg",
    alt: "Project 3",
    title: "Mobile App",
    description: "Cross-platform solution"
  }
]

export default function ProjectShowcase() {
  return (
    <section className="py-16">
      <div className="max-w-6xl mx-auto px-4">
        <h2 className="text-3xl font-bold text-center mb-12">Featured Projects</h2>
        
        <ImageCarousel
          images={images}
          autoPlay={true}
          interval={5000}
          showDots={true}
          showArrows={true}
          className="rounded-lg overflow-hidden"
        />
        
        {/* Alternative: Manual carousel with cards */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6 mt-12">
          {images.map((image, index) => (
            <Card key={index} className="overflow-hidden hover:shadow-lg transition-shadow">
              <div className="aspect-video bg-gray-200 relative overflow-hidden">
                <img 
                  src={image.src} 
                  alt={image.alt}
                  className="w-full h-full object-cover transition-transform hover:scale-105"
                />
              </div>
              <CardContent className="p-6">
                <h3 className="text-xl font-semibold mb-2">{image.title}</h3>
                <p className="text-gray-600">{image.description}</p>
              </CardContent>
            </Card>
          ))}
        </div>
      </div>
    </section>
  )
}
```

## Advanced Usage

### Custom Animation Variants

Create custom animations using Framer Motion:

```typescript
import { motion } from "framer-motion"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"

const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1,
      delayChildren: 0.3
    }
  }
}

const itemVariants = {
  hidden: { y: 20, opacity: 0 },
  visible: {
    y: 0,
    opacity: 1,
    transition: {
      type: "spring",
      stiffness: 100
    }
  }
}

export default function StaggeredCards() {
  const cards = [
    { title: "Analytics", value: "2,345", change: "+12%" },
    { title: "Revenue", value: "$45,678", change: "+8%" },
    { title: "Users", value: "1,234", change: "+23%" },
    { title: "Growth", value: "89%", change: "+5%" }
  ]

  return (
    <motion.div
      variants={containerVariants}
      initial="hidden"
      animate="visible"
      className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6"
    >
      {cards.map((card, index) => (
        <motion.div key={index} variants={itemVariants}>
          <Card className="text-center">
            <CardHeader>
              <CardTitle className="text-sm font-medium text-gray-600">
                {card.title}
              </CardTitle>
            </CardHeader>
            <CardContent>
              <div className="text-2xl font-bold">{card.value}</div>
              <p className="text-sm text-green-600">{card.change}</p>
            </CardContent>
          </Card>
        </motion.div>
      ))}
    </motion.div>
  )
}
```

### Integration with Real-time Data

Animate data updates with Magic UI:

```typescript
"use client"

import { useEffect, useState } from "react"
import { AnimatedNumber } from "magicui/animated-number"
import { ProgressBar } from "magicui/progress-bar"
import { Card, CardContent, CardHeader, CardTitle } from "@/components/ui/card"

interface LiveStats {
  visitors: number
  sales: number
  conversion: number
}

export default function LiveDashboard() {
  const [stats, setStats] = useState<LiveStats>({
    visitors: 0,
    sales: 0,
    conversion: 0
  })

  useEffect(() => {
    // Simulate real-time data updates
    const interval = setInterval(() => {
      setStats(prev => ({
        visitors: prev.visitors + Math.floor(Math.random() * 10),
        sales: prev.sales + Math.floor(Math.random() * 5),
        conversion: Math.min(100, prev.conversion + Math.random() * 2)
      }))
    }, 2000)

    // Initial data
    setStats({
      visitors: 1250,
      sales: 89,
      conversion: 3.4
    })

    return () => clearInterval(interval)
  }, [])

  return (
    <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
      <Card>
        <CardHeader>
          <CardTitle className="text-sm font-medium">Live Visitors</CardTitle>
        </CardHeader>
        <CardContent>
          <AnimatedNumber
            value={stats.visitors}
            className="text-3xl font-bold text-blue-600"
            duration={1000}
          />
          <p className="text-sm text-gray-600 mt-1">Currently online</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader>
          <CardTitle className="text-sm font-medium">Sales Today</CardTitle>
        </CardHeader>
        <CardContent>
          <AnimatedNumber
            value={stats.sales}
            className="text-3xl font-bold text-green-600"
            duration={1000}
          />
          <p className="text-sm text-gray-600 mt-1">Transactions</p>
        </CardContent>
      </Card>

      <Card>
        <CardHeader>
          <CardTitle className="text-sm font-medium">Conversion Rate</CardTitle>
        </CardHeader>
        <CardContent>
          <div className="space-y-2">
            <AnimatedNumber
              value={stats.conversion}
              className="text-3xl font-bold text-purple-600"
              duration={1000}
              decimals={1}
              suffix="%"
            />
            <ProgressBar
              value={stats.conversion}
              max={10}
              className="h-2"
              animated={true}
            />
          </div>
        </CardContent>
      </Card>
    </div>
  )
}
```

## Best Practices

1. **Performance**: Use `will-change` CSS property sparingly and remove after animations
2. **Accessibility**: Respect `prefers-reduced-motion` for users with motion sensitivity
3. **Timing**: Keep UI animations under 300ms, storytelling animations can be longer
4. **Easing**: Use appropriate easing functions for natural motion
5. **Purpose**: Ensure animations serve a functional purpose, not just decoration
6. **Testing**: Test animations on various devices and connection speeds

## Common Pitfalls

### Issue 1: Animation Performance
Heavy animations can cause janky performance on slower devices.

**Solution:**
```typescript
// Use transform properties for better performance
const optimizedVariants = {
  hidden: { opacity: 0, transform: "translateY(20px)" },
  visible: { opacity: 1, transform: "translateY(0px)" }
}

// Avoid animating layout properties
const badVariants = {
  hidden: { opacity: 0, top: "20px" }, // Causes layout thrashing
  visible: { opacity: 1, top: "0px" }
}
```

### Issue 2: Accessibility Issues
Not respecting user motion preferences.

**Solution:**
```typescript
import { useReducedMotion } from "framer-motion"

export default function AccessibleAnimation() {
  const shouldReduceMotion = useReducedMotion()
  
  const variants = shouldReduceMotion
    ? { hidden: { opacity: 0 }, visible: { opacity: 1 } }
    : { 
        hidden: { opacity: 0, scale: 0.8 }, 
        visible: { opacity: 1, scale: 1 } 
      }
  
  return (
    <motion.div variants={variants} initial="hidden" animate="visible">
      Content
    </motion.div>
  )
}
```

### Issue 3: Bundle Size Impact
Including too many animated components can increase bundle size.

**Solution:**
```typescript
// Import only what you need
import { TextGenerateEffect } from "magicui/text-generate-effect"
// Instead of importing everything
// import * from "magicui"

// Use dynamic imports for heavy components
const HeavyAnimation = dynamic(() => import("magicui/heavy-animation"), {
  loading: () => <div>Loading...</div>
})
```

## References

{% embed url="https://magicui.design/" %}

{% embed url="https://github.com/magicuidesign/magicui" %}

{% embed url="https://www.framer.com/motion/" %}
