---
description: >-
  Complete grimoire for React Bits - a comprehensive library of 100+ animated and interactive
  React enchantments for building stunning, memorable mystical websites. Master CLI usage, customization
  options, and mystical integration patterns.

theme: "magic"
---

# React Bits - The Interactive Animation Mastery

## The Ancient Knowledge

React Bits is a comprehensive library of animated and interactive React components designed for creating "stunning, memorable websites." With over 100 unique components and 22.4k GitHub stars, it offers extensive customization options with both JavaScript/TypeScript and Tailwind vs. CSS variants, plus a CLI for easy component installation.

## Key Features

- **100+ Components**: Extensive collection of creative animations and interactions
- **CLI Installation**: Easy component installation with command-line tools
- **Flexible Styling**: Support for both Tailwind CSS and vanilla CSS
- **TypeScript Support**: Full type safety with TypeScript definitions
- **Minimal Dependencies**: Lightweight with minimal external dependencies
- **Creative Animations**: Text animations, scroll effects, dynamic backgrounds, and image effects
- **Highly Customizable**: Extensive customization options for each component

## Use Cases

1. **Interactive Portfolios**: Dynamic showcases with animated project galleries
2. **Creative Agencies**: Visually striking websites with unique animations
3. **Product Landing Pages**: Engaging presentations with scroll-triggered effects
4. **One-Page Applications**: Highly interactive single-page experiences
5. **Presentation Sites**: Dynamic content for conferences and events
6. **Art & Design Showcases**: Creative displays with advanced visual effects

## Installation and Setup

### 1. Prerequisites

Ensure you have a Next.js project with Tailwind CSS:

```bash
# Create Next.js project
pnpm create next-app@latest my-app --typescript --tailwind --eslint
cd my-app

# Install Framer Motion (required for animations)
pnpm add framer-motion
```

### 2. Install React Bits CLI

```bash
# Install the CLI globally
npm install -g react-bits-cli

# Or use npx for one-time usage
npx react-bits-cli init
```

### 3. Initialize React Bits

```bash
# Initialize in your project
react-bits init

# This creates a react-bits.config.js file and sets up the component structure
```

### 4. Install Components

```bash
# Install specific components
react-bits add text-animate
react-bits add scroll-reveal
react-bits add background-gradient

# Install multiple components at once
react-bits add text-animate scroll-reveal background-gradient

# List available components
react-bits list
```

## Core Component Categories

### 1. Text Animations

Dynamic text effects for engaging content:

```typescript
import { TextAnimate } from "@/components/react-bits/text-animate"
import { TypeWriter } from "@/components/react-bits/type-writer"
import { TextReveal } from "@/components/react-bits/text-reveal"

export default function TextAnimationDemo() {
  return (
    <div className="space-y-12 py-16">
      {/* Animated Text Entry */}
      <TextAnimate
        text="Welcome to React Bits"
        className="text-4xl font-bold text-center"
        animation="slideUp"
        delay={0.1}
        duration={0.8}
      />
      
      {/* Typewriter Effect */}
      <TypeWriter
        words={[
          "Build amazing websites",
          "Create stunning animations", 
          "Engage your audience"
        ]}
        className="text-2xl text-center text-blue-600"
        typeSpeed={100}
        deleteSpeed={50}
        delayBetween={2000}
      />
      
      {/* Text Reveal on Scroll */}
      <TextReveal
        text="This text reveals as you scroll down the page, creating an engaging reading experience."
        className="text-lg max-w-2xl mx-auto text-center"
        revealBy="words"
        stagger={0.1}
      />
    </div>
  )
}
```

### 2. Scroll-Triggered Animations

Components that animate based on scroll position:

```typescript
import { ScrollReveal } from "@/components/react-bits/scroll-reveal"
import { ParallaxText } from "@/components/react-bits/parallax-text"
import { ScrollProgress } from "@/components/react-bits/scroll-progress"
import { Card, CardContent, CardDescription, CardHeader, CardTitle } from "@/components/ui/card"

const features = [
  {
    title: "Performance First",
    description: "Optimized animations that don't compromise speed",
    icon: "🚀"
  },
  {
    title: "Easy Integration", 
    description: "Drop-in components that work with your existing setup",
    icon: "🔧"
  },
  {
    title: "Fully Customizable",
    description: "Modify every aspect to match your design vision",
    icon: "🎨"
  }
]

export default function ScrollAnimations() {
  return (
    <>
      {/* Scroll Progress Indicator */}
      <ScrollProgress className="fixed top-0 left-0 right-0 h-1 bg-gradient-to-r from-blue-500 to-purple-500 z-50" />
      
      {/* Parallax Text */}
      <ParallaxText
        text="REACT BITS"
        className="text-8xl font-bold text-gray-200 select-none"
        speed={0.5}
      />
      
      {/* Scroll-Revealed Cards */}
      <div className="grid grid-cols-1 md:grid-cols-3 gap-8 py-16">
        {features.map((feature, index) => (
          <ScrollReveal
            key={index}
            animation="slideUp"
            delay={index * 0.2}
            threshold={0.3}
          >
            <Card className="h-full hover:shadow-lg transition-shadow">
              <CardHeader className="text-center">
                <div className="text-4xl mb-4">{feature.icon}</div>
                <CardTitle>{feature.title}</CardTitle>
              </CardHeader>
              <CardContent>
                <CardDescription className="text-center">
                  {feature.description}
                </CardDescription>
              </CardContent>
            </Card>
          </ScrollReveal>
        ))}
      </div>
    </>
  )
}
```

### 3. Dynamic Backgrounds

Animated background effects for visual impact:

```typescript
import { GradientBackground } from "@/components/react-bits/gradient-background"
import { ParticleField } from "@/components/react-bits/particle-field"
import { WaveBackground } from "@/components/react-bits/wave-background"
import { Button } from "@/components/ui/button"

export default function HeroWithDynamicBackground() {
  return (
    <section className="relative min-h-screen flex items-center justify-center overflow-hidden">
      {/* Animated Gradient Background */}
      <GradientBackground
        colors={["#667eea", "#764ba2", "#f093fb"]}
        speed={0.02}
        className="absolute inset-0"
      />
      
      {/* Alternative: Particle Field */}
      {/* 
      <ParticleField
        particleCount={100}
        particleColor="#ffffff"
        connectionDistance={150}
        speed={0.5}
        className="absolute inset-0"
      />
      */}
      
      {/* Alternative: Wave Background */}
      {/*
      <WaveBackground
        waveColor="#3b82f6"
        waveOpacity={0.3}
        waveSpeed={0.02}
        waveHeight={100}
        className="absolute inset-0"
      />
      */}
      
      {/* Hero Content */}
      <div className="relative z-10 text-center max-w-4xl mx-auto px-4 text-white">
        <h1 className="text-5xl md:text-7xl font-bold mb-6">
          Create Something
          <span className="block bg-gradient-to-r from-yellow-400 to-pink-400 bg-clip-text text-transparent">
            Extraordinary
          </span>
        </h1>
        <p className="text-xl md:text-2xl mb-8 opacity-90 max-w-2xl mx-auto">
          Build websites that leave lasting impressions with React Bits' 
          collection of stunning animated components.
        </p>
        <div className="space-x-4">
          <Button size="lg" className="bg-white text-gray-900 hover:bg-gray-100">
            Get Started
          </Button>
          <Button variant="outline" size="lg" className="border-white text-white hover:bg-white hover:text-gray-900">
            View Examples
          </Button>
        </div>
      </div>
    </section>
  )
}
```

### 4. Image Effects

Advanced image animations and effects:

```typescript
import { ImageReveal } from "@/components/react-bits/image-reveal"
import { ImageParallax } from "@/components/react-bits/image-parallax"
import { ImageHover } from "@/components/react-bits/image-hover"

const projects = [
  {
    id: 1,
    title: "E-commerce Platform",
    description: "Modern shopping experience with seamless checkout",
    image: "/project1.jpg",
    category: "Web Development"
  },
  {
    id: 2,
    title: "Mobile Banking App",
    description: "Secure and intuitive financial management",
    image: "/project2.jpg", 
    category: "Mobile App"
  },
  {
    id: 3,
    title: "Data Visualization Tool",
    description: "Interactive charts and analytics dashboard",
    image: "/project3.jpg",
    category: "Data Science"
  }
]

export default function ProjectGallery() {
  return (
    <section className="py-16">
      <div className="max-w-6xl mx-auto px-4">
        <h2 className="text-4xl font-bold text-center mb-16">Featured Projects</h2>
        
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
          {projects.map((project, index) => (
            <div key={project.id} className="group">
              <ImageReveal
                src={project.image}
                alt={project.title}
                className="aspect-video rounded-lg overflow-hidden mb-4"
                revealDirection="up"
                delay={index * 0.1}
              >
                <ImageHover
                  overlay="gradient"
                  overlayColor="rgba(0,0,0,0.7)"
                  className="w-full h-full"
                >
                  <div className="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity">
                    <Button variant="secondary">View Project</Button>
                  </div>
                </ImageHover>
              </ImageReveal>
              
              <div className="space-y-2">
                <span className="text-sm text-blue-600 font-medium">
                  {project.category}
                </span>
                <h3 className="text-xl font-semibold">{project.title}</h3>
                <p className="text-gray-600">{project.description}</p>
              </div>
            </div>
          ))}
        </div>
      </div>
    </section>
  )
}
```

## CLI Usage

### Available Commands

```bash
# Initialize React Bits in your project
react-bits init

# List all available components
react-bits list

# Search for specific components
react-bits search "text"

# Add components to your project
react-bits add component-name

# Remove components
react-bits remove component-name

# Update components to latest version
react-bits update

# Show component documentation
react-bits docs component-name

# Generate custom component template
react-bits generate my-component
```

### Configuration

Customize React Bits behavior with `react-bits.config.js`:

```javascript
module.exports = {
  // Component installation directory
  componentsDir: "./components/react-bits",
  
  // Styling preference
  styling: "tailwind", // or "css"
  
  // TypeScript support
  typescript: true,
  
  // Animation library preference
  animationLibrary: "framer-motion", // or "css-animations"
  
  // Custom component templates
  templates: {
    component: "./templates/component.template.tsx",
    story: "./templates/story.template.tsx"
  },
  
  // Default animation settings
  animations: {
    duration: 0.6,
    easing: "easeInOut",
    stagger: 0.1
  }
}
```

## Advanced Customization

### Creating Custom Components

Extend React Bits with your own animated components:

```typescript
import { motion, useInView } from "framer-motion"
import { useRef } from "react"

interface CustomAnimatedCardProps {
  children: React.ReactNode
  className?: string
  delay?: number
  direction?: "up" | "down" | "left" | "right"
}

export function CustomAnimatedCard({ 
  children, 
  className = "", 
  delay = 0,
  direction = "up" 
}: CustomAnimatedCardProps) {
  const ref = useRef(null)
  const isInView = useInView(ref, { once: true, margin: "-100px" })
  
  const directionVariants = {
    up: { y: 50 },
    down: { y: -50 },
    left: { x: 50 },
    right: { x: -50 }
  }
  
  return (
    <motion.div
      ref={ref}
      initial={{ 
        opacity: 0, 
        ...directionVariants[direction] 
      }}
      animate={isInView ? { 
        opacity: 1, 
        x: 0, 
        y: 0 
      } : {}}
      transition={{ 
        duration: 0.6, 
        delay,
        ease: "easeOut" 
      }}
      className={className}
    >
      {children}
    </motion.div>
  )
}
```

### Performance Optimization

Optimize animations for better performance:

```typescript
import { motion, useReducedMotion } from "framer-motion"

export function OptimizedAnimation({ children }: { children: React.ReactNode }) {
  const shouldReduceMotion = useReducedMotion()
  
  return (
    <motion.div
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ 
        duration: shouldReduceMotion ? 0 : 0.6,
        ease: "easeOut"
      }}
      // Use transform instead of layout properties
      style={{ 
        willChange: "transform, opacity",
        transform: "translateZ(0)" // Force hardware acceleration
      }}
    >
      {children}
    </motion.div>
  )
}
```

## Best Practices

1. **Performance**: Use `transform` properties instead of layout properties for animations
2. **Accessibility**: Always respect `prefers-reduced-motion` user preferences
3. **Progressive Enhancement**: Ensure content is accessible without JavaScript
4. **Bundle Size**: Import only the components you need to minimize bundle size
5. **Testing**: Test animations on various devices and connection speeds
6. **Purpose**: Use animations to enhance UX, not just for visual appeal

## Common Pitfalls

### Issue 1: Animation Performance Issues
Heavy animations causing janky scrolling or interactions.

**Solution:**
```typescript
// Use CSS transforms for better performance
const optimizedVariants = {
  hidden: { opacity: 0, transform: "translateY(20px)" },
  visible: { opacity: 1, transform: "translateY(0)" }
}

// Enable hardware acceleration
const style = {
  willChange: "transform",
  transform: "translateZ(0)"
}
```

### Issue 2: Accessibility Concerns
Animations causing motion sickness or accessibility issues.

**Solution:**
```typescript
import { useReducedMotion } from "framer-motion"

const shouldReduceMotion = useReducedMotion()
const animationProps = shouldReduceMotion 
  ? { initial: false, animate: false }
  : { initial: "hidden", animate: "visible" }
```

### Issue 3: Bundle Size Impact
Including too many animated components increases bundle size.

**Solution:**
```bash
# Install only needed components
react-bits add text-animate scroll-reveal

# Use dynamic imports for heavy components
const HeavyAnimation = dynamic(() => import("@/components/react-bits/heavy-animation"))
```

## References

{% embed url="https://github.com/react-bits/react-bits" %}

{% embed url="https://www.framer.com/motion/" %}

{% embed url="https://tailwindcss.com/" %}
