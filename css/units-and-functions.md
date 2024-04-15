# Units & Functions

## CSS Units

Choosing the right units in CSS is crucial for crafting responsive and visually appealing websites. This guide dives into popular units, with helpful visuals to solidify your understanding.

### **Pixel Perfect (px):**

Imagine tiny squares on your screen! Each `px` is one of those squares, but its actual size depends on your device's pixel density. Great for precise layouts on specific devices, but tricky for responsiveness.

!\[pixel unit graphic, representing a grid of squares on a screen]

### **Percentage (%):**

Think of % as a slice of the pie. It's relative to another element's size, making it scale beautifully across different screens. Perfect for setting margins, padding, and element dimensions without pixel headaches.

!\[percentage unit graphic, showing a pie chart with slices representing different percentages]

### **Emulating the Parent (em):**

Em follows the footsteps of its parent element. 1em equals the parent's font size, inheriting its responsiveness. Ideal for nested elements where consistent sizing matters.

!\[em unit graphic, with arrows pointing from a parent element's font size to child elements with "1em" labels]

### **Rooting for Responsiveness (rem):**

Rem takes things to the root, basing itself on the document's **base font size**. This makes rem truly responsive, unaffected by parent element fluctuations. Your best friend for fluid layouts!

!\[rem unit graphic, with an arrow pointing from the document's base font size to child elements with "1rem" labels]

### **Viewport Wonders (vw, vh, vmin, vmax):**

Think of the viewport as your canvas. vw and vh paint on it horizontally and vertically, with 1 unit being 1% of the respective dimension. vmin and vmax adapt to the smaller or larger dimension, respectively, maximizing flexibility.

!\[viewport unit graphic, showing a rectangle representing the viewport with percentages labeled along the sides for vw, vh, vmin, and vmax]

### **Beyond Pixels:**

Beyond the digital realm, CSS offers real-world units like centimeters (cm), millimeters (mm), inches (in), points (pt), and picas (pc). These are perfect for print layouts or designs that need precise physical dimensions.

Remember, choosing the right unit is key to responsive design. Experiment, mix and match, and don't forget to test across different devices to ensure your website shines everywhere!

This wiki is just the beginning. As you explore and master CSS units, you'll unlock a world of creative possibilities for your web creations!

## CSS Functions

Unveils the magic of CSS functions, empowering you to craft dynamic, responsive, and visually captivating websites. Delve into bite-sized explanations, practical code examples, and clear headings to master essential tools like animation, responsive images, and adaptive layouts. Elevate your web design expertise, one function at a time.

### **url(): Image Chameleon**

Imagine using one `img` tag but displaying different images based on screen size or user preference? `url()` makes it possible! Simply feed it the path to an image (or even multiple images like a favicon set!), and watch your element transform.

```css
body {
  background-image: url("img/background-large.jpg");
}

@media (max-width: 768px) {
  body {
    background-image: url("img/background-small.jpg");
  }
}
```

### **attr(): Content Thief**

Need to display the author's name from an `<h2>` tag within your footer? `attr()` comes to the rescue! It steals the value of any HTML attribute and injects it into your CSS. No more copy-pasting content – let the code do the work!

```css
footer {
  content: "Article by " attr(h2, author);
}
```

### **calc(): Math Magician**

Tired of pixel-perfect calculations? `calc()` lets you use basic math (+, -, \*, /) right within your CSS. Need a button 50% wider than its height? Easy! Just `calc(height * 1.5)`. No more messy calculations or magic numbers.

```css
button {
  width: calc(height * 1.5);
}
```

### **lang(): Language Tailor**

Building a multilingual website? `lang()` helps target specific layouts or styles based on the language used in your HTML. Need French text to be slightly larger? Simply target elements with a `lang="fr"` attribute!

```css
h1[lang="fr"] {
  font-size: 1.2em;
}
```

### **not(): Negation Ninja**

Want to target everything _except_ a specific element? `not()` is your new best friend! Need to show a "Login" button only when not logged in? Use `:not(.logged-in)` to hide it for logged-in users.

```css
.cta {
  display: none;
}

.cta:not(.logged-in) {
  display: block;
}
```

### **var(): The Master of Reuse**

Need a color scheme that's easily adaptable throughout your website? Set it as a custom property and use `var()` to reference it anywhere in your CSS, ensuring consistency and easy updates.

```css
:root {
  --primary-color: #007bff;
}

button {
  background-color: var(--primary-color);
}
```

### **rgb(), rgba(), hsl(), hsla(): Color Code Wizards**

Unlock endless color possibilities with these functions! Define colors numerically for precise control and flexibility. Add optional alpha channels for transparency effects.

```css
body {
  background-color: rgb(255, 204, 0); /* Vivid orange */
}

header {
  background-color: hsla(240, 100%, 50%, 0.8); /* Semi-transparent blue */
}
```

### **cubic-bezier(): Animation Choreographer**

Take charge of animation timing with `cubic-bezier()`! Create unique easing effects for smooth transitions, bouncy elements, or dramatic pauses. Be the director of your animations!

```css
.box {
  animation: bounce 1s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
```

### **clip-path(), offset-path(): Shape Shifters**

Carve out unique shapes and paths using SVG syntax right within CSS! Create captivating visual effects, unconventional layouts, and dynamic element interactions with these versatile functions.

```scss
img {
  clip-path: circle(50% at 50% 50%); /* Circular image */
}
```

### **steps(): Easing Maestro**

Transform smooth animations into distinct steps for a retro, pixelated aesthetic or emphasize specific animation states. Create eye-catching visual rhythms with this powerful function.

```css
.progress-bar {
  animation: progress 5s steps(10); /* 10 distinct steps */
}
```

### **scale(), translate(), rotate(), skew(): The Transformation Quartet**

Turn your elements into acrobats with this dynamic squad! Combine them to create mind-bending animations, subtle hover effects, or even 3D illusions. Resize, reposition, spin, and slant any element with ease!

```css
.card:hover {
  transform: scale(1.1) rotate(5deg);
}
```

### **filter(): Instagram Envy**

Who needs Photoshop when you have `filter()`? Apply blurs, brightness adjustments, color shifts, and even grayscale effects directly in CSS. Create artistic visuals, enhance readability, or craft unique design elements without external tools.

```css
img {
  filter: blur(5px);
}
```

### **clamp(), max(), min(): Value Guardians**

These functions ensure your styles stay within bounds, preventing unwanted overflows or layout mishaps. Set minimum and maximum values for properties, or even clamp them within a specific range, for a more controlled and responsive experience.

```css
.container {
  width: clamp(300px, 80vw, 1200px); /* Width between 300px and 1200px, adapting to viewport */
}
```

### **is(), where(): Selector Whisperers**

Need to target multiple elements with similar characteristics? These pseudo-class selectors let you group and target elements based on shared attributes, classes, or relationships, streamlining your CSS and making it more maintainable.

```css
button:is(.primary, .secondary) {
  background-color: blue;
}
```

### **image-set(): The Adaptive Image Curator**

Deliver the perfect image for every device and connection speed! `image-set()` lets you specify multiple images, and the browser intelligently chooses the most suitable one based on screen resolution and network conditions, ensuring optimal visual quality and performance.

```css
.hero-image {
  background-image: image-set(
    url("image-small.jpg") 1x,
    url("image-medium.jpg") 2x,
    url("image-large.jpg") 3x
  );
}
```

### **::slotted(): The Shadow DOM Detective**

Working with shadow DOM? `::slotted()` lets you style elements slotted into your shadow roots from the outside, ensuring consistency and maintainability in complex component structures. It's like a secret handshake for parent and child components!

```css
#my-component::slotted(h2) {
  color: red;
}
```

These functions help in creating dynamic, responsive, and visually appealing websites. However, it's important to note that not all CSS functions have the same level of browser support, so it's crucial to test and ensure your experience works for everyone.

## References

{% embed url="https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Values_and_units#numbers_lengths_and_percentages" %}

{% embed url="https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Functions" %}
