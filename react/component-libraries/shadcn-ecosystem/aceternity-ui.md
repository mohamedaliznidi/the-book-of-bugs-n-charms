---
description: >-
  Complete grimoire for Aceternity UI - a library of Tailwind + Framer Motion enchantments
  for React/Next.js by Manu Arora. Master animated enchantments, 3D mystical visuals, premium features,
  and how to create impressively designed modern mystical websites.

theme: "magic"
---

# Aceternity UI - The 3D Animation Mastery

## The Ancient Knowledge

Aceternity UI is a library of Tailwind + Framer Motion components for React/Next.js created by Manu Arora. It offers dozens of animated UI components and templates aimed at modern web design, with marketing materials claiming it helps "make your websites look 10x better" with copy-paste components including cards, layouts, and 3D visuals.

## Key Features

- **Modern Animations**: Heavily animated components with smooth transitions
- **3D Visual Effects**: Advanced 3D components and interactive visuals
- **Tailwind Integration**: Built with Tailwind CSS for easy customization
- **Framer Motion Powered**: Professional-grade animations and interactions
- **Copy-Paste Components**: Ready-to-use components with minimal setup
- **Premium Options**: Free components plus Pro plan with 70+ premium packs
- **Interactive Playground**: Live preview and experimentation environment

## Use Cases

1. **Tech Product Showcases**: Interactive company "About" pages with 3D elements
2. **Modern Landing Pages**: Impressive first impressions with advanced animations
3. **Portfolio Websites**: Creative showcases with unique visual effects
4. **SaaS Marketing**: Professional product pages with engaging interactions
5. **Creative Agencies**: Visually striking websites that stand out
6. **Interactive Presentations**: Dynamic content for conferences and demos

## Installation and Setup

### 1. Prerequisites

Set up a Next.js project with Tailwind CSS and Framer Motion:

```bash
# Create Next.js project
pnpm create next-app@latest my-app --typescript --tailwind --eslint
cd my-app

# Install Framer Motion
pnpm add framer-motion

# Install additional dependencies for 3D effects
pnpm add three @react-three/fiber @react-three/drei
```

### 2. Install Aceternity UI

```bash
# Install via npm
npm install aceternity-ui

# Or copy components directly from ui.aceternity.com
```

### 3. Configure Tailwind CSS

Add Aceternity UI to your `tailwind.config.js`:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    './pages/**/*.{js,ts,jsx,tsx,mdx}',
    './components/**/*.{js,ts,jsx,tsx,mdx}',
    './app/**/*.{js,ts,jsx,tsx,mdx}',
    './node_modules/aceternity-ui/**/*.{js,ts,jsx,tsx}', // Add this line
  ],
  theme: {
    extend: {
      animation: {
        'gradient-x': 'gradient-x 15s ease infinite',
        'gradient-y': 'gradient-y 15s ease infinite',
        'gradient-xy': 'gradient-xy 15s ease infinite',
      },
      keyframes: {
        'gradient-y': {
          '0%, 100%': {
            'background-size': '400% 400%',
            'background-position': 'center top'
          },
          '50%': {
            'background-size': '200% 200%',
            'background-position': 'center center'
          }
        },
        'gradient-x': {
          '0%, 100%': {
            'background-size': '200% 200%',
            'background-position': 'left center'
          },
          '50%': {
            'background-size': '200% 200%',
            'background-position': 'right center'
          }
        },
        'gradient-xy': {
          '0%, 100%': {
            'background-size': '400% 400%',
            'background-position': 'left center'
          },
          '50%': {
            'background-size': '200% 200%',
            'background-position': 'right center'
          }
        }
      }
    },
  },
  plugins: [],
}
```

## Core Components

### 1. 3D Card Effects

Interactive cards with 3D hover effects:

```typescript
import { CardContainer, CardBody, CardItem } from "aceternity-ui/3d-card"
import { Button } from "@/components/ui/button"

export default function ThreeDCardDemo() {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
      <CardContainer className="inter-var">
        <CardBody className="bg-gray-50 relative group/card dark:hover:shadow-2xl dark:hover:shadow-emerald-500/[0.1] dark:bg-black dark:border-white/[0.2] border-black/[0.1] w-auto sm:w-[30rem] h-auto rounded-xl p-6 border">
          <CardItem
            translateZ="50"
            className="text-xl font-bold text-neutral-600 dark:text-white"
          >
            Make things float in air
          </CardItem>
          <CardItem
            as="p"
            translateZ="60"
            className="text-neutral-500 text-sm max-w-sm mt-2 dark:text-neutral-300"
          >
            Hover over this card to unleash the power of CSS perspective
          </CardItem>
          <CardItem translateZ="100" className="w-full mt-4">
            <img
              src="/placeholder-image.jpg"
              height="1000"
              width="1000"
              className="h-60 w-full object-cover rounded-xl group-hover/card:shadow-xl"
              alt="thumbnail"
            />
          </CardItem>
          <div className="flex justify-between items-center mt-20">
            <CardItem
              translateZ={20}
              as="button"
              className="px-4 py-2 rounded-xl text-xs font-normal dark:text-white"
            >
              Try now →
            </CardItem>
            <CardItem
              translateZ={20}
              as="button"
              className="px-4 py-2 rounded-xl bg-black dark:bg-white dark:text-black text-white text-xs font-bold"
            >
              Sign up
            </CardItem>
          </div>
        </CardBody>
      </CardContainer>
    </div>
  )
}
```

### 2. Interactive Globe

3D globe component for global presence visualization:

```typescript
import { Globe } from "aceternity-ui/globe"
import { motion } from "framer-motion"

const globeConfig = {
  pointSize: 4,
  globeColor: "#062056",
  showAtmosphere: true,
  atmosphereColor: "#FFFFFF",
  atmosphereAltitude: 0.1,
  emissive: "#062056",
  emissiveIntensity: 0.1,
  shininess: 0.9,
  polygonColor: "rgba(255,255,255,0.7)",
  ambientLight: "#38bdf8",
  directionalLeftLight: "#ffffff",
  directionalTopLight: "#ffffff",
  pointLight: "#ffffff",
  arcTime: 1000,
  arcLength: 0.9,
  rings: 1,
  maxRings: 3,
  initialPosition: { lat: 22.3193, lng: 114.1694 },
  autoRotate: true,
  autoRotateSpeed: 0.5,
}

const sampleArcs = [
  {
    order: 1,
    startLat: -19.885592,
    startLng: -43.951191,
    endLat: -22.9068,
    endLng: -43.1729,
    arcAlt: 0.1,
    color: "#06b6d4",
  },
  {
    order: 1,
    startLat: 28.6139,
    startLng: 77.2090,
    endLat: 3.139,
    endLng: 101.6869,
    arcAlt: 0.2,
    color: "#06b6d4",
  },
  {
    order: 1,
    startLat: -19.885592,
    startLng: -43.951191,
    endLat: -1.303396,
    endLng: 36.852443,
    arcAlt: 0.5,
    color: "#06b6d4",
  },
]

export default function GlobeDemo() {
  return (
    <div className="flex flex-row items-center justify-center py-20 h-screen md:h-auto dark:bg-black bg-white relative w-full">
      <div className="max-w-7xl mx-auto w-full relative overflow-hidden h-full md:h-[40rem] px-4">
        <motion.div
          initial={{
            opacity: 0,
            y: 20,
          }}
          animate={{
            opacity: 1,
            y: 0,
          }}
          transition={{
            duration: 1,
          }}
          className="div"
        >
          <h2 className="text-center text-xl md:text-4xl font-bold text-black dark:text-white">
            We sell soap worldwide
          </h2>
          <p className="text-center text-base md:text-lg font-normal text-neutral-700 dark:text-neutral-200 max-w-md mt-2 mx-auto">
            This globe is interactive and customizable. Have fun with it, and
            don&apos;t forget to share it. :)
          </p>
        </motion.div>
        <div className="absolute w-full bottom-0 inset-x-0 h-40 bg-gradient-to-b pointer-events-none select-none from-transparent dark:to-black to-white z-40" />
        <div className="absolute w-full -bottom-20 h-72 md:h-full z-10">
          <Globe
            config={globeConfig}
            data={sampleArcs}
          />
        </div>
      </div>
    </div>
  )
}
```

### 3. Animated Testimonials

Dynamic testimonial cards with smooth animations:

```typescript
import { InfiniteMovingCards } from "aceternity-ui/infinite-moving-cards"

const testimonials = [
  {
    quote: "It was the best of times, it was the worst of times, it was the age of wisdom, it was the age of foolishness, it was the epoch of belief, it was the epoch of incredulity, it was the season of Light, it was the season of Darkness, it was the spring of hope, it was the winter of despair.",
    name: "Charles Dickens",
    title: "A Tale of Two Cities",
  },
  {
    quote: "To be, or not to be, that is the question: Whether 'tis nobler in the mind to suffer The slings and arrows of outrageous fortune, Or to take Arms against a Sea of troubles, And by opposing end them: to die, to sleep.",
    name: "William Shakespeare",
    title: "Hamlet",
  },
  {
    quote: "All that we see or seem is but a dream within a dream.",
    name: "Edgar Allan Poe",
    title: "A Dream Within a Dream",
  },
  {
    quote: "It is a truth universally acknowledged, that a single man in possession of a good fortune, must be in want of a wife.",
    name: "Jane Austen",
    title: "Pride and Prejudice",
  },
  {
    quote: "Call me Ishmael. Some years ago—never mind how long precisely—having little or no money in my purse, and nothing particular to interest me on shore, I thought I would sail about a little and see the watery part of the world.",
    name: "Herman Melville",
    title: "Moby-Dick",
  },
]

export default function TestimonialsDemo() {
  return (
    <div className="h-[40rem] rounded-md flex flex-col antialiased bg-white dark:bg-black dark:bg-grid-white/[0.05] items-center justify-center relative overflow-hidden">
      <InfiniteMovingCards
        items={testimonials}
        direction="right"
        speed="slow"
      />
    </div>
  )
}
```

### 4. Hero Section with Spotlight

Dramatic hero section with spotlight effects:

```typescript
import { Spotlight } from "aceternity-ui/spotlight"
import { Button } from "@/components/ui/button"
import { motion } from "framer-motion"

export default function SpotlightHero() {
  return (
    <div className="h-screen w-full rounded-md flex md:items-center md:justify-center bg-black/[0.96] antialiased bg-grid-white/[0.02] relative overflow-hidden">
      <Spotlight
        className="-top-40 left-0 md:left-60 md:-top-20"
        fill="white"
      />
      <div className="p-4 max-w-7xl mx-auto relative z-10 w-full pt-20 md:pt-0">
        <motion.h1 
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.8 }}
          className="text-4xl md:text-7xl font-bold text-center bg-clip-text text-transparent bg-gradient-to-b from-neutral-50 to-neutral-400 bg-opacity-50"
        >
          Spotlight <br /> is the new feature.
        </motion.h1>
        <motion.p 
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.8, delay: 0.2 }}
          className="mt-4 font-normal text-base text-neutral-300 max-w-lg text-center mx-auto"
        >
          Spotlight effect is a great way to draw attention to a specific part
          of the page. Here, we are drawing the attention towards the text.
        </motion.p>
        <motion.div
          initial={{ opacity: 0, y: 20 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.8, delay: 0.4 }}
          className="flex justify-center mt-8"
        >
          <Button size="lg" className="bg-gradient-to-r from-blue-500 to-purple-600 hover:from-blue-600 hover:to-purple-700">
            Get Started
          </Button>
        </motion.div>
      </div>
    </div>
  )
}
```

## Advanced Features

### Custom 3D Animations

Create custom 3D effects using React Three Fiber:

```typescript
import { Canvas } from '@react-three/fiber'
import { OrbitControls, Sphere, MeshDistortMaterial } from '@react-three/drei'
import { motion } from 'framer-motion-3d'

function AnimatedSphere() {
  return (
    <motion.mesh
      initial={{ scale: 0 }}
      animate={{ scale: 1 }}
      transition={{ duration: 1 }}
    >
      <Sphere args={[1, 100, 200]} scale={2}>
        <MeshDistortMaterial
          color="#8352FD"
          attach="material"
          distort={0.5}
          speed={1.5}
          roughness={0}
        />
      </Sphere>
    </motion.mesh>
  )
}

export default function Custom3DScene() {
  return (
    <div className="h-screen w-full">
      <Canvas>
        <OrbitControls enableZoom={false} />
        <ambientLight intensity={1} />
        <directionalLight position={[3, 2, 1]} />
        <AnimatedSphere />
      </Canvas>
    </div>
  )
}
```

### Interactive Background Effects

Dynamic backgrounds that respond to user interaction:

```typescript
import { BackgroundBeams } from "aceternity-ui/background-beams"
import { motion, useMotionValue, useSpring, useTransform } from "framer-motion"
import { useRef } from "react"

export default function InteractiveBackground() {
  const ref = useRef<HTMLDivElement>(null)
  const x = useMotionValue(0)
  const y = useMotionValue(0)
  
  const mouseXSpring = useSpring(x)
  const mouseYSpring = useSpring(y)
  
  const rotateX = useTransform(mouseYSpring, [-0.5, 0.5], ["17.5deg", "-17.5deg"])
  const rotateY = useTransform(mouseXSpring, [-0.5, 0.5], ["-17.5deg", "17.5deg"])

  const handleMouseMove = (e: React.MouseEvent<HTMLDivElement>) => {
    if (!ref.current) return
    
    const rect = ref.current.getBoundingClientRect()
    const width = rect.width
    const height = rect.height
    const mouseX = e.clientX - rect.left
    const mouseY = e.clientY - rect.top
    const xPct = mouseX / width - 0.5
    const yPct = mouseY / height - 0.5
    
    x.set(xPct)
    y.set(yPct)
  }

  return (
    <div 
      ref={ref}
      onMouseMove={handleMouseMove}
      className="h-screen w-full bg-neutral-950 relative flex flex-col items-center justify-center antialiased"
    >
      <motion.div
        style={{
          rotateY,
          rotateX,
          transformStyle: "preserve-3d",
        }}
        className="relative"
      >
        <div
          style={{
            transform: "translateZ(75px)",
            transformStyle: "preserve-3d",
          }}
        >
          <h1 className="text-4xl md:text-7xl font-bold text-neutral-200 text-center">
            Interactive 3D
          </h1>
          <p className="text-neutral-400 text-center mt-4 max-w-lg">
            Move your mouse around to see the 3D effect in action
          </p>
        </div>
      </motion.div>
      <BackgroundBeams />
    </div>
  )
}
```

## Premium Features

### Pro Components

Aceternity UI offers premium components with advanced features:

- **Advanced 3D Models**: Complex 3D scenes and interactions
- **Particle Systems**: Sophisticated particle effects and animations
- **Custom Shaders**: WebGL shaders for unique visual effects
- **Interactive Charts**: Animated data visualizations
- **Advanced Layouts**: Complex grid systems and responsive designs

### Pricing Structure

- **Free Tier**: Basic components and templates
- **Pro Plan**: 70+ premium component packs
- **Enterprise**: Custom components and support

## Best Practices

1. **Performance**: Use `will-change` CSS property for animated elements
2. **3D Optimization**: Limit the number of 3D elements on mobile devices
3. **Accessibility**: Provide fallbacks for users with motion sensitivity
4. **Loading**: Implement proper loading states for heavy 3D components
5. **Browser Support**: Test 3D effects across different browsers
6. **Mobile Experience**: Optimize animations for touch devices

## Common Pitfalls

### Issue 1: Performance on Mobile
Heavy 3D animations causing poor performance on mobile devices.

**Solution:**
```typescript
import { useMediaQuery } from "@/hooks/use-media-query"

export default function ResponsiveComponent() {
  const isMobile = useMediaQuery("(max-width: 768px)")
  
  return (
    <div>
      {isMobile ? (
        <SimpleCard /> // Fallback for mobile
      ) : (
        <ThreeDCard /> // Full 3D effect for desktop
      )}
    </div>
  )
}
```

### Issue 2: Bundle Size Impact
Including heavy 3D libraries increases bundle size significantly.

**Solution:**
```typescript
// Use dynamic imports for heavy components
import dynamic from 'next/dynamic'

const Globe = dynamic(() => import('aceternity-ui/globe'), {
  loading: () => <div>Loading globe...</div>,
  ssr: false
})
```

### Issue 3: Browser Compatibility
3D effects not working in older browsers.

**Solution:**
```typescript
// Feature detection and fallbacks
const supports3D = () => {
  const canvas = document.createElement('canvas')
  const gl = canvas.getContext('webgl') || canvas.getContext('experimental-webgl')
  return !!gl
}

export default function ConditionalComponent() {
  const [canRender3D, setCanRender3D] = useState(false)
  
  useEffect(() => {
    setCanRender3D(supports3D())
  }, [])
  
  return canRender3D ? <ThreeDComponent /> : <FallbackComponent />
}
```

## References

{% embed url="https://ui.aceternity.com/" %}

{% embed url="https://www.framer.com/motion/" %}

{% embed url="https://docs.pmnd.rs/react-three-fiber/" %}
