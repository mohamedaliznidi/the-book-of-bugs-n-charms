---
description: >-
  p5.js is a JavaScript library for creative coding that makes coding accessible 
  for artists, designers, educators, and beginners with a focus on visual arts.
---

# p5.js

## Introduction

p5.js is a JavaScript library for creative coding, with a focus on making coding accessible and inclusive for artists, designers, educators, beginners, and anyone else. It's based on the core principles of Processing, providing a full set of drawing functionality and interactive features for creating visual art, animations, and interactive experiences in the web browser.

## Use Cases

1. **Interactive Art**: Create dynamic, responsive visual art pieces
2. **Data Visualization**: Transform data into engaging visual representations
3. **Educational Tools**: Build interactive learning experiences and simulations
4. **Game Development**: Create simple 2D games and interactive experiences
5. **Generative Art**: Use algorithms to create unique artistic compositions
6. **Animation**: Build smooth, frame-based animations and motion graphics

## Installation and Setup

### CDN (Quick Start)

```html
<!DOCTYPE html>
<html>
<head>
    <title>p5.js Sketch</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.7.0/p5.min.js"></script>
</head>
<body>
    <script>
        function setup() {
            createCanvas(800, 600);
        }

        function draw() {
            background(220);
            ellipse(mouseX, mouseY, 50, 50);
        }
    </script>
</body>
</html>
```

### NPM Installation

```bash
npm install p5
```

```javascript
import p5 from 'p5';

const sketch = (p) => {
    p.setup = () => {
        p.createCanvas(800, 600);
    };

    p.draw = () => {
        p.background(220);
        p.ellipse(p.mouseX, p.mouseY, 50, 50);
    };
};

new p5(sketch);
```

### Local Development Setup

```html
<!DOCTYPE html>
<html>
<head>
    <title>p5.js Project</title>
    <script src="p5.min.js"></script>
    <style>
        body {
            margin: 0;
            padding: 20px;
            font-family: Arial, sans-serif;
        }
        main {
            display: flex;
            justify-content: center;
        }
    </style>
</head>
<body>
    <main></main>
    <script src="sketch.js"></script>
</body>
</html>
```

## Basic Concepts

### Setup and Draw Loop

```javascript
// Global variables
let x = 0;
let speed = 2;

function setup() {
    // Runs once at the beginning
    createCanvas(800, 600);
    background(220);
}

function draw() {
    // Runs continuously (60fps by default)
    background(220, 50); // Semi-transparent background for trails
    
    // Draw moving circle
    fill(255, 100, 100);
    ellipse(x, height/2, 50, 50);
    
    // Update position
    x += speed;
    
    // Wrap around screen
    if (x > width + 25) {
        x = -25;
    }
}
```

### Coordinate System and Canvas

```javascript
function setup() {
    createCanvas(800, 600);
    
    // Canvas coordinates:
    // (0, 0) is top-left corner
    // (width, height) is bottom-right corner
    
    background(240);
    
    // Draw coordinate reference
    stroke(200);
    strokeWeight(1);
    
    // Grid lines
    for (let i = 0; i <= width; i += 50) {
        line(i, 0, i, height);
    }
    for (let i = 0; i <= height; i += 50) {
        line(0, i, width, i);
    }
    
    // Center point
    stroke(255, 0, 0);
    strokeWeight(3);
    point(width/2, height/2);
    
    // Corner markers
    stroke(0, 255, 0);
    strokeWeight(5);
    point(0, 0);           // Top-left
    point(width, 0);       // Top-right
    point(0, height);      // Bottom-left
    point(width, height);  // Bottom-right
}
```

### Colors and Styling

```javascript
function setup() {
    createCanvas(800, 600);
    background(50);
    
    // Color modes
    colorMode(RGB, 255); // Default: RGB values 0-255
    // colorMode(HSB, 360, 100, 100); // HSB: Hue 0-360, Saturation/Brightness 0-100
    
    // Fill colors
    fill(255, 100, 100);        // RGB
    fill(255, 100, 100, 150);   // RGB with alpha (transparency)
    fill('#FF6464');            // Hex color
    fill(color(255, 100, 100)); // Color object
    
    // Stroke colors
    stroke(0);                  // Black stroke
    strokeWeight(3);            // Stroke thickness
    noStroke();                 // No outline
    
    // Examples
    fill(255, 100, 100);
    ellipse(200, 200, 100, 100);
    
    fill(100, 255, 100, 150);
    stroke(0);
    strokeWeight(2);
    rect(300, 150, 100, 100);
    
    noFill();
    stroke(100, 100, 255);
    strokeWeight(5);
    ellipse(500, 200, 100, 100);
}
```

## Drawing Shapes and Primitives

### Basic Shapes

```javascript
function setup() {
    createCanvas(800, 600);
    background(240);
    
    // Points
    stroke(255, 0, 0);
    strokeWeight(8);
    point(50, 50);
    point(100, 50);
    
    // Lines
    stroke(0);
    strokeWeight(2);
    line(50, 100, 150, 100);    // Horizontal line
    line(200, 80, 200, 120);    // Vertical line
    line(250, 80, 300, 120);    // Diagonal line
    
    // Rectangles
    fill(255, 200, 200);
    rect(50, 150, 100, 80);           // Basic rectangle
    rect(200, 150, 100, 80, 10);      // Rounded corners
    rectMode(CENTER);                  // Change rect mode
    rect(400, 190, 100, 80);          // Centered rectangle
    rectMode(CORNER);                  // Reset to default
    
    // Ellipses and circles
    fill(200, 255, 200);
    ellipse(100, 300, 80, 60);        // Ellipse
    circle(250, 300, 80);             // Circle (p5.js v1.0+)
    ellipseMode(CORNER);              // Change ellipse mode
    ellipse(350, 270, 80, 60);        // Corner mode ellipse
    ellipseMode(CENTER);              // Reset to default
    
    // Triangles
    fill(200, 200, 255);
    triangle(50, 400, 100, 350, 150, 400);
    
    // Quadrilaterals
    fill(255, 255, 200);
    quad(200, 400, 250, 380, 280, 420, 220, 440);
    
    // Arcs
    fill(255, 200, 255);
    arc(400, 400, 80, 80, 0, PI);           // Half circle
    arc(500, 400, 80, 80, 0, PI/2);         // Quarter circle
    arc(600, 400, 80, 80, PI/4, 3*PI/4);    // Arc segment
}
```

### Custom Shapes with Vertices

```javascript
function setup() {
    createCanvas(800, 600);
    background(240);
    
    // Custom polygon
    fill(255, 150, 150);
    stroke(100);
    strokeWeight(2);
    
    beginShape();
    vertex(200, 100);
    vertex(300, 120);
    vertex(320, 200);
    vertex(280, 250);
    vertex(220, 240);
    vertex(180, 180);
    endShape(CLOSE);
    
    // Star shape
    fill(255, 255, 150);
    translate(500, 200);
    beginShape();
    for (let i = 0; i < 10; i++) {
        let angle = map(i, 0, 10, 0, TWO_PI);
        let radius = i % 2 === 0 ? 50 : 25;
        let x = cos(angle) * radius;
        let y = sin(angle) * radius;
        vertex(x, y);
    }
    endShape(CLOSE);
    
    // Curved shape with bezier vertices
    resetMatrix(); // Reset transformations
    fill(150, 255, 150);
    beginShape();
    vertex(100, 400);
    bezierVertex(150, 350, 200, 350, 250, 400);
    bezierVertex(200, 450, 150, 450, 100, 400);
    endShape();
}
```

## Animation and Movement

### Basic Animation Patterns

```javascript
let angle = 0;
let x = 0;
let y = 300;
let xSpeed = 3;
let ySpeed = 2;

function setup() {
    createCanvas(800, 600);
}

function draw() {
    background(240, 50); // Transparent background for trails
    
    // Oscillating motion (sine wave)
    let oscillateX = width/4 + cos(angle) * 100;
    let oscillateY = 150 + sin(angle * 2) * 50;
    
    fill(255, 100, 100);
    ellipse(oscillateX, oscillateY, 30, 30);
    
    // Linear movement with bouncing
    x += xSpeed;
    y += ySpeed;
    
    // Bounce off edges
    if (x > width - 15 || x < 15) {
        xSpeed *= -1;
    }
    if (y > height - 15 || y < 15) {
        ySpeed *= -1;
    }
    
    fill(100, 255, 100);
    ellipse(x, y, 30, 30);
    
    // Circular motion
    let circleX = width * 3/4 + cos(angle * 3) * 80;
    let circleY = height/2 + sin(angle * 3) * 80;
    
    fill(100, 100, 255);
    ellipse(circleX, circleY, 30, 30);
    
    angle += 0.05;
}
```

### Particle Systems

```javascript
let particles = [];

function setup() {
    createCanvas(800, 600);
    
    // Create initial particles
    for (let i = 0; i < 50; i++) {
        particles.push(new Particle(random(width), random(height)));
    }
}

function draw() {
    background(20, 30);
    
    // Update and display particles
    for (let i = particles.length - 1; i >= 0; i--) {
        let p = particles[i];
        p.update();
        p.display();
        
        // Remove dead particles
        if (p.isDead()) {
            particles.splice(i, 1);
        }
    }
    
    // Add new particles at mouse position
    if (mouseIsPressed) {
        particles.push(new Particle(mouseX, mouseY));
    }
    
    // Display particle count
    fill(255);
    text(`Particles: ${particles.length}`, 10, 20);
}

class Particle {
    constructor(x, y) {
        this.position = createVector(x, y);
        this.velocity = createVector(random(-2, 2), random(-2, 2));
        this.acceleration = createVector(0, 0.1); // Gravity
        this.life = 255;
        this.lifespan = 255;
        this.size = random(5, 15);
        this.color = color(random(100, 255), random(100, 255), random(100, 255));
    }
    
    update() {
        this.velocity.add(this.acceleration);
        this.position.add(this.velocity);
        this.life -= 2;
        
        // Add some randomness
        this.acceleration.add(random(-0.1, 0.1), random(-0.1, 0.1));
        this.acceleration.mult(0.9); // Damping
    }
    
    display() {
        push();
        translate(this.position.x, this.position.y);
        
        // Fade out over time
        let alpha = map(this.life, 0, this.lifespan, 0, 255);
        this.color.setAlpha(alpha);
        
        fill(this.color);
        noStroke();
        ellipse(0, 0, this.size, this.size);
        pop();
    }
    
    isDead() {
        return this.life <= 0;
    }
}
```

### Easing and Smooth Transitions

```javascript
let targetX = 400;
let targetY = 300;
let currentX = 400;
let currentY = 300;
let easing = 0.05;

function setup() {
    createCanvas(800, 600);
}

function draw() {
    background(240);
    
    // Easing towards target
    let dx = targetX - currentX;
    let dy = targetY - currentY;
    
    currentX += dx * easing;
    currentY += dy * easing;
    
    // Draw target (mouse position)
    fill(255, 100, 100, 100);
    noStroke();
    ellipse(mouseX, mouseY, 60, 60);
    
    // Draw current position
    fill(100, 100, 255);
    stroke(0);
    strokeWeight(2);
    ellipse(currentX, currentY, 40, 40);
    
    // Draw connection line
    stroke(150);
    strokeWeight(1);
    line(currentX, currentY, mouseX, mouseY);
    
    // Update target to mouse position
    targetX = mouseX;
    targetY = mouseY;
    
    // Display distance
    let distance = dist(currentX, currentY, mouseX, mouseY);
    fill(0);
    noStroke();
    text(`Distance: ${distance.toFixed(2)}`, 10, 20);
}
```

## Interactivity and User Input

### Mouse Interaction

```javascript
let brushSize = 20;
let brushColor;
let isDrawing = false;

function setup() {
    createCanvas(800, 600);
    background(240);
    brushColor = color(100, 150, 255);
    
    // Instructions
    fill(0);
    text("Click and drag to draw. Press 'c' to clear. Scroll to change brush size.", 10, 20);
}

function draw() {
    // Draw brush preview
    if (!isDrawing) {
        // Restore background at mouse position
        fill(240);
        noStroke();
        ellipse(pmouseX, pmouseY, brushSize + 10, brushSize + 10);
        
        // Draw preview circle
        noFill();
        stroke(0, 100);
        strokeWeight(1);
        ellipse(mouseX, mouseY, brushSize, brushSize);
    }
    
    // Display brush info
    fill(240);
    noStroke();
    rect(0, 0, 200, 40);
    fill(0);
    text(`Brush size: ${brushSize}`, 10, 35);
}

function mousePressed() {
    isDrawing = true;
}

function mouseDragged() {
    if (isDrawing) {
        fill(brushColor);
        noStroke();
        ellipse(mouseX, mouseY, brushSize, brushSize);
    }
}

function mouseReleased() {
    isDrawing = false;
}

function mouseWheel(event) {
    // Change brush size with mouse wheel
    brushSize += event.delta > 0 ? -2 : 2;
    brushSize = constrain(brushSize, 5, 100);
    return false; // Prevent page scrolling
}

function keyPressed() {
    if (key === 'c' || key === 'C') {
        background(240);
        // Redraw instructions
        fill(0);
        text("Click and drag to draw. Press 'c' to clear. Scroll to change brush size.", 10, 20);
    }
    
    // Change colors with number keys
    if (key >= '1' && key <= '5') {
        let colors = [
            color(255, 100, 100),  // Red
            color(100, 255, 100),  // Green
            color(100, 100, 255),  // Blue
            color(255, 255, 100),  // Yellow
            color(255, 100, 255)   // Magenta
        ];
        brushColor = colors[parseInt(key) - 1];
    }
}
```

### Keyboard Controls

```javascript
let player = {
    x: 400,
    y: 300,
    size: 30,
    speed: 5,
    color: color(100, 150, 255)
};

let keys = {};

function setup() {
    createCanvas(800, 600);
}

function draw() {
    background(240);
    
    // Handle continuous key presses
    if (keys['ArrowUp'] || keys['w'] || keys['W']) {
        player.y -= player.speed;
    }
    if (keys['ArrowDown'] || keys['s'] || keys['S']) {
        player.y += player.speed;
    }
    if (keys['ArrowLeft'] || keys['a'] || keys['A']) {
        player.x -= player.speed;
    }
    if (keys['ArrowRight'] || keys['d'] || keys['D']) {
        player.x += player.speed;
    }
    
    // Keep player on screen
    player.x = constrain(player.x, player.size/2, width - player.size/2);
    player.y = constrain(player.y, player.size/2, height - player.size/2);
    
    // Draw player
    fill(player.color);
    stroke(0);
    strokeWeight(2);
    ellipse(player.x, player.y, player.size, player.size);
    
    // Draw movement indicator
    noFill();
    stroke(100);
    strokeWeight(1);
    ellipse(player.x, player.y, player.size + 20, player.size + 20);
    
    // Instructions
    fill(0);
    noStroke();
    text("Use WASD or arrow keys to move. Hold SHIFT to run.", 10, 20);
    text(`Position: (${player.x.toFixed(0)}, ${player.y.toFixed(0)})`, 10, 40);
}

function keyPressed() {
    keys[key] = true;
    keys[keyCode] = true;
    
    // Speed boost with shift
    if (keyIsDown(SHIFT)) {
        player.speed = 8;
        player.color = color(255, 100, 100);
    }
}

function keyReleased() {
    keys[key] = false;
    keys[keyCode] = false;
    
    // Reset speed when shift released
    if (!keyIsDown(SHIFT)) {
        player.speed = 5;
        player.color = color(100, 150, 255);
    }
}

## Advanced Graphics and Transformations

### Transformations (Translate, Rotate, Scale)

```javascript
let angle = 0;

function setup() {
    createCanvas(800, 600);
}

function draw() {
    background(240);

    // Save the current transformation matrix
    push();

    // Translate to center
    translate(width/2, height/2);

    // Rotate based on time
    rotate(angle);

    // Scale based on mouse position
    let scaleAmount = map(mouseX, 0, width, 0.5, 2);
    scale(scaleAmount);

    // Draw a shape at the origin (which is now the center)
    fill(255, 100, 100);
    rectMode(CENTER);
    rect(0, 0, 100, 60);

    // Draw additional elements
    fill(100, 255, 100);
    ellipse(50, 0, 20, 20);
    ellipse(-50, 0, 20, 20);

    // Restore the transformation matrix
    pop();

    // Draw something without transformations
    fill(100, 100, 255);
    ellipse(50, 50, 30, 30);

    angle += 0.02;

    // Instructions
    fill(0);
    text("Move mouse to scale the red rectangle", 10, 20);
}
```

### Complex Patterns and Fractals

```javascript
function setup() {
    createCanvas(800, 600);
    background(20);
    noLoop(); // Draw once
}

function draw() {
    // Sierpinski Triangle
    drawSierpinski(width/4, height - 50, 200, 6);

    // Fractal Tree
    push();
    translate(3*width/4, height);
    stroke(100, 255, 100);
    strokeWeight(2);
    drawTree(100, 8);
    pop();
}

function drawSierpinski(x, y, size, depth) {
    if (depth === 0) {
        fill(255, 100, 100, 150);
        noStroke();
        triangle(x, y, x + size/2, y - size * 0.866, x + size, y);
    } else {
        let newSize = size / 2;
        drawSierpinski(x, y, newSize, depth - 1);
        drawSierpinski(x + newSize/2, y - newSize * 0.866, newSize, depth - 1);
        drawSierpinski(x + newSize, y, newSize, depth - 1);
    }
}

function drawTree(length, depth) {
    if (depth > 0) {
        // Draw the branch
        line(0, 0, 0, -length);

        // Move to the end of the branch
        translate(0, -length);

        // Right branch
        push();
        rotate(PI/6);
        drawTree(length * 0.7, depth - 1);
        pop();

        // Left branch
        push();
        rotate(-PI/6);
        drawTree(length * 0.7, depth - 1);
        pop();
    }
}

function mousePressed() {
    background(20);
    redraw();
}
```

### Image Processing and Filters

```javascript
let img;
let originalImg;

function preload() {
    // Load an image (you'll need to provide an image file)
    img = loadImage('https://picsum.photos/400/300');
}

function setup() {
    createCanvas(800, 600);
    originalImg = img.get(); // Make a copy
}

function draw() {
    background(240);

    // Display original image
    image(originalImg, 50, 50, 200, 150);

    // Apply different filters based on mouse position
    let filterType = floor(map(mouseX, 0, width, 0, 6));

    img = originalImg.get(); // Reset to original

    switch(filterType) {
        case 0:
            // No filter
            break;
        case 1:
            img.filter(GRAY);
            break;
        case 2:
            img.filter(INVERT);
            break;
        case 3:
            img.filter(THRESHOLD, 0.5);
            break;
        case 4:
            img.filter(BLUR, 3);
            break;
        case 5:
            img.filter(POSTERIZE, 4);
            break;
    }

    // Display filtered image
    image(img, 300, 50, 200, 150);

    // Labels
    fill(0);
    text("Original", 50, 220);
    text("Filtered (move mouse)", 300, 220);

    let filters = ["None", "Gray", "Invert", "Threshold", "Blur", "Posterize"];
    text(`Current filter: ${filters[filterType]}`, 300, 240);
}
```

## Data Visualization

### Charts and Graphs

```javascript
let data = [];
let maxValue = 0;

function setup() {
    createCanvas(800, 600);

    // Generate sample data
    for (let i = 0; i < 12; i++) {
        data.push({
            month: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
                   'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'][i],
            value: random(20, 100)
        });
    }

    maxValue = Math.max(...data.map(d => d.value));
}

function draw() {
    background(240);

    // Chart dimensions
    let chartX = 100;
    let chartY = 100;
    let chartWidth = 600;
    let chartHeight = 400;

    // Draw chart background
    fill(255);
    stroke(200);
    rect(chartX, chartY, chartWidth, chartHeight);

    // Draw bars
    let barWidth = chartWidth / data.length;

    for (let i = 0; i < data.length; i++) {
        let barHeight = map(data[i].value, 0, maxValue, 0, chartHeight);
        let x = chartX + i * barWidth;
        let y = chartY + chartHeight - barHeight;

        // Color based on value
        let hue = map(data[i].value, 0, maxValue, 240, 0); // Blue to red
        colorMode(HSB);
        fill(hue, 80, 90);
        colorMode(RGB);

        stroke(100);
        rect(x + 5, y, barWidth - 10, barHeight);

        // Value labels
        fill(0);
        textAlign(CENTER);
        text(data[i].value.toFixed(0), x + barWidth/2, y - 5);

        // Month labels
        text(data[i].month, x + barWidth/2, chartY + chartHeight + 20);
    }

    // Chart title
    textAlign(CENTER);
    textSize(16);
    fill(0);
    text("Monthly Data Visualization", width/2, 50);

    // Y-axis labels
    textAlign(RIGHT);
    textSize(12);
    for (let i = 0; i <= 5; i++) {
        let value = map(i, 0, 5, 0, maxValue);
        let y = map(i, 0, 5, chartY + chartHeight, chartY);
        text(value.toFixed(0), chartX - 10, y + 5);

        // Grid lines
        stroke(220);
        line(chartX, y, chartX + chartWidth, y);
    }

    textAlign(LEFT); // Reset text alignment
}

function mousePressed() {
    // Regenerate data
    for (let i = 0; i < data.length; i++) {
        data[i].value = random(20, 100);
    }
    maxValue = Math.max(...data.map(d => d.value));
}
```

### Real-time Data Visualization

```javascript
let dataPoints = [];
let maxPoints = 100;

function setup() {
    createCanvas(800, 600);

    // Initialize with some data
    for (let i = 0; i < maxPoints; i++) {
        dataPoints.push(random(100, 500));
    }
}

function draw() {
    background(20);

    // Add new data point
    dataPoints.push(noise(frameCount * 0.01) * 400 + 100);

    // Remove old data points
    if (dataPoints.length > maxPoints) {
        dataPoints.shift();
    }

    // Draw the line graph
    stroke(100, 255, 100);
    strokeWeight(2);
    noFill();

    beginShape();
    for (let i = 0; i < dataPoints.length; i++) {
        let x = map(i, 0, maxPoints - 1, 50, width - 50);
        let y = map(dataPoints[i], 0, 600, height - 50, 50);
        vertex(x, y);
    }
    endShape();

    // Draw data points
    fill(255, 100, 100);
    noStroke();
    for (let i = 0; i < dataPoints.length; i++) {
        let x = map(i, 0, maxPoints - 1, 50, width - 50);
        let y = map(dataPoints[i], 0, 600, height - 50, 50);
        ellipse(x, y, 4, 4);
    }

    // Draw current value
    let currentValue = dataPoints[dataPoints.length - 1];
    fill(255);
    textAlign(LEFT);
    text(`Current Value: ${currentValue.toFixed(2)}`, 50, 30);
    text(`Data Points: ${dataPoints.length}`, 50, 50);

    // Draw grid
    stroke(50);
    strokeWeight(1);
    for (let i = 0; i <= 10; i++) {
        let y = map(i, 0, 10, height - 50, 50);
        line(50, y, width - 50, y);
    }
    for (let i = 0; i <= 10; i++) {
        let x = map(i, 0, 10, 50, width - 50);
        line(x, 50, x, height - 50);
    }
}
```

## Sound and Audio

### Basic Sound Playback

```javascript
// Note: Requires p5.sound.js library
let sound;
let amplitude;
let fft;

function preload() {
    // Load a sound file (you'll need to provide an audio file)
    sound = loadSound('path/to/your/audio.mp3');
}

function setup() {
    createCanvas(800, 600);

    // Create amplitude and FFT analyzers
    amplitude = new p5.Amplitude();
    fft = new p5.FFT();

    // Connect to the sound
    amplitude.setInput(sound);
    fft.setInput(sound);
}

function draw() {
    background(20);

    // Get amplitude level
    let level = amplitude.getLevel();
    let size = map(level, 0, 1, 50, 300);

    // Draw amplitude visualization
    fill(255, 100, 100);
    noStroke();
    ellipse(width/4, height/2, size, size);

    // Get frequency spectrum
    let spectrum = fft.analyze();

    // Draw frequency bars
    fill(100, 255, 100);
    for (let i = 0; i < spectrum.length; i++) {
        let x = map(i, 0, spectrum.length, width/2, width);
        let h = map(spectrum[i], 0, 255, 0, height/2);
        rect(x, height - h, width / spectrum.length, h);
    }

    // Instructions
    fill(255);
    text("Click to play/pause", 20, 30);
    text(`Volume: ${(level * 100).toFixed(1)}%`, 20, 50);
    text("Amplitude", width/4 - 30, height/2 + size/2 + 20);
    text("Frequency Spectrum", width/2, height - 10);
}

function mousePressed() {
    if (sound.isPlaying()) {
        sound.pause();
    } else {
        sound.play();
    }
}
```

### Generative Music

```javascript
let oscillator;
let envelope;
let scale = [60, 62, 64, 67, 69, 72, 74]; // C major scale (MIDI notes)
let currentNote = 0;
let lastNoteTime = 0;
let noteInterval = 500; // milliseconds

function setup() {
    createCanvas(800, 600);

    // Create oscillator and envelope
    oscillator = new p5.Oscillator('sine');
    envelope = new p5.Envelope();

    // Configure envelope (attack, decay, sustain, release)
    envelope.setADSR(0.1, 0.2, 0.3, 0.5);
    envelope.setRange(0.3, 0);

    oscillator.amp(envelope);
    oscillator.start();

    lastNoteTime = millis();
}

function draw() {
    background(20, 50);

    // Play notes automatically
    if (millis() - lastNoteTime > noteInterval) {
        playRandomNote();
        lastNoteTime = millis();
    }

    // Visual representation
    let freq = midiToFreq(scale[currentNote]);
    let y = map(freq, 200, 800, height - 50, 50);

    fill(100, 255, 100, 150);
    ellipse(width/2, y, 50, 50);

    // Draw scale
    stroke(100);
    for (let i = 0; i < scale.length; i++) {
        let noteFreq = midiToFreq(scale[i]);
        let noteY = map(noteFreq, 200, 800, height - 50, 50);
        line(50, noteY, width - 50, noteY);

        fill(255);
        noStroke();
        text(`Note ${i + 1}`, 60, noteY + 5);
    }

    // Instructions
    fill(255);
    text("Click to play random note", 20, 30);
    text("Press space to change tempo", 20, 50);
    text(`Current tempo: ${(60000/noteInterval).toFixed(1)} BPM`, 20, 70);
}

function playRandomNote() {
    currentNote = floor(random(scale.length));
    let freq = midiToFreq(scale[currentNote]);
    oscillator.freq(freq);
    envelope.play();
}

function mousePressed() {
    playRandomNote();
}

function keyPressed() {
    if (key === ' ') {
        noteInterval = random(200, 1000);
    }
}
```

## Game Development Basics

### Simple Collision Detection Game

```javascript
let player;
let enemies = [];
let score = 0;
let gameState = 'playing'; // 'playing', 'gameOver'

function setup() {
    createCanvas(800, 600);

    // Initialize player
    player = {
        x: width/2,
        y: height/2,
        size: 30,
        speed: 5
    };

    // Create initial enemies
    for (let i = 0; i < 5; i++) {
        createEnemy();
    }
}

function draw() {
    background(20);

    if (gameState === 'playing') {
        updateGame();
        drawGame();
    } else {
        drawGameOver();
    }
}

function updateGame() {
    // Move player with arrow keys
    if (keyIsDown(LEFT_ARROW)) player.x -= player.speed;
    if (keyIsDown(RIGHT_ARROW)) player.x += player.speed;
    if (keyIsDown(UP_ARROW)) player.y -= player.speed;
    if (keyIsDown(DOWN_ARROW)) player.y += player.speed;

    // Keep player on screen
    player.x = constrain(player.x, player.size/2, width - player.size/2);
    player.y = constrain(player.y, player.size/2, height - player.size/2);

    // Update enemies
    for (let i = enemies.length - 1; i >= 0; i--) {
        let enemy = enemies[i];

        // Move towards player
        let dx = player.x - enemy.x;
        let dy = player.y - enemy.y;
        let distance = sqrt(dx*dx + dy*dy);

        enemy.x += (dx / distance) * enemy.speed;
        enemy.y += (dy / distance) * enemy.speed;

        // Check collision with player
        if (distance < (player.size + enemy.size) / 2) {
            gameState = 'gameOver';
        }

        // Remove enemies that are too close (eaten by player)
        if (distance < player.size/2) {
            enemies.splice(i, 1);
            score += 10;
            createEnemy(); // Create new enemy
        }
    }

    // Increase difficulty over time
    if (frameCount % 300 === 0) { // Every 5 seconds
        createEnemy();
    }
}

function drawGame() {
    // Draw player
    fill(100, 255, 100);
    stroke(255);
    strokeWeight(2);
    ellipse(player.x, player.y, player.size, player.size);

    // Draw enemies
    for (let enemy of enemies) {
        fill(255, 100, 100);
        stroke(255);
        ellipse(enemy.x, enemy.y, enemy.size, enemy.size);
    }

    // Draw score
    fill(255);
    textSize(20);
    text(`Score: ${score}`, 20, 30);
    text(`Enemies: ${enemies.length}`, 20, 55);

    // Instructions
    textSize(12);
    text("Use arrow keys to move. Avoid red enemies!", 20, height - 20);
}

function drawGameOver() {
    fill(255, 100, 100);
    textAlign(CENTER);
    textSize(48);
    text("GAME OVER", width/2, height/2 - 50);

    fill(255);
    textSize(24);
    text(`Final Score: ${score}`, width/2, height/2);

    textSize(16);
    text("Press R to restart", width/2, height/2 + 50);

    textAlign(LEFT); // Reset alignment
}

function createEnemy() {
    let side = floor(random(4));
    let enemy = {
        size: random(20, 40),
        speed: random(1, 3)
    };

    // Spawn from random side of screen
    switch(side) {
        case 0: // Top
            enemy.x = random(width);
            enemy.y = -enemy.size;
            break;
        case 1: // Right
            enemy.x = width + enemy.size;
            enemy.y = random(height);
            break;
        case 2: // Bottom
            enemy.x = random(width);
            enemy.y = height + enemy.size;
            break;
        case 3: // Left
            enemy.x = -enemy.size;
            enemy.y = random(height);
            break;
    }

    enemies.push(enemy);
}

function keyPressed() {
    if (key === 'r' || key === 'R') {
        if (gameState === 'gameOver') {
            // Restart game
            gameState = 'playing';
            score = 0;
            enemies = [];
            player.x = width/2;
            player.y = height/2;

            for (let i = 0; i < 5; i++) {
                createEnemy();
            }
        }
    }
}

## Object-Oriented Programming with p5.js

### Creating Classes for Game Objects

```javascript
class Particle {
    constructor(x, y, color) {
        this.position = createVector(x, y);
        this.velocity = createVector(random(-2, 2), random(-2, 2));
        this.acceleration = createVector(0, 0);
        this.size = random(5, 15);
        this.color = color || color(random(255), random(255), random(255));
        this.life = 255;
        this.maxLife = 255;
    }

    update() {
        this.velocity.add(this.acceleration);
        this.position.add(this.velocity);
        this.life -= 2;

        // Reset acceleration
        this.acceleration.mult(0);

        // Add some drag
        this.velocity.mult(0.99);
    }

    applyForce(force) {
        this.acceleration.add(force);
    }

    display() {
        push();
        translate(this.position.x, this.position.y);

        let alpha = map(this.life, 0, this.maxLife, 0, 255);
        this.color.setAlpha(alpha);

        fill(this.color);
        noStroke();
        ellipse(0, 0, this.size, this.size);
        pop();
    }

    isDead() {
        return this.life <= 0;
    }

    edges() {
        if (this.position.x > width) this.position.x = 0;
        if (this.position.x < 0) this.position.x = width;
        if (this.position.y > height) this.position.y = 0;
        if (this.position.y < 0) this.position.y = height;
    }
}

class ParticleSystem {
    constructor(x, y) {
        this.origin = createVector(x, y);
        this.particles = [];
    }

    addParticle() {
        this.particles.push(new Particle(this.origin.x, this.origin.y));
    }

    update() {
        for (let i = this.particles.length - 1; i >= 0; i--) {
            let p = this.particles[i];
            p.update();

            if (p.isDead()) {
                this.particles.splice(i, 1);
            }
        }
    }

    display() {
        for (let particle of this.particles) {
            particle.display();
        }
    }

    applyForce(force) {
        for (let particle of this.particles) {
            particle.applyForce(force);
        }
    }

    getParticleCount() {
        return this.particles.length;
    }
}

// Usage
let particleSystem;

function setup() {
    createCanvas(800, 600);
    particleSystem = new ParticleSystem(width/2, height/2);
}

function draw() {
    background(20, 50);

    // Add particles continuously
    particleSystem.addParticle();

    // Apply gravity
    let gravity = createVector(0, 0.1);
    particleSystem.applyForce(gravity);

    // Update and display
    particleSystem.update();
    particleSystem.display();

    // Display info
    fill(255);
    text(`Particles: ${particleSystem.getParticleCount()}`, 10, 20);
}
```

### State Management for Complex Applications

```javascript
class GameState {
    constructor() {
        this.currentState = 'menu';
        this.states = {
            menu: new MenuState(),
            playing: new PlayingState(),
            paused: new PausedState(),
            gameOver: new GameOverState()
        };
    }

    update() {
        this.states[this.currentState].update();
    }

    display() {
        this.states[this.currentState].display();
    }

    changeState(newState) {
        if (this.states[newState]) {
            this.states[this.currentState].exit();
            this.currentState = newState;
            this.states[this.currentState].enter();
        }
    }

    handleInput(key) {
        this.states[this.currentState].handleInput(key);
    }
}

class MenuState {
    constructor() {
        this.selectedOption = 0;
        this.options = ['Start Game', 'Settings', 'Exit'];
    }

    enter() {
        console.log('Entered menu state');
    }

    exit() {
        console.log('Exited menu state');
    }

    update() {
        // Menu update logic
    }

    display() {
        background(50);

        // Title
        fill(255);
        textAlign(CENTER);
        textSize(48);
        text('GAME TITLE', width/2, 150);

        // Menu options
        textSize(24);
        for (let i = 0; i < this.options.length; i++) {
            if (i === this.selectedOption) {
                fill(255, 255, 100);
                text('> ' + this.options[i] + ' <', width/2, 250 + i * 50);
            } else {
                fill(200);
                text(this.options[i], width/2, 250 + i * 50);
            }
        }

        // Instructions
        textSize(16);
        fill(150);
        text('Use UP/DOWN arrows to navigate, ENTER to select', width/2, height - 50);
    }

    handleInput(key) {
        if (keyCode === UP_ARROW) {
            this.selectedOption = (this.selectedOption - 1 + this.options.length) % this.options.length;
        } else if (keyCode === DOWN_ARROW) {
            this.selectedOption = (this.selectedOption + 1) % this.options.length;
        } else if (keyCode === ENTER) {
            if (this.selectedOption === 0) {
                gameState.changeState('playing');
            }
        }
    }
}

class PlayingState {
    constructor() {
        this.score = 0;
        this.player = { x: 400, y: 300, size: 30 };
    }

    enter() {
        this.score = 0;
        this.player = { x: 400, y: 300, size: 30 };
    }

    exit() {
        // Save score, cleanup, etc.
    }

    update() {
        // Game logic
        if (keyIsDown(LEFT_ARROW)) this.player.x -= 5;
        if (keyIsDown(RIGHT_ARROW)) this.player.x += 5;
        if (keyIsDown(UP_ARROW)) this.player.y -= 5;
        if (keyIsDown(DOWN_ARROW)) this.player.y += 5;

        this.score += 1;
    }

    display() {
        background(20);

        // Draw player
        fill(100, 255, 100);
        ellipse(this.player.x, this.player.y, this.player.size, this.player.size);

        // Draw UI
        fill(255);
        textAlign(LEFT);
        text(`Score: ${this.score}`, 20, 30);
        text('Press P to pause, ESC for menu', 20, 50);
    }

    handleInput(key) {
        if (key === 'p' || key === 'P') {
            gameState.changeState('paused');
        } else if (keyCode === ESCAPE) {
            gameState.changeState('menu');
        }
    }
}

class PausedState {
    enter() {}
    exit() {}

    update() {
        // Paused - no updates
    }

    display() {
        // Draw the game state underneath
        gameState.states.playing.display();

        // Draw pause overlay
        fill(0, 0, 0, 150);
        rect(0, 0, width, height);

        fill(255);
        textAlign(CENTER);
        textSize(48);
        text('PAUSED', width/2, height/2);

        textSize(16);
        text('Press P to resume, ESC for menu', width/2, height/2 + 50);
    }

    handleInput(key) {
        if (key === 'p' || key === 'P') {
            gameState.changeState('playing');
        } else if (keyCode === ESCAPE) {
            gameState.changeState('menu');
        }
    }
}

class GameOverState {
    enter() {
        this.finalScore = gameState.states.playing.score;
    }

    exit() {}

    update() {}

    display() {
        background(100, 0, 0);

        fill(255);
        textAlign(CENTER);
        textSize(48);
        text('GAME OVER', width/2, height/2 - 50);

        textSize(24);
        text(`Final Score: ${this.finalScore}`, width/2, height/2);

        textSize(16);
        text('Press R to restart, ESC for menu', width/2, height/2 + 50);
    }

    handleInput(key) {
        if (key === 'r' || key === 'R') {
            gameState.changeState('playing');
        } else if (keyCode === ESCAPE) {
            gameState.changeState('menu');
        }
    }
}

// Global game state
let gameState;

function setup() {
    createCanvas(800, 600);
    gameState = new GameState();
}

function draw() {
    gameState.update();
    gameState.display();
}

function keyPressed() {
    gameState.handleInput(key);
}
```

## Performance Optimization

### Efficient Drawing Techniques

```javascript
// Avoid expensive operations in draw loop
let precomputedValues = [];
let backgroundGraphics;

function setup() {
    createCanvas(800, 600);

    // Precompute expensive calculations
    for (let i = 0; i < 100; i++) {
        precomputedValues.push({
            x: random(width),
            y: random(height),
            size: random(5, 20),
            color: color(random(255), random(255), random(255))
        });
    }

    // Create background graphics buffer
    backgroundGraphics = createGraphics(width, height);
    drawBackground();
}

function drawBackground() {
    backgroundGraphics.background(20);
    backgroundGraphics.stroke(40);

    // Draw grid once
    for (let i = 0; i < width; i += 50) {
        backgroundGraphics.line(i, 0, i, height);
    }
    for (let i = 0; i < height; i += 50) {
        backgroundGraphics.line(0, i, width, i);
    }
}

function draw() {
    // Use predrawn background
    image(backgroundGraphics, 0, 0);

    // Use precomputed values
    for (let item of precomputedValues) {
        fill(item.color);
        ellipse(item.x, item.y, item.size, item.size);

        // Only update what changes
        item.x += sin(frameCount * 0.01 + item.y * 0.01) * 2;
    }

    // Display performance info
    fill(255);
    text(`FPS: ${frameRate().toFixed(1)}`, 10, 20);
}

// Efficient collision detection using spatial partitioning
class SpatialGrid {
    constructor(cellSize) {
        this.cellSize = cellSize;
        this.grid = new Map();
    }

    clear() {
        this.grid.clear();
    }

    insert(object) {
        let cellX = Math.floor(object.x / this.cellSize);
        let cellY = Math.floor(object.y / this.cellSize);
        let key = `${cellX},${cellY}`;

        if (!this.grid.has(key)) {
            this.grid.set(key, []);
        }
        this.grid.get(key).push(object);
    }

    getNearby(x, y) {
        let nearby = [];
        let cellX = Math.floor(x / this.cellSize);
        let cellY = Math.floor(y / this.cellSize);

        // Check surrounding cells
        for (let dx = -1; dx <= 1; dx++) {
            for (let dy = -1; dy <= 1; dy++) {
                let key = `${cellX + dx},${cellY + dy}`;
                if (this.grid.has(key)) {
                    nearby.push(...this.grid.get(key));
                }
            }
        }

        return nearby;
    }
}
```

## Best Practices

### Code Organization

```javascript
// Use meaningful variable names
let playerPosition = createVector(400, 300);
let enemySpeed = 2;
let gameScore = 0;

// Group related functionality
const GameConfig = {
    CANVAS_WIDTH: 800,
    CANVAS_HEIGHT: 600,
    PLAYER_SPEED: 5,
    ENEMY_SPAWN_RATE: 60,
    MAX_ENEMIES: 10
};

const Colors = {
    BACKGROUND: color(20),
    PLAYER: color(100, 255, 100),
    ENEMY: color(255, 100, 100),
    UI_TEXT: color(255)
};

// Use functions to break down complex operations
function updatePlayer() {
    handlePlayerInput();
    constrainPlayerToBounds();
    updatePlayerAnimation();
}

function handlePlayerInput() {
    if (keyIsDown(LEFT_ARROW)) playerPosition.x -= GameConfig.PLAYER_SPEED;
    if (keyIsDown(RIGHT_ARROW)) playerPosition.x += GameConfig.PLAYER_SPEED;
    if (keyIsDown(UP_ARROW)) playerPosition.y -= GameConfig.PLAYER_SPEED;
    if (keyIsDown(DOWN_ARROW)) playerPosition.y += GameConfig.PLAYER_SPEED;
}

function constrainPlayerToBounds() {
    playerPosition.x = constrain(playerPosition.x, 0, width);
    playerPosition.y = constrain(playerPosition.y, 0, height);
}

// Use comments for complex algorithms
function generatePerlinNoiseTerrain(width, height, scale) {
    let terrain = [];

    // Generate height map using Perlin noise
    for (let x = 0; x < width; x++) {
        terrain[x] = [];
        for (let y = 0; y < height; y++) {
            // Sample noise at scaled coordinates
            let noiseValue = noise(x * scale, y * scale);
            terrain[x][y] = noiseValue;
        }
    }

    return terrain;
}
```

### Error Handling and Debugging

```javascript
// Safe resource loading
function preload() {
    try {
        myImage = loadImage('assets/image.jpg',
            () => console.log('Image loaded successfully'),
            () => console.error('Failed to load image')
        );
    } catch (error) {
        console.error('Error in preload:', error);
    }
}

// Debugging utilities
function drawDebugInfo() {
    if (keyIsDown(68)) { // 'D' key for debug
        fill(255, 255, 0);
        textAlign(LEFT);
        text(`Mouse: (${mouseX}, ${mouseY})`, 10, height - 60);
        text(`Frame: ${frameCount}`, 10, height - 40);
        text(`FPS: ${frameRate().toFixed(1)}`, 10, height - 20);

        // Draw coordinate grid
        stroke(255, 255, 0, 100);
        for (let i = 0; i < width; i += 50) {
            line(i, 0, i, height);
        }
        for (let i = 0; i < height; i += 50) {
            line(0, i, width, i);
        }
    }
}

// Bounds checking
function safeArrayAccess(array, index) {
    if (index >= 0 && index < array.length) {
        return array[index];
    } else {
        console.warn(`Array index ${index} out of bounds`);
        return null;
    }
}

// Performance monitoring
let performanceMonitor = {
    frameTimeHistory: [],
    maxHistory: 60,

    update() {
        this.frameTimeHistory.push(millis());
        if (this.frameTimeHistory.length > this.maxHistory) {
            this.frameTimeHistory.shift();
        }
    },

    getAverageFrameTime() {
        if (this.frameTimeHistory.length < 2) return 0;

        let totalTime = this.frameTimeHistory[this.frameTimeHistory.length - 1] -
                       this.frameTimeHistory[0];
        return totalTime / (this.frameTimeHistory.length - 1);
    },

    display() {
        fill(255);
        text(`Avg Frame Time: ${this.getAverageFrameTime().toFixed(2)}ms`, 10, 40);
    }
};
```

## Common Pitfalls

### Issue 1: Performance Problems with Large Numbers of Objects
Drawing too many objects can cause frame rate drops.

**Solution:**
```javascript
// Use object pooling
class ObjectPool {
    constructor(createFn, resetFn, initialSize = 10) {
        this.createFn = createFn;
        this.resetFn = resetFn;
        this.pool = [];
        this.active = [];

        // Pre-populate pool
        for (let i = 0; i < initialSize; i++) {
            this.pool.push(this.createFn());
        }
    }

    get() {
        let obj = this.pool.pop() || this.createFn();
        this.active.push(obj);
        return obj;
    }

    release(obj) {
        let index = this.active.indexOf(obj);
        if (index !== -1) {
            this.active.splice(index, 1);
            this.resetFn(obj);
            this.pool.push(obj);
        }
    }
}
```

### Issue 2: Memory Leaks with Event Listeners
Not properly removing event listeners can cause memory leaks.

**Solution:**
```javascript
// Proper cleanup
function setup() {
    createCanvas(800, 600);
    window.addEventListener('resize', handleResize);
}

function handleResize() {
    resizeCanvas(windowWidth, windowHeight);
}

// Clean up when needed
function cleanup() {
    window.removeEventListener('resize', handleResize);
}
```

### Issue 3: Coordinate System Confusion
Forgetting about p5.js coordinate system (0,0 at top-left).

**Solution:**
```javascript
// Use helper functions for coordinate conversion
function screenToWorld(screenX, screenY) {
    return {
        x: screenX - width/2,
        y: height/2 - screenY  // Flip Y axis
    };
}

function worldToScreen(worldX, worldY) {
    return {
        x: worldX + width/2,
        y: height/2 - worldY  // Flip Y axis
    };
}
```

### Issue 4: Blocking Operations in draw()
Performing expensive operations in draw() can freeze the sketch.

**Solution:**
```javascript
// Use async operations and callbacks
let dataLoaded = false;
let data = null;

function setup() {
    createCanvas(800, 600);
    loadDataAsync();
}

function loadDataAsync() {
    // Simulate async data loading
    setTimeout(() => {
        data = generateLargeDataset();
        dataLoaded = true;
    }, 100);
}

function draw() {
    background(240);

    if (dataLoaded) {
        displayData(data);
    } else {
        displayLoadingScreen();
    }
}
```

## References

{% embed url="https://p5js.org/" %}

{% embed url="https://p5js.org/reference/" %}

{% embed url="https://github.com/processing/p5.js" %}

{% embed url="https://p5js.org/examples/" %}
```
```
