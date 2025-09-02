---
description: >-
  Three.js is a powerful JavaScript library for creating 3D graphics in the browser 
  using WebGL, making 3D development accessible with an intuitive API.
---

# Three.js

## Introduction

Three.js is a cross-browser JavaScript library and API used to create and display animated 3D computer graphics in a web browser using WebGL. It provides a high-level abstraction over WebGL, making 3D graphics development more accessible while still offering the power and flexibility needed for complex 3D applications.

## Use Cases

1. **3D Visualizations**: Create interactive data visualizations and scientific models
2. **Product Showcases**: Build 3D product configurators and virtual showrooms
3. **Games**: Develop browser-based 3D games and interactive experiences
4. **Architectural Visualization**: Create virtual tours and building walkthroughs
5. **Educational Content**: Build interactive 3D learning experiences
6. **Art and Creative Projects**: Develop generative art and creative installations

## Installation and Setup

### CDN (Quick Start)

```html
<!DOCTYPE html>
<html>
<head>
    <title>Three.js Scene</title>
    <style>
        body { margin: 0; overflow: hidden; }
        canvas { display: block; }
    </style>
</head>
<body>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r155/three.min.js"></script>
    <script>
        // Scene setup
        const scene = new THREE.Scene();
        const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
        const renderer = new THREE.WebGLRenderer();
        
        renderer.setSize(window.innerWidth, window.innerHeight);
        document.body.appendChild(renderer.domElement);
        
        // Create a cube
        const geometry = new THREE.BoxGeometry();
        const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
        const cube = new THREE.Mesh(geometry, material);
        scene.add(cube);
        
        camera.position.z = 5;
        
        // Animation loop
        function animate() {
            requestAnimationFrame(animate);
            
            cube.rotation.x += 0.01;
            cube.rotation.y += 0.01;
            
            renderer.render(scene, camera);
        }
        
        animate();
    </script>
</body>
</html>
```

### NPM Installation

```bash
npm install three
```

```javascript
import * as THREE from 'three';

// Scene setup
const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer();

renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement);

// Create geometry and material
const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
const cube = new THREE.Mesh(geometry, material);
scene.add(cube);

camera.position.z = 5;

// Animation loop
function animate() {
    requestAnimationFrame(animate);
    
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;
    
    renderer.render(scene, camera);
}

animate();
```

## Core Concepts

### Scene, Camera, and Renderer

```javascript
// The fundamental Three.js setup
class ThreeJSApp {
    constructor() {
        this.scene = null;
        this.camera = null;
        this.renderer = null;
        this.init();
    }

    init() {
        // Create scene
        this.scene = new THREE.Scene();
        this.scene.background = new THREE.Color(0x222222);

        // Create camera
        this.camera = new THREE.PerspectiveCamera(
            75,                                    // Field of view
            window.innerWidth / window.innerHeight, // Aspect ratio
            0.1,                                   // Near clipping plane
            1000                                   // Far clipping plane
        );
        this.camera.position.set(0, 0, 5);

        // Create renderer
        this.renderer = new THREE.WebGLRenderer({ 
            antialias: true,
            alpha: true 
        });
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        this.renderer.setPixelRatio(window.devicePixelRatio);
        this.renderer.shadowMap.enabled = true;
        this.renderer.shadowMap.type = THREE.PCFSoftShadowMap;

        document.body.appendChild(this.renderer.domElement);

        // Handle window resize
        window.addEventListener('resize', () => this.onWindowResize());
    }

    onWindowResize() {
        this.camera.aspect = window.innerWidth / window.innerHeight;
        this.camera.updateProjectionMatrix();
        this.renderer.setSize(window.innerWidth, window.innerHeight);
    }

    render() {
        this.renderer.render(this.scene, this.camera);
    }
}

// Usage
const app = new ThreeJSApp();
```

### Geometries and Meshes

```javascript
// Basic geometries
function createBasicShapes(scene) {
    // Box geometry
    const boxGeometry = new THREE.BoxGeometry(1, 1, 1);
    const boxMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
    const box = new THREE.Mesh(boxGeometry, boxMaterial);
    box.position.set(-3, 0, 0);
    scene.add(box);

    // Sphere geometry
    const sphereGeometry = new THREE.SphereGeometry(0.5, 32, 32);
    const sphereMaterial = new THREE.MeshBasicMaterial({ color: 0x00ff00 });
    const sphere = new THREE.Mesh(sphereGeometry, sphereMaterial);
    sphere.position.set(-1, 0, 0);
    scene.add(sphere);

    // Cylinder geometry
    const cylinderGeometry = new THREE.CylinderGeometry(0.5, 0.5, 1, 32);
    const cylinderMaterial = new THREE.MeshBasicMaterial({ color: 0x0000ff });
    const cylinder = new THREE.Mesh(cylinderGeometry, cylinderMaterial);
    cylinder.position.set(1, 0, 0);
    scene.add(cylinder);

    // Plane geometry
    const planeGeometry = new THREE.PlaneGeometry(2, 2);
    const planeMaterial = new THREE.MeshBasicMaterial({ 
        color: 0xffff00, 
        side: THREE.DoubleSide 
    });
    const plane = new THREE.Mesh(planeGeometry, planeMaterial);
    plane.position.set(3, 0, 0);
    plane.rotation.y = Math.PI / 4;
    scene.add(plane);

    return [box, sphere, cylinder, plane];
}
```

### Materials and Textures

```javascript
// Different material types
function createMaterialExamples(scene) {
    const geometry = new THREE.SphereGeometry(0.5, 32, 32);

    // Basic Material (unaffected by lights)
    const basicMaterial = new THREE.MeshBasicMaterial({ color: 0xff0000 });
    const basicSphere = new THREE.Mesh(geometry, basicMaterial);
    basicSphere.position.set(-4, 1, 0);
    scene.add(basicSphere);

    // Lambert Material (diffuse lighting)
    const lambertMaterial = new THREE.MeshLambertMaterial({ color: 0x00ff00 });
    const lambertSphere = new THREE.Mesh(geometry, lambertMaterial);
    lambertSphere.position.set(-2, 1, 0);
    scene.add(lambertSphere);

    // Phong Material (specular highlights)
    const phongMaterial = new THREE.MeshPhongMaterial({ 
        color: 0x0000ff,
        shininess: 100,
        specular: 0x222222
    });
    const phongSphere = new THREE.Mesh(geometry, phongMaterial);
    phongSphere.position.set(0, 1, 0);
    scene.add(phongSphere);

    // Standard Material (physically based)
    const standardMaterial = new THREE.MeshStandardMaterial({
        color: 0xff00ff,
        metalness: 0.5,
        roughness: 0.1
    });
    const standardSphere = new THREE.Mesh(geometry, standardMaterial);
    standardSphere.position.set(2, 1, 0);
    scene.add(standardSphere);

    // Physical Material (advanced PBR)
    const physicalMaterial = new THREE.MeshPhysicalMaterial({
        color: 0x00ffff,
        metalness: 0.0,
        roughness: 0.0,
        transmission: 0.9,
        thickness: 0.5
    });
    const physicalSphere = new THREE.Mesh(geometry, physicalMaterial);
    physicalSphere.position.set(4, 1, 0);
    scene.add(physicalSphere);

    return [basicSphere, lambertSphere, phongSphere, standardSphere, physicalSphere];
}

// Working with textures
function createTexturedObjects(scene) {
    const loader = new THREE.TextureLoader();
    
    // Load texture
    const texture = loader.load('https://threejs.org/examples/textures/crate.gif');
    
    // Create textured cube
    const geometry = new THREE.BoxGeometry(1, 1, 1);
    const material = new THREE.MeshBasicMaterial({ map: texture });
    const cube = new THREE.Mesh(geometry, material);
    scene.add(cube);

    // Multiple textures on one material
    const brickTexture = loader.load('path/to/brick-diffuse.jpg');
    const brickNormal = loader.load('path/to/brick-normal.jpg');
    const brickRoughness = loader.load('path/to/brick-roughness.jpg');

    const advancedMaterial = new THREE.MeshStandardMaterial({
        map: brickTexture,           // Diffuse map
        normalMap: brickNormal,      // Normal map for surface detail
        roughnessMap: brickRoughness, // Roughness map
        roughness: 0.8,
        metalness: 0.1
    });

    const texturedSphere = new THREE.Mesh(
        new THREE.SphereGeometry(0.5, 32, 32),
        advancedMaterial
    );
    texturedSphere.position.set(2, 0, 0);
    scene.add(texturedSphere);

    return [cube, texturedSphere];
}
```

## Lighting

### Basic Lighting Setup

```javascript
function setupLighting(scene) {
    // Ambient light (global illumination)
    const ambientLight = new THREE.AmbientLight(0x404040, 0.3);
    scene.add(ambientLight);

    // Directional light (like sunlight)
    const directionalLight = new THREE.DirectionalLight(0xffffff, 0.8);
    directionalLight.position.set(5, 5, 5);
    directionalLight.castShadow = true;
    
    // Configure shadow properties
    directionalLight.shadow.mapSize.width = 2048;
    directionalLight.shadow.mapSize.height = 2048;
    directionalLight.shadow.camera.near = 0.5;
    directionalLight.shadow.camera.far = 50;
    directionalLight.shadow.camera.left = -10;
    directionalLight.shadow.camera.right = 10;
    directionalLight.shadow.camera.top = 10;
    directionalLight.shadow.camera.bottom = -10;
    
    scene.add(directionalLight);

    // Point light (like a light bulb)
    const pointLight = new THREE.PointLight(0xff0000, 1, 100);
    pointLight.position.set(2, 2, 2);
    pointLight.castShadow = true;
    scene.add(pointLight);

    // Spot light (like a flashlight)
    const spotLight = new THREE.SpotLight(0x00ff00, 1, 100, Math.PI / 6, 0.5);
    spotLight.position.set(-2, 3, 2);
    spotLight.target.position.set(-2, 0, 0);
    spotLight.castShadow = true;
    scene.add(spotLight);
    scene.add(spotLight.target);

    // Hemisphere light (sky and ground lighting)
    const hemisphereLight = new THREE.HemisphereLight(0x87CEEB, 0x8B4513, 0.3);
    scene.add(hemisphereLight);

    return {
        ambient: ambientLight,
        directional: directionalLight,
        point: pointLight,
        spot: spotLight,
        hemisphere: hemisphereLight
    };
}
```

### Dynamic Lighting Effects

```javascript
class LightingController {
    constructor(scene) {
        this.scene = scene;
        this.lights = this.setupLights();
        this.time = 0;
    }

    setupLights() {
        // Moving point lights
        const light1 = new THREE.PointLight(0xff0000, 1, 10);
        const light2 = new THREE.PointLight(0x00ff00, 1, 10);
        const light3 = new THREE.PointLight(0x0000ff, 1, 10);

        this.scene.add(light1);
        this.scene.add(light2);
        this.scene.add(light3);

        // Light helpers for visualization
        const helper1 = new THREE.PointLightHelper(light1, 0.1);
        const helper2 = new THREE.PointLightHelper(light2, 0.1);
        const helper3 = new THREE.PointLightHelper(light3, 0.1);

        this.scene.add(helper1);
        this.scene.add(helper2);
        this.scene.add(helper3);

        return { light1, light2, light3, helper1, helper2, helper3 };
    }

    update() {
        this.time += 0.01;

        // Animate light positions
        this.lights.light1.position.x = Math.cos(this.time) * 3;
        this.lights.light1.position.z = Math.sin(this.time) * 3;
        this.lights.light1.position.y = 2;

        this.lights.light2.position.x = Math.cos(this.time + Math.PI * 2/3) * 3;
        this.lights.light2.position.z = Math.sin(this.time + Math.PI * 2/3) * 3;
        this.lights.light2.position.y = 2;

        this.lights.light3.position.x = Math.cos(this.time + Math.PI * 4/3) * 3;
        this.lights.light3.position.z = Math.sin(this.time + Math.PI * 4/3) * 3;
        this.lights.light3.position.y = 2;

        // Animate light intensity
        this.lights.light1.intensity = 0.5 + 0.5 * Math.sin(this.time * 2);
        this.lights.light2.intensity = 0.5 + 0.5 * Math.sin(this.time * 2 + Math.PI / 2);
        this.lights.light3.intensity = 0.5 + 0.5 * Math.sin(this.time * 2 + Math.PI);

        // Update helpers
        this.lights.helper1.update();
        this.lights.helper2.update();
        this.lights.helper3.update();
    }
}

## Animation and Movement

### Basic Animation with requestAnimationFrame

```javascript
class AnimationManager {
    constructor(scene, camera, renderer) {
        this.scene = scene;
        this.camera = camera;
        this.renderer = renderer;
        this.clock = new THREE.Clock();
        this.objects = [];
        this.isRunning = false;
    }

    addObject(object, animationFunction) {
        this.objects.push({
            mesh: object,
            animate: animationFunction
        });
    }

    start() {
        this.isRunning = true;
        this.animate();
    }

    stop() {
        this.isRunning = false;
    }

    animate() {
        if (!this.isRunning) return;

        requestAnimationFrame(() => this.animate());

        const deltaTime = this.clock.getDelta();
        const elapsedTime = this.clock.getElapsedTime();

        // Update all animated objects
        this.objects.forEach(obj => {
            if (obj.animate) {
                obj.animate(obj.mesh, deltaTime, elapsedTime);
            }
        });

        this.renderer.render(this.scene, this.camera);
    }
}

// Animation functions
const AnimationFunctions = {
    rotate: (mesh, deltaTime, elapsedTime) => {
        mesh.rotation.x += deltaTime;
        mesh.rotation.y += deltaTime * 0.5;
    },

    bounce: (mesh, deltaTime, elapsedTime) => {
        mesh.position.y = Math.abs(Math.sin(elapsedTime * 2)) * 2;
    },

    orbit: (mesh, deltaTime, elapsedTime) => {
        const radius = 3;
        mesh.position.x = Math.cos(elapsedTime) * radius;
        mesh.position.z = Math.sin(elapsedTime) * radius;
    },

    scale: (mesh, deltaTime, elapsedTime) => {
        const scale = 1 + Math.sin(elapsedTime * 3) * 0.3;
        mesh.scale.set(scale, scale, scale);
    },

    wave: (mesh, deltaTime, elapsedTime) => {
        mesh.position.y = Math.sin(elapsedTime * 2 + mesh.position.x) * 0.5;
    }
};

// Usage example
const animationManager = new AnimationManager(scene, camera, renderer);

// Create objects with different animations
const cube = new THREE.Mesh(
    new THREE.BoxGeometry(),
    new THREE.MeshStandardMaterial({ color: 0xff0000 })
);
cube.position.set(-2, 0, 0);
scene.add(cube);
animationManager.addObject(cube, AnimationFunctions.rotate);

const sphere = new THREE.Mesh(
    new THREE.SphereGeometry(0.5),
    new THREE.MeshStandardMaterial({ color: 0x00ff00 })
);
sphere.position.set(0, 0, 0);
scene.add(sphere);
animationManager.addObject(sphere, AnimationFunctions.bounce);

const cylinder = new THREE.Mesh(
    new THREE.CylinderGeometry(0.3, 0.3, 1),
    new THREE.MeshStandardMaterial({ color: 0x0000ff })
);
scene.add(cylinder);
animationManager.addObject(cylinder, AnimationFunctions.orbit);

animationManager.start();
```

### Tween Animations with GSAP

```javascript
// Using GSAP for smooth animations (requires GSAP library)
class TweenAnimations {
    static animateIn(object, duration = 1) {
        // Scale in animation
        object.scale.set(0, 0, 0);
        gsap.to(object.scale, {
            x: 1, y: 1, z: 1,
            duration: duration,
            ease: "back.out(1.7)"
        });

        // Fade in animation
        object.material.transparent = true;
        object.material.opacity = 0;
        gsap.to(object.material, {
            opacity: 1,
            duration: duration
        });
    }

    static animateOut(object, duration = 0.5) {
        return new Promise((resolve) => {
            gsap.to(object.scale, {
                x: 0, y: 0, z: 0,
                duration: duration,
                ease: "back.in(1.7)"
            });

            gsap.to(object.material, {
                opacity: 0,
                duration: duration,
                onComplete: resolve
            });
        });
    }

    static floatAnimation(object) {
        gsap.to(object.position, {
            y: "+=0.5",
            duration: 2,
            yoyo: true,
            repeat: -1,
            ease: "sine.inOut"
        });
    }

    static rotateAnimation(object) {
        gsap.to(object.rotation, {
            y: Math.PI * 2,
            duration: 4,
            repeat: -1,
            ease: "none"
        });
    }

    static morphGeometry(mesh, targetGeometry, duration = 2) {
        // Animate between geometries (requires compatible vertex counts)
        const startPositions = mesh.geometry.attributes.position.array;
        const endPositions = targetGeometry.attributes.position.array;

        gsap.to(startPositions, {
            duration: duration,
            ease: "power2.inOut",
            onUpdate: function() {
                for (let i = 0; i < startPositions.length; i++) {
                    startPositions[i] = THREE.MathUtils.lerp(
                        startPositions[i],
                        endPositions[i],
                        this.progress()
                    );
                }
                mesh.geometry.attributes.position.needsUpdate = true;
            }
        });
    }
}
```

## Camera Controls and Interaction

### Orbit Controls

```javascript
// Requires OrbitControls from three/examples/jsm/controls/OrbitControls.js
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';

class CameraController {
    constructor(camera, renderer) {
        this.camera = camera;
        this.renderer = renderer;
        this.controls = null;
        this.setupOrbitControls();
    }

    setupOrbitControls() {
        this.controls = new OrbitControls(this.camera, this.renderer.domElement);

        // Configure controls
        this.controls.enableDamping = true;
        this.controls.dampingFactor = 0.05;
        this.controls.screenSpacePanning = false;

        // Set limits
        this.controls.minDistance = 1;
        this.controls.maxDistance = 100;
        this.controls.maxPolarAngle = Math.PI / 2; // Prevent going below ground

        // Set target (what the camera looks at)
        this.controls.target.set(0, 0, 0);
    }

    update() {
        if (this.controls) {
            this.controls.update();
        }
    }

    // Animate camera to a specific position
    animateTo(position, target, duration = 2) {
        const startPosition = this.camera.position.clone();
        const startTarget = this.controls.target.clone();

        gsap.to({ t: 0 }, {
            t: 1,
            duration: duration,
            ease: "power2.inOut",
            onUpdate: function() {
                // Interpolate camera position
                this.camera.position.lerpVectors(startPosition, position, this.t);

                // Interpolate target
                this.controls.target.lerpVectors(startTarget, target, this.t);

                this.controls.update();
            }.bind(this)
        });
    }

    // Preset camera positions
    presets = {
        front: { position: new THREE.Vector3(0, 0, 5), target: new THREE.Vector3(0, 0, 0) },
        back: { position: new THREE.Vector3(0, 0, -5), target: new THREE.Vector3(0, 0, 0) },
        top: { position: new THREE.Vector3(0, 5, 0), target: new THREE.Vector3(0, 0, 0) },
        side: { position: new THREE.Vector3(5, 0, 0), target: new THREE.Vector3(0, 0, 0) },
        isometric: { position: new THREE.Vector3(3, 3, 3), target: new THREE.Vector3(0, 0, 0) }
    };

    goToPreset(presetName, duration = 2) {
        const preset = this.presets[presetName];
        if (preset) {
            this.animateTo(preset.position, preset.target, duration);
        }
    }
}
```

### Mouse and Touch Interaction

```javascript
class InteractionManager {
    constructor(camera, scene, renderer) {
        this.camera = camera;
        this.scene = scene;
        this.renderer = renderer;
        this.raycaster = new THREE.Raycaster();
        this.mouse = new THREE.Vector2();
        this.selectedObject = null;
        this.hoveredObject = null;

        this.setupEventListeners();
    }

    setupEventListeners() {
        const canvas = this.renderer.domElement;

        // Mouse events
        canvas.addEventListener('mousemove', (event) => this.onMouseMove(event));
        canvas.addEventListener('click', (event) => this.onClick(event));
        canvas.addEventListener('dblclick', (event) => this.onDoubleClick(event));

        // Touch events
        canvas.addEventListener('touchstart', (event) => this.onTouchStart(event));
        canvas.addEventListener('touchmove', (event) => this.onTouchMove(event));
        canvas.addEventListener('touchend', (event) => this.onTouchEnd(event));
    }

    updateMousePosition(event) {
        const rect = this.renderer.domElement.getBoundingClientRect();
        this.mouse.x = ((event.clientX - rect.left) / rect.width) * 2 - 1;
        this.mouse.y = -((event.clientY - rect.top) / rect.height) * 2 + 1;
    }

    getIntersectedObjects() {
        this.raycaster.setFromCamera(this.mouse, this.camera);
        return this.raycaster.intersectObjects(this.scene.children, true);
    }

    onMouseMove(event) {
        this.updateMousePosition(event);
        const intersects = this.getIntersectedObjects();

        // Handle hover effects
        if (intersects.length > 0) {
            const object = intersects[0].object;

            if (this.hoveredObject !== object) {
                // Remove hover from previous object
                if (this.hoveredObject) {
                    this.onObjectHoverOut(this.hoveredObject);
                }

                // Add hover to new object
                this.hoveredObject = object;
                this.onObjectHoverIn(object);
            }
        } else {
            if (this.hoveredObject) {
                this.onObjectHoverOut(this.hoveredObject);
                this.hoveredObject = null;
            }
        }
    }

    onClick(event) {
        this.updateMousePosition(event);
        const intersects = this.getIntersectedObjects();

        if (intersects.length > 0) {
            const object = intersects[0].object;
            this.onObjectClick(object, intersects[0]);
        } else {
            this.onBackgroundClick();
        }
    }

    onDoubleClick(event) {
        this.updateMousePosition(event);
        const intersects = this.getIntersectedObjects();

        if (intersects.length > 0) {
            const object = intersects[0].object;
            this.onObjectDoubleClick(object, intersects[0]);
        }
    }

    // Event handlers (override these in subclasses)
    onObjectHoverIn(object) {
        document.body.style.cursor = 'pointer';

        // Example: Change material color on hover
        if (object.material) {
            object.userData.originalColor = object.material.color.getHex();
            object.material.color.setHex(0xffff00);
        }
    }

    onObjectHoverOut(object) {
        document.body.style.cursor = 'default';

        // Restore original color
        if (object.material && object.userData.originalColor !== undefined) {
            object.material.color.setHex(object.userData.originalColor);
        }
    }

    onObjectClick(object, intersection) {
        console.log('Clicked object:', object);
        console.log('Intersection point:', intersection.point);

        // Example: Select/deselect object
        if (this.selectedObject === object) {
            this.deselectObject();
        } else {
            this.selectObject(object);
        }
    }

    onObjectDoubleClick(object, intersection) {
        console.log('Double-clicked object:', object);

        // Example: Focus camera on object
        const box = new THREE.Box3().setFromObject(object);
        const center = box.getCenter(new THREE.Vector3());
        const size = box.getSize(new THREE.Vector3());

        const maxDim = Math.max(size.x, size.y, size.z);
        const fov = this.camera.fov * (Math.PI / 180);
        const cameraDistance = maxDim / (2 * Math.tan(fov / 2)) * 1.5;

        const direction = this.camera.position.clone().sub(center).normalize();
        const newPosition = center.clone().add(direction.multiplyScalar(cameraDistance));

        // Animate camera to new position
        gsap.to(this.camera.position, {
            x: newPosition.x,
            y: newPosition.y,
            z: newPosition.z,
            duration: 1,
            ease: "power2.inOut"
        });
    }

    onBackgroundClick() {
        this.deselectObject();
    }

    selectObject(object) {
        this.deselectObject(); // Deselect previous

        this.selectedObject = object;

        // Visual feedback for selection
        if (object.material) {
            object.userData.originalEmissive = object.material.emissive.getHex();
            object.material.emissive.setHex(0x444444);
        }
    }

    deselectObject() {
        if (this.selectedObject) {
            // Restore original emissive
            if (this.selectedObject.material &&
                this.selectedObject.userData.originalEmissive !== undefined) {
                this.selectedObject.material.emissive.setHex(
                    this.selectedObject.userData.originalEmissive
                );
            }

            this.selectedObject = null;
        }
    }

    // Touch event handlers
    onTouchStart(event) {
        event.preventDefault();
        if (event.touches.length === 1) {
            const touch = event.touches[0];
            this.updateMousePosition(touch);
        }
    }

    onTouchMove(event) {
        event.preventDefault();
        if (event.touches.length === 1) {
            const touch = event.touches[0];
            this.updateMousePosition(touch);
            this.onMouseMove(touch);
        }
    }

    onTouchEnd(event) {
        event.preventDefault();
        if (event.changedTouches.length === 1) {
            const touch = event.changedTouches[0];
            this.updateMousePosition(touch);
            this.onClick(touch);
        }
    }
}

## Loading 3D Models and Assets

### GLTF Model Loading

```javascript
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js';
import { DRACOLoader } from 'three/examples/jsm/loaders/DRACOLoader.js';

class ModelLoader {
    constructor() {
        this.gltfLoader = new GLTFLoader();
        this.dracoLoader = new DRACOLoader();

        // Setup Draco decoder for compressed models
        this.dracoLoader.setDecoderPath('https://www.gstatic.com/draco/versioned/decoders/1.5.6/');
        this.gltfLoader.setDRACOLoader(this.dracoLoader);

        this.loadingManager = new THREE.LoadingManager();
        this.setupLoadingManager();
    }

    setupLoadingManager() {
        this.loadingManager.onStart = (url, itemsLoaded, itemsTotal) => {
            console.log(`Started loading: ${url} (${itemsLoaded}/${itemsTotal})`);
        };

        this.loadingManager.onProgress = (url, itemsLoaded, itemsTotal) => {
            const progress = (itemsLoaded / itemsTotal) * 100;
            console.log(`Loading progress: ${progress.toFixed(2)}%`);
            this.updateProgressBar(progress);
        };

        this.loadingManager.onLoad = () => {
            console.log('All assets loaded');
            this.hideProgressBar();
        };

        this.loadingManager.onError = (url) => {
            console.error(`Error loading: ${url}`);
        };
    }

    async loadModel(url, options = {}) {
        return new Promise((resolve, reject) => {
            this.gltfLoader.load(
                url,
                (gltf) => {
                    const model = gltf.scene;

                    // Apply options
                    if (options.scale) {
                        model.scale.setScalar(options.scale);
                    }

                    if (options.position) {
                        model.position.copy(options.position);
                    }

                    if (options.rotation) {
                        model.rotation.copy(options.rotation);
                    }

                    // Enable shadows
                    if (options.castShadow || options.receiveShadow) {
                        model.traverse((child) => {
                            if (child.isMesh) {
                                child.castShadow = options.castShadow || false;
                                child.receiveShadow = options.receiveShadow || false;
                            }
                        });
                    }

                    // Store animations
                    if (gltf.animations && gltf.animations.length > 0) {
                        model.userData.animations = gltf.animations;
                        model.userData.mixer = new THREE.AnimationMixer(model);

                        // Auto-play first animation if requested
                        if (options.autoPlay) {
                            const action = model.userData.mixer.clipAction(gltf.animations[0]);
                            action.play();
                        }
                    }

                    resolve({ model, gltf });
                },
                (progress) => {
                    const percentComplete = (progress.loaded / progress.total) * 100;
                    console.log(`Model loading: ${percentComplete.toFixed(2)}%`);
                },
                (error) => {
                    console.error('Error loading model:', error);
                    reject(error);
                }
            );
        });
    }

    updateProgressBar(progress) {
        const progressBar = document.getElementById('progress-bar');
        if (progressBar) {
            progressBar.style.width = `${progress}%`;
        }
    }

    hideProgressBar() {
        const progressContainer = document.getElementById('progress-container');
        if (progressContainer) {
            progressContainer.style.display = 'none';
        }
    }
}

// Usage example
const modelLoader = new ModelLoader();

async function loadAndDisplayModel() {
    try {
        const { model, gltf } = await modelLoader.loadModel('path/to/model.gltf', {
            scale: 2,
            position: new THREE.Vector3(0, 0, 0),
            castShadow: true,
            receiveShadow: true,
            autoPlay: true
        });

        scene.add(model);

        // Handle animations
        if (model.userData.mixer) {
            // Add to animation loop
            animationMixers.push(model.userData.mixer);
        }

    } catch (error) {
        console.error('Failed to load model:', error);
    }
}
```

### Texture and Environment Loading

```javascript
class AssetLoader {
    constructor() {
        this.textureLoader = new THREE.TextureLoader();
        this.cubeTextureLoader = new THREE.CubeTextureLoader();
        this.rgbeLoader = new THREE.RGBELoader();
        this.exrLoader = new THREE.EXRLoader();
    }

    loadTexture(url, options = {}) {
        return new Promise((resolve, reject) => {
            this.textureLoader.load(
                url,
                (texture) => {
                    // Apply texture settings
                    if (options.wrapS) texture.wrapS = options.wrapS;
                    if (options.wrapT) texture.wrapT = options.wrapT;
                    if (options.repeat) texture.repeat.copy(options.repeat);
                    if (options.flipY !== undefined) texture.flipY = options.flipY;

                    resolve(texture);
                },
                undefined,
                reject
            );
        });
    }

    loadCubeTexture(urls) {
        return new Promise((resolve, reject) => {
            this.cubeTextureLoader.load(urls, resolve, undefined, reject);
        });
    }

    loadHDREnvironment(url) {
        return new Promise((resolve, reject) => {
            this.rgbeLoader.load(url, (texture) => {
                texture.mapping = THREE.EquirectangularReflectionMapping;
                resolve(texture);
            }, undefined, reject);
        });
    }

    async setupEnvironment(scene, environmentUrl) {
        try {
            const envTexture = await this.loadHDREnvironment(environmentUrl);

            // Set as scene background
            scene.background = envTexture;

            // Set as environment map for all materials
            scene.environment = envTexture;

            return envTexture;
        } catch (error) {
            console.error('Failed to load environment:', error);

            // Fallback to simple gradient background
            const loader = new THREE.CubeTextureLoader();
            const texture = loader.load([
                'px.jpg', 'nx.jpg',
                'py.jpg', 'ny.jpg',
                'pz.jpg', 'nz.jpg'
            ]);
            scene.background = texture;

            return texture;
        }
    }
}
```

## Post-Processing Effects

### Basic Post-Processing Setup

```javascript
import { EffectComposer } from 'three/examples/jsm/postprocessing/EffectComposer.js';
import { RenderPass } from 'three/examples/jsm/postprocessing/RenderPass.js';
import { UnrealBloomPass } from 'three/examples/jsm/postprocessing/UnrealBloomPass.js';
import { FilmPass } from 'three/examples/jsm/postprocessing/FilmPass.js';
import { GlitchPass } from 'three/examples/jsm/postprocessing/GlitchPass.js';
import { OutlinePass } from 'three/examples/jsm/postprocessing/OutlinePass.js';

class PostProcessingManager {
    constructor(renderer, scene, camera) {
        this.renderer = renderer;
        this.scene = scene;
        this.camera = camera;

        this.composer = new EffectComposer(renderer);
        this.passes = {};

        this.setupBasicPasses();
    }

    setupBasicPasses() {
        // Render pass (always first)
        this.passes.render = new RenderPass(this.scene, this.camera);
        this.composer.addPass(this.passes.render);

        // Bloom effect
        this.passes.bloom = new UnrealBloomPass(
            new THREE.Vector2(window.innerWidth, window.innerHeight),
            1.5,  // strength
            0.4,  // radius
            0.85  // threshold
        );
        this.passes.bloom.enabled = false;
        this.composer.addPass(this.passes.bloom);

        // Film grain effect
        this.passes.film = new FilmPass(
            0.35,   // noise intensity
            0.025,  // scanline intensity
            648,    // scanline count
            false   // grayscale
        );
        this.passes.film.enabled = false;
        this.composer.addPass(this.passes.film);

        // Glitch effect
        this.passes.glitch = new GlitchPass();
        this.passes.glitch.enabled = false;
        this.composer.addPass(this.passes.glitch);

        // Outline effect
        this.passes.outline = new OutlinePass(
            new THREE.Vector2(window.innerWidth, window.innerHeight),
            this.scene,
            this.camera
        );
        this.passes.outline.edgeStrength = 3.0;
        this.passes.outline.edgeGlow = 0.0;
        this.passes.outline.edgeThickness = 1.0;
        this.passes.outline.pulsePeriod = 0;
        this.passes.outline.visibleEdgeColor.set('#ffffff');
        this.passes.outline.hiddenEdgeColor.set('#190a05');
        this.passes.outline.enabled = false;
        this.composer.addPass(this.passes.outline);
    }

    enableEffect(effectName, options = {}) {
        const pass = this.passes[effectName];
        if (pass) {
            pass.enabled = true;

            // Apply options
            Object.keys(options).forEach(key => {
                if (pass[key] !== undefined) {
                    pass[key] = options[key];
                }
            });
        }
    }

    disableEffect(effectName) {
        const pass = this.passes[effectName];
        if (pass) {
            pass.enabled = false;
        }
    }

    setOutlineObjects(objects) {
        if (this.passes.outline) {
            this.passes.outline.selectedObjects = objects;
        }
    }

    render() {
        this.composer.render();
    }

    onWindowResize() {
        this.composer.setSize(window.innerWidth, window.innerHeight);

        // Update passes that need size updates
        if (this.passes.bloom) {
            this.passes.bloom.setSize(window.innerWidth, window.innerHeight);
        }
        if (this.passes.outline) {
            this.passes.outline.setSize(window.innerWidth, window.innerHeight);
        }
    }
}

// Usage
const postProcessing = new PostProcessingManager(renderer, scene, camera);

// Enable bloom effect
postProcessing.enableEffect('bloom', {
    strength: 2.0,
    radius: 0.5,
    threshold: 0.1
});

// Enable film grain
postProcessing.enableEffect('film', {
    nIntensity: 0.5,
    sIntensity: 0.05
});

// In render loop, use composer instead of renderer
function animate() {
    requestAnimationFrame(animate);

    // Update scene...

    // Render with post-processing
    postProcessing.render();
}
```

## Performance Optimization

### Level of Detail (LOD)

```javascript
class LODManager {
    constructor(camera) {
        this.camera = camera;
        this.lodObjects = [];
    }

    createLODObject(geometries, materials, distances) {
        const lod = new THREE.LOD();

        for (let i = 0; i < geometries.length; i++) {
            const mesh = new THREE.Mesh(geometries[i], materials[i] || materials[0]);
            lod.addLevel(mesh, distances[i] || i * 10);
        }

        this.lodObjects.push(lod);
        return lod;
    }

    update() {
        this.lodObjects.forEach(lod => {
            lod.update(this.camera);
        });
    }

    // Create LOD for a complex model
    createModelLOD(highDetailModel, position = new THREE.Vector3()) {
        // Create simplified versions
        const mediumGeometry = this.simplifyGeometry(highDetailModel.geometry, 0.5);
        const lowGeometry = this.simplifyGeometry(highDetailModel.geometry, 0.2);

        const geometries = [
            highDetailModel.geometry,
            mediumGeometry,
            lowGeometry
        ];

        const materials = [highDetailModel.material];
        const distances = [0, 50, 200]; // Switch distances

        const lod = this.createLODObject(geometries, materials, distances);
        lod.position.copy(position);

        return lod;
    }

    simplifyGeometry(geometry, ratio) {
        // Simple decimation (in practice, use a proper decimation library)
        const simplified = geometry.clone();

        // Reduce vertex count (simplified approach)
        const positions = simplified.attributes.position.array;
        const newPositions = [];

        for (let i = 0; i < positions.length; i += 3 * Math.ceil(1 / ratio)) {
            newPositions.push(positions[i], positions[i + 1], positions[i + 2]);
        }

        simplified.setAttribute('position', new THREE.Float32BufferAttribute(newPositions, 3));

        return simplified;
    }
}
```

### Instancing for Performance

```javascript
class InstancedObjectManager {
    constructor(geometry, material, count) {
        this.geometry = geometry;
        this.material = material;
        this.count = count;

        this.instancedMesh = new THREE.InstancedMesh(geometry, material, count);
        this.dummy = new THREE.Object3D();

        this.setupInstances();
    }

    setupInstances() {
        // Set up initial transforms for each instance
        for (let i = 0; i < this.count; i++) {
            // Random position
            this.dummy.position.set(
                (Math.random() - 0.5) * 100,
                (Math.random() - 0.5) * 100,
                (Math.random() - 0.5) * 100
            );

            // Random rotation
            this.dummy.rotation.set(
                Math.random() * Math.PI * 2,
                Math.random() * Math.PI * 2,
                Math.random() * Math.PI * 2
            );

            // Random scale
            const scale = 0.5 + Math.random() * 1.5;
            this.dummy.scale.set(scale, scale, scale);

            this.dummy.updateMatrix();
            this.instancedMesh.setMatrixAt(i, this.dummy.matrix);
        }

        this.instancedMesh.instanceMatrix.needsUpdate = true;
    }

    updateInstance(index, position, rotation, scale) {
        this.dummy.position.copy(position);
        this.dummy.rotation.copy(rotation);
        this.dummy.scale.copy(scale);

        this.dummy.updateMatrix();
        this.instancedMesh.setMatrixAt(index, this.dummy.matrix);
        this.instancedMesh.instanceMatrix.needsUpdate = true;
    }

    animateInstances(time) {
        for (let i = 0; i < this.count; i++) {
            // Get current matrix
            this.instancedMesh.getMatrixAt(i, this.dummy.matrix);
            this.dummy.matrix.decompose(this.dummy.position, this.dummy.quaternion, this.dummy.scale);

            // Apply animation
            this.dummy.rotation.y = time * 0.001 + i * 0.1;
            this.dummy.position.y += Math.sin(time * 0.002 + i) * 0.01;

            this.dummy.updateMatrix();
            this.instancedMesh.setMatrixAt(i, this.dummy.matrix);
        }

        this.instancedMesh.instanceMatrix.needsUpdate = true;
    }

    getMesh() {
        return this.instancedMesh;
    }
}

// Usage
const instanceManager = new InstancedObjectManager(
    new THREE.BoxGeometry(0.5, 0.5, 0.5),
    new THREE.MeshStandardMaterial({ color: 0x00ff00 }),
    1000
);

scene.add(instanceManager.getMesh());

// In animation loop
function animate() {
    instanceManager.animateInstances(Date.now());
    // ... rest of animation
}

## Advanced Techniques

### Custom Shaders

```javascript
// Custom vertex shader
const vertexShader = `
    uniform float time;
    varying vec2 vUv;
    varying vec3 vPosition;

    void main() {
        vUv = uv;
        vPosition = position;

        // Wave effect
        vec3 pos = position;
        pos.z += sin(pos.x * 10.0 + time) * 0.1;
        pos.z += sin(pos.y * 10.0 + time * 1.5) * 0.1;

        gl_Position = projectionMatrix * modelViewMatrix * vec4(pos, 1.0);
    }
`;

// Custom fragment shader
const fragmentShader = `
    uniform float time;
    uniform vec3 color;
    varying vec2 vUv;
    varying vec3 vPosition;

    void main() {
        // Animated color based on position and time
        vec3 finalColor = color;
        finalColor.r += sin(vPosition.x * 5.0 + time) * 0.3;
        finalColor.g += sin(vPosition.y * 5.0 + time * 1.2) * 0.3;
        finalColor.b += sin(vPosition.z * 5.0 + time * 0.8) * 0.3;

        // Add some noise
        float noise = fract(sin(dot(vUv, vec2(12.9898, 78.233))) * 43758.5453);
        finalColor += noise * 0.1;

        gl_FragColor = vec4(finalColor, 1.0);
    }
`;

// Create shader material
const shaderMaterial = new THREE.ShaderMaterial({
    uniforms: {
        time: { value: 0.0 },
        color: { value: new THREE.Color(0xff0000) }
    },
    vertexShader: vertexShader,
    fragmentShader: fragmentShader
});

// Create mesh with custom shader
const geometry = new THREE.PlaneGeometry(5, 5, 50, 50);
const mesh = new THREE.Mesh(geometry, shaderMaterial);
scene.add(mesh);

// Update shader uniforms in animation loop
function animate() {
    shaderMaterial.uniforms.time.value = Date.now() * 0.001;
    // ... rest of animation
}
```

### Procedural Generation

```javascript
class ProceduralGenerator {
    static generateTerrain(width, height, scale = 1, octaves = 4) {
        const geometry = new THREE.PlaneGeometry(width, height, width - 1, height - 1);
        const vertices = geometry.attributes.position.array;

        // Generate height map using noise
        for (let i = 0; i < vertices.length; i += 3) {
            const x = vertices[i];
            const y = vertices[i + 1];

            let height = 0;
            let amplitude = 1;
            let frequency = 0.01;

            // Multiple octaves of noise
            for (let octave = 0; octave < octaves; octave++) {
                height += this.noise(x * frequency, y * frequency) * amplitude;
                amplitude *= 0.5;
                frequency *= 2;
            }

            vertices[i + 2] = height * scale;
        }

        geometry.attributes.position.needsUpdate = true;
        geometry.computeVertexNormals();

        return geometry;
    }

    static generateCity(gridSize = 10, blockSize = 2) {
        const cityGroup = new THREE.Group();

        for (let x = 0; x < gridSize; x++) {
            for (let z = 0; z < gridSize; z++) {
                // Skip some blocks for streets
                if (Math.random() > 0.7) continue;

                const height = 1 + Math.random() * 5;
                const width = blockSize * (0.8 + Math.random() * 0.4);
                const depth = blockSize * (0.8 + Math.random() * 0.4);

                const geometry = new THREE.BoxGeometry(width, height, depth);
                const material = new THREE.MeshStandardMaterial({
                    color: new THREE.Color().setHSL(0.6, 0.3, 0.3 + Math.random() * 0.4)
                });

                const building = new THREE.Mesh(geometry, material);
                building.position.set(
                    (x - gridSize / 2) * blockSize,
                    height / 2,
                    (z - gridSize / 2) * blockSize
                );

                building.castShadow = true;
                building.receiveShadow = true;

                cityGroup.add(building);
            }
        }

        return cityGroup;
    }

    static generateForest(count = 100, area = 50) {
        const forestGroup = new THREE.Group();

        for (let i = 0; i < count; i++) {
            const tree = this.generateTree();
            tree.position.set(
                (Math.random() - 0.5) * area,
                0,
                (Math.random() - 0.5) * area
            );

            tree.scale.setScalar(0.5 + Math.random() * 1.5);
            tree.rotation.y = Math.random() * Math.PI * 2;

            forestGroup.add(tree);
        }

        return forestGroup;
    }

    static generateTree() {
        const treeGroup = new THREE.Group();

        // Trunk
        const trunkGeometry = new THREE.CylinderGeometry(0.1, 0.15, 2, 8);
        const trunkMaterial = new THREE.MeshStandardMaterial({ color: 0x8B4513 });
        const trunk = new THREE.Mesh(trunkGeometry, trunkMaterial);
        trunk.position.y = 1;
        trunk.castShadow = true;
        treeGroup.add(trunk);

        // Leaves
        const leavesGeometry = new THREE.SphereGeometry(1, 8, 6);
        const leavesMaterial = new THREE.MeshStandardMaterial({ color: 0x228B22 });
        const leaves = new THREE.Mesh(leavesGeometry, leavesMaterial);
        leaves.position.y = 2.5;
        leaves.scale.set(1, 0.8, 1);
        leaves.castShadow = true;
        treeGroup.add(leaves);

        return treeGroup;
    }

    // Simple noise function (use a proper noise library for better results)
    static noise(x, y) {
        return Math.sin(x * 12.9898 + y * 78.233) * 43758.5453 % 1;
    }
}

// Usage
const terrain = ProceduralGenerator.generateTerrain(50, 50, 3, 6);
const terrainMesh = new THREE.Mesh(terrain, new THREE.MeshStandardMaterial({
    color: 0x90EE90,
    wireframe: false
}));
terrainMesh.rotation.x = -Math.PI / 2;
terrainMesh.receiveShadow = true;
scene.add(terrainMesh);

const city = ProceduralGenerator.generateCity(15, 3);
city.position.y = 3;
scene.add(city);

const forest = ProceduralGenerator.generateForest(50, 30);
scene.add(forest);
```

## Best Practices

### Code Organization

```javascript
// Organize your Three.js application into classes
class ThreeJSApplication {
    constructor() {
        this.scene = null;
        this.camera = null;
        this.renderer = null;
        this.controls = null;
        this.clock = new THREE.Clock();

        this.objects = [];
        this.lights = [];
        this.animations = [];

        this.init();
    }

    init() {
        this.createScene();
        this.createCamera();
        this.createRenderer();
        this.createControls();
        this.createLights();
        this.createObjects();
        this.setupEventListeners();
        this.animate();
    }

    createScene() {
        this.scene = new THREE.Scene();
        this.scene.background = new THREE.Color(0x222222);
        this.scene.fog = new THREE.Fog(0x222222, 50, 200);
    }

    createCamera() {
        this.camera = new THREE.PerspectiveCamera(
            75,
            window.innerWidth / window.innerHeight,
            0.1,
            1000
        );
        this.camera.position.set(0, 5, 10);
    }

    createRenderer() {
        this.renderer = new THREE.WebGLRenderer({
            antialias: true,
            alpha: true
        });
        this.renderer.setSize(window.innerWidth, window.innerHeight);
        this.renderer.setPixelRatio(window.devicePixelRatio);
        this.renderer.shadowMap.enabled = true;
        this.renderer.shadowMap.type = THREE.PCFSoftShadowMap;
        this.renderer.outputEncoding = THREE.sRGBEncoding;
        this.renderer.toneMapping = THREE.ACESFilmicToneMapping;
        this.renderer.toneMappingExposure = 1;

        document.body.appendChild(this.renderer.domElement);
    }

    createControls() {
        // Implement controls if needed
    }

    createLights() {
        // Implement lighting setup
    }

    createObjects() {
        // Implement object creation
    }

    setupEventListeners() {
        window.addEventListener('resize', () => this.onWindowResize());

        // Add other event listeners
        document.addEventListener('keydown', (event) => this.onKeyDown(event));
        this.renderer.domElement.addEventListener('click', (event) => this.onClick(event));
    }

    onWindowResize() {
        this.camera.aspect = window.innerWidth / window.innerHeight;
        this.camera.updateProjectionMatrix();
        this.renderer.setSize(window.innerWidth, window.innerHeight);
    }

    onKeyDown(event) {
        // Handle keyboard input
    }

    onClick(event) {
        // Handle mouse clicks
    }

    update() {
        const deltaTime = this.clock.getDelta();
        const elapsedTime = this.clock.getElapsedTime();

        // Update controls
        if (this.controls) {
            this.controls.update();
        }

        // Update animations
        this.animations.forEach(animation => {
            if (animation.update) {
                animation.update(deltaTime, elapsedTime);
            }
        });

        // Update objects
        this.objects.forEach(object => {
            if (object.update) {
                object.update(deltaTime, elapsedTime);
            }
        });
    }

    render() {
        this.renderer.render(this.scene, this.camera);
    }

    animate() {
        requestAnimationFrame(() => this.animate());
        this.update();
        this.render();
    }

    dispose() {
        // Clean up resources
        this.objects.forEach(object => {
            if (object.dispose) {
                object.dispose();
            }
        });

        this.renderer.dispose();

        // Remove event listeners
        window.removeEventListener('resize', this.onWindowResize);
    }
}

// Usage
const app = new ThreeJSApplication();
```

### Memory Management

```javascript
class ResourceManager {
    constructor() {
        this.geometries = new Map();
        this.materials = new Map();
        this.textures = new Map();
        this.meshes = new Set();
    }

    // Reuse geometries
    getGeometry(type, ...params) {
        const key = `${type}_${params.join('_')}`;

        if (!this.geometries.has(key)) {
            let geometry;
            switch (type) {
                case 'box':
                    geometry = new THREE.BoxGeometry(...params);
                    break;
                case 'sphere':
                    geometry = new THREE.SphereGeometry(...params);
                    break;
                // Add more geometry types
            }
            this.geometries.set(key, geometry);
        }

        return this.geometries.get(key);
    }

    // Reuse materials
    getMaterial(type, options = {}) {
        const key = `${type}_${JSON.stringify(options)}`;

        if (!this.materials.has(key)) {
            let material;
            switch (type) {
                case 'standard':
                    material = new THREE.MeshStandardMaterial(options);
                    break;
                case 'basic':
                    material = new THREE.MeshBasicMaterial(options);
                    break;
                // Add more material types
            }
            this.materials.set(key, material);
        }

        return this.materials.get(key);
    }

    // Track meshes for cleanup
    createMesh(geometry, material) {
        const mesh = new THREE.Mesh(geometry, material);
        this.meshes.add(mesh);
        return mesh;
    }

    // Dispose of resources
    dispose() {
        // Dispose geometries
        this.geometries.forEach(geometry => geometry.dispose());
        this.geometries.clear();

        // Dispose materials
        this.materials.forEach(material => {
            if (material.map) material.map.dispose();
            if (material.normalMap) material.normalMap.dispose();
            if (material.roughnessMap) material.roughnessMap.dispose();
            material.dispose();
        });
        this.materials.clear();

        // Dispose textures
        this.textures.forEach(texture => texture.dispose());
        this.textures.clear();

        // Clear mesh references
        this.meshes.clear();
    }
}

// Global resource manager
const resourceManager = new ResourceManager();

// Use it throughout your application
const boxGeometry = resourceManager.getGeometry('box', 1, 1, 1);
const standardMaterial = resourceManager.getMaterial('standard', { color: 0xff0000 });
const mesh = resourceManager.createMesh(boxGeometry, standardMaterial);
```

## Common Pitfalls

### Issue 1: Memory Leaks
Not properly disposing of geometries, materials, and textures.

**Solution:**
```javascript
// Always dispose of resources when removing objects
function removeObject(object, scene) {
    scene.remove(object);

    if (object.geometry) {
        object.geometry.dispose();
    }

    if (object.material) {
        if (Array.isArray(object.material)) {
            object.material.forEach(material => disposeMaterial(material));
        } else {
            disposeMaterial(object.material);
        }
    }
}

function disposeMaterial(material) {
    if (material.map) material.map.dispose();
    if (material.normalMap) material.normalMap.dispose();
    if (material.roughnessMap) material.roughnessMap.dispose();
    if (material.metalnessMap) material.metalnessMap.dispose();
    material.dispose();
}
```

### Issue 2: Performance Problems with Large Scenes
Too many draw calls and complex geometries can cause performance issues.

**Solution:**
```javascript
// Use instancing for repeated objects
// Use LOD for distant objects
// Implement frustum culling
// Use object pooling for dynamic objects

class ObjectPool {
    constructor(createFn, resetFn, initialSize = 10) {
        this.createFn = createFn;
        this.resetFn = resetFn;
        this.pool = [];
        this.active = [];

        for (let i = 0; i < initialSize; i++) {
            this.pool.push(this.createFn());
        }
    }

    get() {
        let obj = this.pool.pop();
        if (!obj) {
            obj = this.createFn();
        }
        this.active.push(obj);
        return obj;
    }

    release(obj) {
        const index = this.active.indexOf(obj);
        if (index !== -1) {
            this.active.splice(index, 1);
            this.resetFn(obj);
            this.pool.push(obj);
        }
    }
}
```

### Issue 3: Incorrect Lighting Setup
Poor lighting can make scenes look flat or unrealistic.

**Solution:**
```javascript
// Use a combination of different light types
function setupRealisticLighting(scene) {
    // Ambient light for base illumination
    const ambientLight = new THREE.AmbientLight(0x404040, 0.2);
    scene.add(ambientLight);

    // Main directional light (sun)
    const sunLight = new THREE.DirectionalLight(0xffffff, 0.8);
    sunLight.position.set(10, 10, 5);
    sunLight.castShadow = true;
    sunLight.shadow.mapSize.setScalar(2048);
    scene.add(sunLight);

    // Fill light to reduce harsh shadows
    const fillLight = new THREE.DirectionalLight(0x87CEEB, 0.3);
    fillLight.position.set(-5, 0, -5);
    scene.add(fillLight);

    // Rim light for edge definition
    const rimLight = new THREE.DirectionalLight(0xffffff, 0.2);
    rimLight.position.set(0, 5, -10);
    scene.add(rimLight);
}
```

### Issue 4: Z-Fighting and Depth Issues
Objects appearing to flicker when they overlap.

**Solution:**
```javascript
// Adjust camera near/far planes
camera.near = 0.1;
camera.far = 1000;
camera.updateProjectionMatrix();

// Use polygon offset for overlapping geometry
material.polygonOffset = true;
material.polygonOffsetFactor = -1;
material.polygonOffsetUnits = -1;

// Separate objects slightly in Z-depth
object1.position.z = 0;
object2.position.z = 0.001;
```

## References

{% embed url="https://threejs.org/" %}

{% embed url="https://threejs.org/docs/" %}

{% embed url="https://github.com/mrdoob/three.js/" %}

{% embed url="https://threejs.org/examples/" %}
```
```
```
