---
description: >-
  Master Framer Motion for React - create smooth animations, page transitions, 
  and interactive gestures with declarative syntax and powerful animation controls.
---

# Framer Motion

## Introduction

Framer Motion is a production-ready motion library for React that makes creating animations simple and intuitive. It provides a declarative API for complex animations, gestures, and layout transitions, allowing you to create smooth, performant animations with minimal code.

## Use Cases

1. **Page Transitions**: Smooth transitions between different pages or views in your application
2. **Component Animations**: Animate component entrance, exit, and state changes
3. **Gesture Handling**: Add drag, hover, tap, and other interactive gestures to elements
4. **Layout Animations**: Automatically animate layout changes when components resize or reposition
5. **Scroll-triggered Animations**: Create animations that respond to scroll position

## Installation

```bash
npm install framer-motion
# or
yarn add framer-motion
# or
pnpm add framer-motion
```

## Basic Animation Example

```jsx
import { motion } from 'framer-motion';

function BasicAnimation() {
  return (
    <motion.div
      initial={{ opacity: 0, y: 50 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.5 }}
      className="p-6 bg-blue-500 text-white rounded-lg"
    >
      Hello, I'm animated!
    </motion.div>
  );
}
```

## Page Transitions

### Simple Page Transition

```jsx
import { motion, AnimatePresence } from 'framer-motion';
import { useRouter } from 'next/router';

const pageVariants = {
  initial: {
    opacity: 0,
    x: '-100vw',
    scale: 0.8
  },
  in: {
    opacity: 1,
    x: 0,
    scale: 1
  },
  out: {
    opacity: 0,
    x: '100vw',
    scale: 1.2
  }
};

const pageTransition = {
  type: 'tween',
  ease: 'anticipate',
  duration: 0.5
};

function PageWrapper({ children }) {
  const router = useRouter();
  
  return (
    <AnimatePresence mode="wait">
      <motion.div
        key={router.pathname}
        initial="initial"
        animate="in"
        exit="out"
        variants={pageVariants}
        transition={pageTransition}
      >
        {children}
      </motion.div>
    </AnimatePresence>
  );
}
```

### Advanced Page Transition with Route-based Animations

```jsx
import { motion, AnimatePresence } from 'framer-motion';

const routeVariants = {
  '/': {
    initial: { opacity: 0, y: 100 },
    animate: { opacity: 1, y: 0 },
    exit: { opacity: 0, y: -100 }
  },
  '/about': {
    initial: { opacity: 0, x: 100 },
    animate: { opacity: 1, x: 0 },
    exit: { opacity: 0, x: -100 }
  },
  '/contact': {
    initial: { opacity: 0, scale: 0.8 },
    animate: { opacity: 1, scale: 1 },
    exit: { opacity: 0, scale: 1.2 }
  }
};

function RouteTransition({ children, route }) {
  const variants = routeVariants[route] || routeVariants['/'];
  
  return (
    <AnimatePresence mode="wait">
      <motion.div
        key={route}
        initial="initial"
        animate="animate"
        exit="exit"
        variants={variants}
        transition={{ duration: 0.3, ease: 'easeInOut' }}
      >
        {children}
      </motion.div>
    </AnimatePresence>
  );
}
```

## Component Animations

### List Animations

```jsx
import { motion, AnimatePresence } from 'framer-motion';

const listVariants = {
  hidden: {
    opacity: 0
  },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1
    }
  }
};

const itemVariants = {
  hidden: {
    opacity: 0,
    y: 20
  },
  visible: {
    opacity: 1,
    y: 0
  }
};

function AnimatedList({ items }) {
  return (
    <motion.ul
      variants={listVariants}
      initial="hidden"
      animate="visible"
      className="space-y-2"
    >
      <AnimatePresence>
        {items.map((item) => (
          <motion.li
            key={item.id}
            variants={itemVariants}
            exit={{ opacity: 0, x: -100 }}
            layout
            className="p-3 bg-gray-100 rounded"
          >
            {item.text}
          </motion.li>
        ))}
      </AnimatePresence>
    </motion.ul>
  );
}
```

### Modal Animation

```jsx
import { motion, AnimatePresence } from 'framer-motion';

const backdropVariants = {
  visible: { opacity: 1 },
  hidden: { opacity: 0 }
};

const modalVariants = {
  hidden: {
    y: '-100vh',
    opacity: 0
  },
  visible: {
    y: '0',
    opacity: 1,
    transition: {
      duration: 0.1,
      type: 'spring',
      damping: 25,
      stiffness: 500
    }
  },
  exit: {
    y: '100vh',
    opacity: 0
  }
};

function Modal({ isOpen, onClose, children }) {
  return (
    <AnimatePresence>
      {isOpen && (
        <motion.div
          className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50"
          variants={backdropVariants}
          initial="hidden"
          animate="visible"
          exit="hidden"
          onClick={onClose}
        >
          <motion.div
            className="bg-white p-6 rounded-lg max-w-md w-full mx-4"
            variants={modalVariants}
            onClick={(e) => e.stopPropagation()}
          >
            {children}
          </motion.div>
        </motion.div>
      )}
    </AnimatePresence>
  );
}
```

## Gesture Handling

### Drag and Drop

```jsx
import { motion } from 'framer-motion';

function DraggableCard() {
  return (
    <motion.div
      drag
      dragConstraints={{
        top: -50,
        left: -50,
        right: 50,
        bottom: 50,
      }}
      dragElastic={0.2}
      whileDrag={{ scale: 1.1, rotate: 5 }}
      className="w-32 h-32 bg-gradient-to-r from-purple-400 to-pink-400 rounded-lg cursor-grab active:cursor-grabbing flex items-center justify-center text-white font-bold"
    >
      Drag me!
    </motion.div>
  );
}
```

### Interactive Button

```jsx
import { motion } from 'framer-motion';

function InteractiveButton({ children, onClick }) {
  return (
    <motion.button
      whileHover={{ 
        scale: 1.05,
        boxShadow: '0px 5px 15px rgba(0, 0, 0, 0.2)'
      }}
      whileTap={{ scale: 0.95 }}
      transition={{ type: 'spring', stiffness: 400, damping: 17 }}
      onClick={onClick}
      className="px-6 py-3 bg-blue-500 text-white rounded-lg font-medium"
    >
      {children}
    </motion.button>
  );
}
```

### Hover Card

```jsx
import { motion } from 'framer-motion';

function HoverCard({ title, description, image }) {
  return (
    <motion.div
      whileHover="hover"
      className="relative overflow-hidden rounded-lg bg-white shadow-lg cursor-pointer"
    >
      <motion.img
        src={image}
        alt={title}
        className="w-full h-48 object-cover"
        variants={{
          hover: { scale: 1.1 }
        }}
        transition={{ duration: 0.3 }}
      />
      <motion.div
        className="absolute inset-0 bg-black bg-opacity-0 flex items-center justify-center"
        variants={{
          hover: { backgroundColor: 'rgba(0, 0, 0, 0.7)' }
        }}
      >
        <motion.div
          className="text-white text-center opacity-0"
          variants={{
            hover: { opacity: 1, y: 0 }
          }}
          initial={{ y: 20 }}
        >
          <h3 className="text-xl font-bold mb-2">{title}</h3>
          <p className="text-sm">{description}</p>
        </motion.div>
      </motion.div>
    </motion.div>
  );
}
```

## Layout Animations

### Automatic Layout Animation

```jsx
import { motion, AnimatePresence } from 'framer-motion';
import { useState } from 'react';

function LayoutAnimation() {
  const [isExpanded, setIsExpanded] = useState(false);

  return (
    <motion.div
      layout
      className={`bg-blue-500 text-white p-4 rounded-lg cursor-pointer ${
        isExpanded ? 'w-80' : 'w-40'
      }`}
      onClick={() => setIsExpanded(!isExpanded)}
    >
      <motion.h3 layout className="font-bold">
        Click to expand
      </motion.h3>
      <AnimatePresence>
        {isExpanded && (
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            className="mt-4"
          >
            <p>This content appears when expanded!</p>
            <p>Layout animations handle the size change automatically.</p>
          </motion.div>
        )}
      </AnimatePresence>
    </motion.div>
  );
}
```

## Scroll-triggered Animations

### Scroll-based Reveal

```jsx
import { motion, useInView } from 'framer-motion';
import { useRef } from 'react';

function ScrollReveal({ children }) {
  const ref = useRef(null);
  const isInView = useInView(ref, { once: true, margin: '-100px' });

  return (
    <motion.div
      ref={ref}
      initial={{ opacity: 0, y: 75 }}
      animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 75 }}
      transition={{ duration: 0.5, delay: 0.25 }}
    >
      {children}
    </motion.div>
  );
}
```

### Parallax Scroll Effect

```jsx
import { motion, useScroll, useTransform } from 'framer-motion';

function ParallaxSection() {
  const { scrollYProgress } = useScroll();
  const y = useTransform(scrollYProgress, [0, 1], ['0%', '50%']);
  const opacity = useTransform(scrollYProgress, [0, 0.5, 1], [1, 0.5, 0]);

  return (
    <motion.div
      style={{ y, opacity }}
      className="h-screen bg-gradient-to-b from-blue-400 to-purple-600 flex items-center justify-center"
    >
      <h1 className="text-6xl font-bold text-white">Parallax Effect</h1>
    </motion.div>
  );
}
```

## Advanced Patterns

### Custom Hook for Animations

```jsx
import { useAnimation } from 'framer-motion';
import { useEffect } from 'react';

function useSequentialAnimation(trigger) {
  const controls = useAnimation();

  useEffect(() => {
    if (trigger) {
      const sequence = async () => {
        await controls.start({ scale: 1.2, transition: { duration: 0.2 } });
        await controls.start({ scale: 1, transition: { duration: 0.2 } });
        await controls.start({ rotate: 360, transition: { duration: 0.5 } });
        await controls.start({ rotate: 0, transition: { duration: 0.2 } });
      };
      sequence();
    }
  }, [trigger, controls]);

  return controls;
}

function AnimatedComponent({ isTriggered }) {
  const controls = useSequentialAnimation(isTriggered);

  return (
    <motion.div
      animate={controls}
      className="w-20 h-20 bg-red-500 rounded-full"
    />
  );
}
```

## Best Practices

- **Performance**: Use `transform` and `opacity` properties for the smoothest animations
- **Accessibility**: Respect `prefers-reduced-motion` media query for users who prefer minimal animations
- **Layout Animations**: Use `layout` prop for automatic layout transitions when elements change size or position
- **Exit Animations**: Always wrap components with `AnimatePresence` when they need exit animations
- **Stagger Effects**: Use `staggerChildren` in parent variants for coordinated child animations
- **Gesture Constraints**: Always set appropriate `dragConstraints` for draggable elements

## Common Pitfalls

### Issue 1: Exit Animations Not Working
Exit animations require `AnimatePresence` wrapper and proper key management.

**Solution:**
```jsx
<AnimatePresence mode="wait">
  {showComponent && (
    <motion.div
      key="unique-key"
      exit={{ opacity: 0 }}
    >
      Content
    </motion.div>
  )}
</AnimatePresence>
```

### Issue 2: Layout Shift During Animations
Animations can cause layout shifts if not handled properly.

**Solution:**
Use `position: absolute` or reserve space for animated elements:
```jsx
<motion.div
  initial={{ opacity: 0, position: 'absolute' }}
  animate={{ opacity: 1, position: 'relative' }}
>
  Content
</motion.div>
```

### Issue 3: Performance Issues with Complex Animations
Too many simultaneous animations can cause performance problems.

**Solution:**
Use `will-change` CSS property and limit concurrent animations:
```jsx
<motion.div
  style={{ willChange: 'transform' }}
  animate={{ x: 100 }}
>
  Content
</motion.div>
```

## References

{% embed url="https://www.framer.com/motion/" %}

{% embed url="https://www.framer.com/motion/examples/" %}
