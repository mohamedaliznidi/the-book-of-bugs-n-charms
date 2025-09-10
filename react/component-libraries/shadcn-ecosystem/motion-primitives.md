---
description: >-
  Complete grimoire for Motion Primitives - an open-source UI kit of animated React enchantments
  built with Framer Motion and Tailwind CSS magic. Master setup, enchantments, and best practices
  for adding smooth mystical animations to your React/Next.js projects.

theme: "magic"
---

# Motion Primitives - The Animation Foundation Mastery

## The Ancient Knowledge

Motion Primitives is an open-source UI kit of animated React components built with Framer Motion and Tailwind CSS. It provides a collection of reusable, ready-made animated components designed for React/Next.js projects, eliminating the need to write motion logic from scratch.

## Key Features

- **Ready-made animations**: Pre-built components with smooth animations
- **Framer Motion powered**: Built on the industry-standard animation library
- **Tailwind CSS integration**: Seamless styling with utility-first CSS
- **Copy-paste components**: Own and customize the code completely
- **TypeScript support**: Full type safety and IntelliSense
- **Performance optimized**: Efficient animations with minimal bundle impact

## Use Cases

1. **Landing Pages**: Eye-catching scroll animations and hero sections
2. **Interactive Dashboards**: Animated progress bars, counters, and data visualizations
3. **Product Showcases**: Sliding content sections and animated feature highlights
4. **Portfolio Sites**: Dynamic project galleries and skill demonstrations
5. **Marketing Sites**: Engaging user interactions and conversion-focused animations

## Installation and Setup

### 1. Prerequisites

Ensure you have a Next.js project with Tailwind CSS and Framer Motion:

```bash
# Create Next.js project
pnpm create next-app@latest my-app --typescript --tailwind --eslint
cd my-app

# Install Framer Motion
pnpm add framer-motion
```

### 2. Install Motion Primitives

```bash
# Install via npm
npm install motion-primitives

# Or copy components directly from the website
# Visit motion-primitives.com and copy desired components
```

### 3. Configure Tailwind CSS

Add the Motion Primitives path to your `tailwind.config.js`:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
    './app/**/*.{js,ts,jsx,tsx,mdx}',
    './node_modules/motion-primitives/**/*.{js,ts,jsx,tsx}', // Add this line
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

## Core Components

### 1. In View Animation

Animate elements as they enter the viewport:

```typescript
import { InView } from 'motion-primitives'

export default function InViewExample() {
  return (
    <div className="space-y-8">
      <InView
        variants={{
          hidden: { opacity: 0, y: 100, filter: 'blur(4px)' },
          visible: { opacity: 1, y: 0, filter: 'blur(0px)' },
        }}
        viewOptions={{ margin: '0px 0px -200px 0px' }}
        transition={{ duration: 0.3, ease: 'easeInOut' }}
      >
        <div className="rounded-lg bg-gradient-to-r from-blue-500 to-purple-600 p-8 text-white">
          <h2 className="text-2xl font-bold">Animated on Scroll</h2>
          <p>This content animates when it comes into view</p>
        </div>
      </InView>
    </div>
  )
}
```

### 2. Scroll Progress

Show reading progress with an animated progress bar:

```typescript
import { ScrollProgress } from 'motion-primitives'

export default function ScrollProgressExample() {
  return (
    <>
      <ScrollProgress className="fixed top-0 z-50 h-1 bg-gradient-to-r from-blue-500 to-purple-600" />
      <div className="min-h-[200vh] p-8">
        <h1 className="text-4xl font-bold">Long Content</h1>
        <p>Scroll to see the progress bar in action...</p>
        {/* Long content here */}
      </div>
    </>
  )
}
```

### 3. Animated Counter

Create engaging number animations:

```typescript
import { AnimatedCounter } from 'motion-primitives'

export default function CounterExample() {
  return (
    <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
      <div className="text-center">
        <AnimatedCounter
          from={0}
          to={1000}
          duration={2}
          className="text-4xl font-bold text-blue-600"
        />
        <p className="text-gray-600 mt-2">Happy Customers</p>
      </div>
      
      <div className="text-center">
        <AnimatedCounter
          from={0}
          to={50}
          duration={1.5}
          className="text-4xl font-bold text-green-600"
        />
        <span className="text-4xl font-bold text-green-600">+</span>
        <p className="text-gray-600 mt-2">Projects Completed</p>
      </div>
      
      <div className="text-center">
        <AnimatedCounter
          from={0}
          to={99.9}
          duration={2.5}
          decimals={1}
          className="text-4xl font-bold text-purple-600"
        />
        <span className="text-4xl font-bold text-purple-600">%</span>
        <p className="text-gray-600 mt-2">Uptime</p>
      </div>
    </div>
  )
}
```

### 4. Fade In Stagger

Animate multiple elements with staggered timing:

```typescript
import { FadeInStagger } from 'motion-primitives'

export default function StaggerExample() {
  const features = [
    { title: 'Fast Performance', description: 'Optimized for speed' },
    { title: 'Easy to Use', description: 'Simple and intuitive' },
    { title: 'Fully Customizable', description: 'Make it your own' },
  ]

  return (
    <FadeInStagger
      faster
      className="grid grid-cols-1 md:grid-cols-3 gap-6"
    >
      {features.map((feature, index) => (
        <div
          key={index}
          className="rounded-lg border p-6 shadow-sm hover:shadow-md transition-shadow"
        >
          <h3 className="text-xl font-semibold mb-2">{feature.title}</h3>
          <p className="text-gray-600">{feature.description}</p>
        </div>
      ))}
    </FadeInStagger>
  )
}
```

## Advanced Usage

### Custom Variants

Create custom animation variants for specific needs:

```typescript
import { InView } from 'motion-primitives'

const customVariants = {
  hidden: { 
    opacity: 0, 
    scale: 0.8, 
    rotateX: -90,
    transformPerspective: 1000 
  },
  visible: { 
    opacity: 1, 
    scale: 1, 
    rotateX: 0,
    transformPerspective: 1000 
  },
}

export default function CustomAnimation() {
  return (
    <InView
      variants={customVariants}
      transition={{ duration: 0.6, ease: 'backOut' }}
    >
      <div className="bg-white rounded-xl shadow-lg p-8">
        <h2 className="text-2xl font-bold">Custom 3D Animation</h2>
      </div>
    </InView>
  )
}
```

### Combining with shadcn/ui

Integrate Motion Primitives with shadcn/ui components:

```typescript
import { InView } from 'motion-primitives'
import { Button } from '@/components/ui/button'
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from '@/components/ui/card'

export default function IntegratedExample() {
  return (
    <InView
      variants={{
        hidden: { opacity: 0, y: 50 },
        visible: { opacity: 1, y: 0 },
      }}
      transition={{ duration: 0.5 }}
    >
      <Card className="max-w-md mx-auto">
        <CardHeader>
          <CardTitle>Animated Card</CardTitle>
          <CardDescription>
            This shadcn/ui card animates into view
          </CardDescription>
        </CardHeader>
        <CardContent>
          <p className="mb-4">Content with smooth animations</p>
          <Button className="w-full">Get Started</Button>
        </CardContent>
      </Card>
    </InView>
  )
}
```

## Best Practices

1. **Performance**: Use `will-change` CSS property sparingly and remove after animations
2. **Accessibility**: Respect `prefers-reduced-motion` for users with motion sensitivity
3. **Timing**: Keep animations under 300ms for UI interactions, longer for storytelling
4. **Easing**: Use appropriate easing functions (`easeInOut` for most cases)
5. **Staggering**: Use stagger delays of 50-100ms for optimal visual flow

## Common Pitfalls

### Issue 1: Animation Performance
Heavy animations can cause janky performance.

**Solution:**
```typescript
// Use transform properties instead of layout properties
variants={{
  hidden: { opacity: 0, transform: 'translateY(20px)' }, // Good
  visible: { opacity: 1, transform: 'translateY(0px)' },
}}

// Avoid animating layout properties
variants={{
  hidden: { opacity: 0, top: '20px' }, // Bad - causes layout thrashing
  visible: { opacity: 1, top: '0px' },
}}
```

### Issue 2: Accessibility Concerns
Not respecting user motion preferences.

**Solution:**
```typescript
import { useReducedMotion } from 'framer-motion'

export default function AccessibleAnimation() {
  const shouldReduceMotion = useReducedMotion()
  
  const variants = shouldReduceMotion
    ? { hidden: { opacity: 0 }, visible: { opacity: 1 } }
    : { 
        hidden: { opacity: 0, y: 20 }, 
        visible: { opacity: 1, y: 0 } 
      }
  
  return <InView variants={variants}>Content</InView>
}
```

## References

{% embed url="https://motion-primitives.com/" %}

{% embed url="https://www.framer.com/motion/" %}

{% embed url="https://github.com/framer/motion" %}
