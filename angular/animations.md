---
description: >-
  Angular Animation & Motion Spells provide powerful mystical tools for creating
  smooth, engaging user interfaces. Master triggers, states, transitions, and
  advanced animation enchantment techniques.
---

# Angular Animation & Motion Spells

## The Ancient Knowledge

Angular's animation system is built on CSS mystical functionality, allowing you to animate any property that the browser considers animatable through magical transformation. This includes positions, sizes, transforms, colors, borders, and more. The Angular animations API makes it easy to define and control mystical animations in your Angular applications.

## Key Mystical Concepts

- **Triggers**: Define an animation spell and attach it to an element
- **States**: Define specific mystical styles for elements at different points in time
- **Transitions**: Define how to animate between mystical states
- **Keyframes**: Define mystical styles at specific points during a transition
- **Groups**: Run multiple animation spells in parallel
- **Sequences**: Run animation enchantments one after another

## Setup and Mystical Installation

### Installing Angular Animation Spells

```bash
# Angular Animations is included by default in new projects
npm install @angular/animations
```

### Basic Setup

```typescript
// app.module.ts
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

@NgModule({
  imports: [
    BrowserModule,
    BrowserAnimationsModule // Required for animations
  ],
  // ...
})
export class AppModule { }
```

## Basic Animation Spells

### Simple Fade Enchantment

```typescript
import { Component } from '@angular/core';
import { trigger, state, style, transition, animate } from '@angular/animations';

@Component({
  selector: 'app-fade-demo',
  template: `
    <div class="demo-container">
      <h2>Fade Animation</h2>
      
      <div class="controls">
        <button (click)="toggleVisibility()">Toggle Visibility</button>
        <button (click)="fadeIn()">Fade In</button>
        <button (click)="fadeOut()">Fade Out</button>
      </div>
      
      <div class="animation-area">
        <div 
          class="fade-box"
          [@fadeAnimation]="fadeState"
          (@fadeAnimation.start)="onAnimationStart($event)"
          (@fadeAnimation.done)="onAnimationDone($event)">
          <h3>Animated Content</h3>
          <p>This content fades in and out smoothly.</p>
        </div>
      </div>
      
      <div class="status">
        <p>Current State: {{ fadeState }}</p>
        <p>Animation Status: {{ animationStatus }}</p>
      </div>
    </div>
  `,
  styles: [`
    .demo-container {
      padding: 20px;
      max-width: 800px;
      margin: 0 auto;
    }
    .controls {
      margin: 20px 0;
    }
    .controls button {
      margin: 0 8px;
      padding: 8px 16px;
      background: #007bff;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    .animation-area {
      margin: 40px 0;
      min-height: 200px;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .fade-box {
      padding: 40px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white;
      border-radius: 12px;
      text-align: center;
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
    }
    .status {
      background: #f8f9fa;
      padding: 16px;
      border-radius: 8px;
      font-family: monospace;
    }
    h2 {
      color: #1976d2;
      text-align: center;
    }
  `],
  animations: [
    trigger('fadeAnimation', [
      state('in', style({ opacity: 1, transform: 'scale(1)' })),
      state('out', style({ opacity: 0, transform: 'scale(0.8)' })),
      transition('in => out', [
        animate('300ms ease-in')
      ]),
      transition('out => in', [
        animate('300ms ease-out')
      ]),
      transition(':enter', [
        style({ opacity: 0, transform: 'scale(0.8)' }),
        animate('300ms ease-out', style({ opacity: 1, transform: 'scale(1)' }))
      ]),
      transition(':leave', [
        animate('300ms ease-in', style({ opacity: 0, transform: 'scale(0.8)' }))
      ])
    ])
  ]
})
export class FadeDemoComponent {
  fadeState = 'in';
  animationStatus = 'idle';

  toggleVisibility(): void {
    this.fadeState = this.fadeState === 'in' ? 'out' : 'in';
  }

  fadeIn(): void {
    this.fadeState = 'in';
  }

  fadeOut(): void {
    this.fadeState = 'out';
  }

  onAnimationStart(event: any): void {
    this.animationStatus = `Animation started: ${event.fromState} => ${event.toState}`;
  }

  onAnimationDone(event: any): void {
    this.animationStatus = `Animation completed: ${event.fromState} => ${event.toState}`;
  }
}
```

### Slide Animations

```typescript
@Component({
  selector: 'app-slide-demo',
  template: `
    <div class="demo-container">
      <h2>Slide Animations</h2>
      
      <div class="controls">
        <button (click)="slideDirection = 'left'; triggerSlide()">Slide Left</button>
        <button (click)="slideDirection = 'right'; triggerSlide()">Slide Right</button>
        <button (click)="slideDirection = 'up'; triggerSlide()">Slide Up</button>
        <button (click)="slideDirection = 'down'; triggerSlide()">Slide Down</button>
      </div>
      
      <div class="slide-container">
        <div 
          class="slide-box"
          [@slideAnimation]="slideState">
          <h3>Sliding Content</h3>
          <p>Direction: {{ slideDirection }}</p>
          <p>State: {{ slideState }}</p>
        </div>
      </div>
      
      <div class="list-demo">
        <h3>List Slide Animation</h3>
        <button (click)="addItem()">Add Item</button>
        <button (click)="removeItem()">Remove Item</button>
        
        <ul class="animated-list">
          <li 
            *ngFor="let item of items; trackBy: trackByFn"
            [@listSlide]
            class="list-item">
            {{ item.name }}
          </li>
        </ul>
      </div>
    </div>
  `,
  styles: [`
    .demo-container {
      padding: 20px;
      max-width: 800px;
      margin: 0 auto;
    }
    .controls {
      margin: 20px 0;
      text-align: center;
    }
    .controls button {
      margin: 0 8px;
      padding: 8px 16px;
      background: #28a745;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    .slide-container {
      height: 300px;
      display: flex;
      justify-content: center;
      align-items: center;
      overflow: hidden;
      border: 2px dashed #ddd;
      border-radius: 8px;
      margin: 20px 0;
    }
    .slide-box {
      padding: 40px;
      background: linear-gradient(45deg, #ff6b6b, #feca57);
      color: white;
      border-radius: 12px;
      text-align: center;
      box-shadow: 0 8px 32px rgba(0, 0, 0, 0.2);
    }
    .list-demo {
      margin-top: 40px;
    }
    .animated-list {
      list-style: none;
      padding: 0;
      max-width: 400px;
    }
    .list-item {
      padding: 12px 16px;
      margin: 8px 0;
      background: #f8f9fa;
      border-left: 4px solid #007bff;
      border-radius: 4px;
    }
    h2, h3 {
      color: #1976d2;
    }
  `],
  animations: [
    trigger('slideAnimation', [
      state('left', style({ transform: 'translateX(-100px)' })),
      state('right', style({ transform: 'translateX(100px)' })),
      state('up', style({ transform: 'translateY(-100px)' })),
      state('down', style({ transform: 'translateY(100px)' })),
      state('center', style({ transform: 'translate(0, 0)' })),
      
      transition('* => left', [
        animate('400ms ease-in-out')
      ]),
      transition('* => right', [
        animate('400ms ease-in-out')
      ]),
      transition('* => up', [
        animate('400ms ease-in-out')
      ]),
      transition('* => down', [
        animate('400ms ease-in-out')
      ]),
      transition('* => center', [
        animate('400ms ease-in-out')
      ])
    ]),
    
    trigger('listSlide', [
      transition(':enter', [
        style({ 
          opacity: 0, 
          transform: 'translateX(-100%)',
          height: '0px',
          overflow: 'hidden'
        }),
        animate('300ms ease-out', style({ 
          opacity: 1, 
          transform: 'translateX(0)',
          height: '*'
        }))
      ]),
      transition(':leave', [
        animate('300ms ease-in', style({ 
          opacity: 0, 
          transform: 'translateX(100%)',
          height: '0px',
          overflow: 'hidden'
        }))
      ])
    ])
  ]
})
export class SlideDemoComponent {
  slideState = 'center';
  slideDirection = 'center';
  items = [
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
  ];
  private nextId = 4;

  triggerSlide(): void {
    this.slideState = this.slideDirection;
    
    // Return to center after animation
    setTimeout(() => {
      this.slideState = 'center';
    }, 500);
  }

  addItem(): void {
    this.items.push({
      id: this.nextId++,
      name: `Item ${this.nextId - 1}`
    });
  }

  removeItem(): void {
    if (this.items.length > 0) {
      this.items.pop();
    }
  }

  trackByFn(index: number, item: any): number {
    return item.id;
  }
}
```

### Advanced Keyframe Animations

```typescript
@Component({
  selector: 'app-keyframe-demo',
  template: `
    <div class="demo-container">
      <h2>Keyframe Animations</h2>
      
      <div class="controls">
        <button (click)="startBounce()">Bounce</button>
        <button (click)="startPulse()">Pulse</button>
        <button (click)="startRotate()">Rotate</button>
        <button (click)="startComplex()">Complex Animation</button>
        <button (click)="resetAnimation()">Reset</button>
      </div>
      
      <div class="animation-showcase">
        <div 
          class="keyframe-box"
          [@keyframeAnimation]="animationState">
          <div class="content">
            <h3>Keyframe Animation</h3>
            <p>{{ animationState }}</p>
          </div>
        </div>
      </div>
      
      <div class="stagger-demo">
        <h3>Stagger Animation</h3>
        <button (click)="toggleStagger()">Toggle Cards</button>
        
        <div class="cards-container" [@staggerAnimation]="showCards">
          <div 
            *ngFor="let card of cards; let i = index"
            class="stagger-card"
            [@cardAnimation]>
            <h4>Card {{ i + 1 }}</h4>
            <p>{{ card.content }}</p>
          </div>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo-container {
      padding: 20px;
      max-width: 1000px;
      margin: 0 auto;
    }
    .controls {
      margin: 20px 0;
      text-align: center;
    }
    .controls button {
      margin: 0 8px;
      padding: 8px 16px;
      background: #6f42c1;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    .animation-showcase {
      height: 400px;
      display: flex;
      justify-content: center;
      align-items: center;
      margin: 40px 0;
    }
    .keyframe-box {
      width: 200px;
      height: 200px;
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      border-radius: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
    }
    .content {
      text-align: center;
      color: white;
    }
    .stagger-demo {
      margin-top: 60px;
    }
    .cards-container {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 20px;
      margin-top: 20px;
    }
    .stagger-card {
      padding: 20px;
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
      border-left: 4px solid #007bff;
    }
    h2, h3 {
      color: #1976d2;
    }
  `],
  animations: [
    trigger('keyframeAnimation', [
      state('bounce', style({ transform: 'translateY(0)' })),
      state('pulse', style({ transform: 'scale(1)' })),
      state('rotate', style({ transform: 'rotate(0deg)' })),
      state('complex', style({ transform: 'translate(0, 0) rotate(0deg) scale(1)' })),
      state('idle', style({ transform: 'translate(0, 0) rotate(0deg) scale(1)' })),
      
      transition('* => bounce', [
        animate('1s', keyframes([
          style({ transform: 'translateY(0)', offset: 0 }),
          style({ transform: 'translateY(-30px)', offset: 0.3 }),
          style({ transform: 'translateY(0)', offset: 0.5 }),
          style({ transform: 'translateY(-15px)', offset: 0.7 }),
          style({ transform: 'translateY(0)', offset: 1.0 })
        ]))
      ]),
      
      transition('* => pulse', [
        animate('1s', keyframes([
          style({ transform: 'scale(1)', offset: 0 }),
          style({ transform: 'scale(1.1)', offset: 0.25 }),
          style({ transform: 'scale(1)', offset: 0.5 }),
          style({ transform: 'scale(1.05)', offset: 0.75 }),
          style({ transform: 'scale(1)', offset: 1.0 })
        ]))
      ]),
      
      transition('* => rotate', [
        animate('2s ease-in-out', keyframes([
          style({ transform: 'rotate(0deg)', offset: 0 }),
          style({ transform: 'rotate(180deg)', offset: 0.5 }),
          style({ transform: 'rotate(360deg)', offset: 1.0 })
        ]))
      ]),
      
      transition('* => complex', [
        animate('2s', keyframes([
          style({ 
            transform: 'translate(0, 0) rotate(0deg) scale(1)', 
            backgroundColor: '#667eea',
            offset: 0 
          }),
          style({ 
            transform: 'translate(50px, -50px) rotate(90deg) scale(1.2)', 
            backgroundColor: '#f093fb',
            offset: 0.25 
          }),
          style({ 
            transform: 'translate(0, -100px) rotate(180deg) scale(0.8)', 
            backgroundColor: '#f5576c',
            offset: 0.5 
          }),
          style({ 
            transform: 'translate(-50px, -50px) rotate(270deg) scale(1.1)', 
            backgroundColor: '#4facfe',
            offset: 0.75 
          }),
          style({ 
            transform: 'translate(0, 0) rotate(360deg) scale(1)', 
            backgroundColor: '#667eea',
            offset: 1.0 
          })
        ]))
      ])
    ]),
    
    trigger('staggerAnimation', [
      transition(':enter', [
        query('@cardAnimation', [
          stagger('100ms', animateChild())
        ], { optional: true })
      ])
    ]),
    
    trigger('cardAnimation', [
      transition(':enter', [
        style({ 
          opacity: 0, 
          transform: 'translateY(50px) scale(0.8)' 
        }),
        animate('400ms cubic-bezier(0.35, 0, 0.25, 1)', 
          style({ 
            opacity: 1, 
            transform: 'translateY(0) scale(1)' 
          })
        )
      ]),
      transition(':leave', [
        animate('300ms ease-in', 
          style({ 
            opacity: 0, 
            transform: 'translateY(-50px) scale(0.8)' 
          })
        )
      ])
    ])
  ]
})
export class KeyframeDemoComponent {
  animationState = 'idle';
  showCards = false;
  cards = [
    { content: 'This is the first card with some content.' },
    { content: 'Second card with different information.' },
    { content: 'Third card completing the set.' },
    { content: 'Fourth card for better demonstration.' }
  ];

  startBounce(): void {
    this.animationState = 'bounce';
  }

  startPulse(): void {
    this.animationState = 'pulse';
  }

  startRotate(): void {
    this.animationState = 'rotate';
  }

  startComplex(): void {
    this.animationState = 'complex';
  }

  resetAnimation(): void {
    this.animationState = 'idle';
  }

  toggleStagger(): void {
    this.showCards = !this.showCards;
  }
}
```
