---
description: >-
  Angular Directive Behavior Spells are mystical classes that add additional magical
  behavior to elements in your templates. Master built-in directive enchantments
  and learn to create custom mystical directives.
---

# Angular Directive Behavior Spells

## The Ancient Knowledge

Directives are mystical classes that add additional magical behavior to elements in your Angular templates. Angular has three kinds of directive enchantments:

1. **Components**: Directives with a template manifestation (most common mystical type)
2. **Structural Directives**: Change the DOM layout by adding/removing elements through mystical manipulation
3. **Attribute Directives**: Change the appearance or behavior of elements through magical enhancement

## Built-in Structural Directive Spells

### NgIf Conditional Manifestation Directive

```typescript
// Basic usage
@Component({
  selector: 'app-conditional-demo',
  template: `
    <div class="demo">
      <button (click)="toggleVisibility()">Toggle Content</button>
      
      <!-- Basic ngIf -->
      <div *ngIf="isVisible">
        <p>This content is conditionally displayed</p>
      </div>
      
      <!-- ngIf with else -->
      <div *ngIf="user; else noUser">
        <h3>Welcome, {{ user.name }}!</h3>
        <p>Email: {{ user.email }}</p>
      </div>
      
      <ng-template #noUser>
        <p>Please log in to see your profile</p>
      </ng-template>
      
      <!-- ngIf with then and else -->
      <div *ngIf="isLoading; then loading; else content"></div>
      
      <ng-template #loading>
        <div class="spinner">Loading...</div>
      </ng-template>
      
      <ng-template #content>
        <div class="data">{{ data | json }}</div>
      </ng-template>
      
      <!-- ngIf with as (storing result) -->
      <div *ngIf="getUserData() | async as userData">
        <h3>{{ userData.name }}</h3>
        <p>{{ userData.email }}</p>
      </div>
    </div>
  `
})
export class ConditionalDemoComponent {
  isVisible = true;
  isLoading = false;
  user: User | null = null;
  data: any = { message: 'Hello World' };

  constructor(private userService: UserService) {}

  toggleVisibility(): void {
    this.isVisible = !this.isVisible;
  }

  getUserData(): Observable<User> {
    return this.userService.getCurrentUser();
  }
}
```

### NgFor Directive

```typescript
@Component({
  selector: 'app-list-demo',
  template: `
    <div class="demo">
      <!-- Basic ngFor -->
      <ul>
        <li *ngFor="let item of items">{{ item }}</li>
      </ul>
      
      <!-- ngFor with index -->
      <div class="numbered-list">
        <div *ngFor="let user of users; let i = index" class="user-item">
          <span class="number">{{ i + 1 }}.</span>
          <span class="name">{{ user.name }}</span>
        </div>
      </div>
      
      <!-- ngFor with multiple variables -->
      <div class="advanced-list">
        <div *ngFor="let product of products; 
                     let i = index; 
                     let first = first; 
                     let last = last; 
                     let even = even; 
                     let odd = odd"
             [class.first]="first"
             [class.last]="last"
             [class.even]="even"
             [class.odd]="odd"
             class="product-item">
          <h4>{{ product.name }}</h4>
          <p>Price: {{ product.price | currency }}</p>
          <span class="position">Position: {{ i + 1 }}</span>
        </div>
      </div>
      
      <!-- ngFor with trackBy for performance -->
      <div class="optimized-list">
        <div *ngFor="let user of users; trackBy: trackByUserId" class="user-card">
          <img [src]="user.avatar" [alt]="user.name">
          <h4>{{ user.name }}</h4>
          <p>{{ user.email }}</p>
          <button (click)="updateUser(user)">Update</button>
        </div>
      </div>
      
      <!-- ngFor with objects -->
      <div class="key-value-list">
        <div *ngFor="let item of objectEntries" class="kv-pair">
          <strong>{{ item.key }}:</strong> {{ item.value }}
        </div>
      </div>
    </div>
  `,
  styles: [`
    .user-item { display: flex; align-items: center; margin: 8px 0; }
    .number { margin-right: 8px; font-weight: bold; }
    .product-item { padding: 16px; margin: 8px 0; border: 1px solid #ddd; }
    .product-item.first { border-top: 3px solid #4caf50; }
    .product-item.last { border-bottom: 3px solid #f44336; }
    .product-item.even { background-color: #f9f9f9; }
    .user-card { display: flex; align-items: center; gap: 12px; padding: 12px; }
    .kv-pair { margin: 4px 0; }
  `]
})
export class ListDemoComponent {
  items = ['Apple', 'Banana', 'Orange', 'Grape'];
  
  users = [
    { id: 1, name: 'John Doe', email: 'john@example.com', avatar: 'assets/john.jpg' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com', avatar: 'assets/jane.jpg' },
    { id: 3, name: 'Bob Johnson', email: 'bob@example.com', avatar: 'assets/bob.jpg' }
  ];
  
  products = [
    { id: 1, name: 'Laptop', price: 999.99 },
    { id: 2, name: 'Mouse', price: 29.99 },
    { id: 3, name: 'Keyboard', price: 79.99 }
  ];
  
  userSettings = {
    theme: 'dark',
    language: 'en',
    notifications: true,
    autoSave: false
  };

  get objectEntries() {
    return Object.entries(this.userSettings).map(([key, value]) => ({ key, value }));
  }

  // TrackBy function for performance optimization
  trackByUserId(index: number, user: any): number {
    return user.id;
  }

  updateUser(user: any): void {
    // Simulate user update
    user.name = user.name + ' (Updated)';
  }
}
```

### NgSwitch Directive

```typescript
@Component({
  selector: 'app-switch-demo',
  template: `
    <div class="demo">
      <div class="controls">
        <label>Select View:</label>
        <select [(ngModel)]="currentView">
          <option value="list">List View</option>
          <option value="grid">Grid View</option>
          <option value="table">Table View</option>
          <option value="card">Card View</option>
        </select>
      </div>
      
      <div class="content" [ngSwitch]="currentView">
        <!-- List View -->
        <div *ngSwitchCase="'list'" class="list-view">
          <h3>List View</h3>
          <ul>
            <li *ngFor="let item of items">{{ item.name }}</li>
          </ul>
        </div>
        
        <!-- Grid View -->
        <div *ngSwitchCase="'grid'" class="grid-view">
          <h3>Grid View</h3>
          <div class="grid">
            <div *ngFor="let item of items" class="grid-item">
              <img [src]="item.image" [alt]="item.name">
              <p>{{ item.name }}</p>
            </div>
          </div>
        </div>
        
        <!-- Table View -->
        <div *ngSwitchCase="'table'" class="table-view">
          <h3>Table View</h3>
          <table>
            <thead>
              <tr>
                <th>Name</th>
                <th>Price</th>
                <th>Category</th>
              </tr>
            </thead>
            <tbody>
              <tr *ngFor="let item of items">
                <td>{{ item.name }}</td>
                <td>{{ item.price | currency }}</td>
                <td>{{ item.category }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        
        <!-- Card View -->
        <div *ngSwitchCase="'card'" class="card-view">
          <h3>Card View</h3>
          <div class="cards">
            <div *ngFor="let item of items" class="card">
              <img [src]="item.image" [alt]="item.name">
              <div class="card-content">
                <h4>{{ item.name }}</h4>
                <p>{{ item.description }}</p>
                <span class="price">{{ item.price | currency }}</span>
              </div>
            </div>
          </div>
        </div>
        
        <!-- Default case -->
        <div *ngSwitchDefault>
          <p>Please select a view type</p>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .controls { margin-bottom: 20px; }
    .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 16px; }
    .grid-item { text-align: center; padding: 16px; border: 1px solid #ddd; }
    .grid-item img { width: 100%; height: 150px; object-fit: cover; }
    table { width: 100%; border-collapse: collapse; }
    th, td { padding: 12px; text-align: left; border-bottom: 1px solid #ddd; }
    .cards { display: flex; flex-wrap: wrap; gap: 16px; }
    .card { width: 300px; border: 1px solid #ddd; border-radius: 8px; overflow: hidden; }
    .card img { width: 100%; height: 200px; object-fit: cover; }
    .card-content { padding: 16px; }
    .price { font-weight: bold; color: #4caf50; }
  `]
})
export class SwitchDemoComponent {
  currentView = 'list';
  
  items = [
    {
      id: 1,
      name: 'Laptop',
      price: 999.99,
      category: 'Electronics',
      description: 'High-performance laptop for work and gaming',
      image: 'assets/laptop.jpg'
    },
    {
      id: 2,
      name: 'Coffee Mug',
      price: 15.99,
      category: 'Kitchen',
      description: 'Ceramic coffee mug with ergonomic handle',
      image: 'assets/mug.jpg'
    },
    {
      id: 3,
      name: 'Book',
      price: 24.99,
      category: 'Education',
      description: 'Learn Angular development from scratch',
      image: 'assets/book.jpg'
    }
  ];
}
```

## Built-in Attribute Directive Enchantments

### NgClass Styling Spell Directive

```typescript
@Component({
  selector: 'app-class-demo',
  template: `
    <div class="demo">
      <div class="controls">
        <button (click)="toggleActive()">Toggle Active</button>
        <button (click)="toggleHighlight()">Toggle Highlight</button>
        <button (click)="changeSize()">Change Size</button>
        <button (click)="toggleDisabled()">Toggle Disabled</button>
      </div>
      
      <!-- String binding -->
      <div [ngClass]="currentClasses" class="box">
        String Classes: {{ currentClasses }}
      </div>
      
      <!-- Array binding -->
      <div [ngClass]="classArray" class="box">
        Array Classes
      </div>
      
      <!-- Object binding -->
      <div [ngClass]="{
        'active': isActive,
        'highlight': isHighlighted,
        'large': size === 'large',
        'medium': size === 'medium',
        'small': size === 'small',
        'disabled': isDisabled
      }" class="box">
        Object Classes
      </div>
      
      <!-- Method binding -->
      <div [ngClass]="getClasses()" class="box">
        Method Classes
      </div>
      
      <!-- Conditional classes with ternary -->
      <div [ngClass]="isActive ? 'active highlight' : 'inactive'" class="box">
        Ternary Classes
      </div>
    </div>
  `,
  styles: [`
    .box {
      padding: 20px;
      margin: 10px 0;
      border: 2px solid #ddd;
      border-radius: 8px;
      transition: all 0.3s ease;
    }
    .active { border-color: #4caf50; background-color: #e8f5e8; }
    .highlight { box-shadow: 0 0 10px rgba(255, 193, 7, 0.5); }
    .large { font-size: 18px; padding: 30px; }
    .medium { font-size: 16px; padding: 20px; }
    .small { font-size: 14px; padding: 10px; }
    .disabled { opacity: 0.5; pointer-events: none; }
    .inactive { background-color: #f5f5f5; color: #999; }
  `]
})
export class ClassDemoComponent {
  isActive = false;
  isHighlighted = false;
  isDisabled = false;
  size: 'small' | 'medium' | 'large' = 'medium';
  currentClasses = 'box medium';
  
  get classArray(): string[] {
    const classes = ['box'];
    if (this.isActive) classes.push('active');
    if (this.isHighlighted) classes.push('highlight');
    classes.push(this.size);
    return classes;
  }

  toggleActive(): void {
    this.isActive = !this.isActive;
    this.updateCurrentClasses();
  }

  toggleHighlight(): void {
    this.isHighlighted = !this.isHighlighted;
  }

  changeSize(): void {
    const sizes: Array<'small' | 'medium' | 'large'> = ['small', 'medium', 'large'];
    const currentIndex = sizes.indexOf(this.size);
    this.size = sizes[(currentIndex + 1) % sizes.length];
    this.updateCurrentClasses();
  }

  toggleDisabled(): void {
    this.isDisabled = !this.isDisabled;
  }

  getClasses(): { [key: string]: boolean } {
    return {
      'active': this.isActive,
      'highlight': this.isHighlighted,
      [this.size]: true,
      'disabled': this.isDisabled
    };
  }

  private updateCurrentClasses(): void {
    let classes = 'box';
    if (this.isActive) classes += ' active';
    if (this.isHighlighted) classes += ' highlight';
    classes += ` ${this.size}`;
    this.currentClasses = classes;
  }
}

### NgStyle Directive

```typescript
@Component({
  selector: 'app-style-demo',
  template: `
    <div class="demo">
      <div class="controls">
        <label>Background Color:</label>
        <input type="color" [(ngModel)]="backgroundColor">

        <label>Text Color:</label>
        <input type="color" [(ngModel)]="textColor">

        <label>Font Size:</label>
        <input type="range" min="12" max="32" [(ngModel)]="fontSize">
        <span>{{ fontSize }}px</span>

        <label>Border Width:</label>
        <input type="range" min="0" max="10" [(ngModel)]="borderWidth">
        <span>{{ borderWidth }}px</span>

        <button (click)="randomizeStyles()">Randomize</button>
      </div>

      <!-- Object binding -->
      <div [ngStyle]="{
        'background-color': backgroundColor,
        'color': textColor,
        'font-size.px': fontSize,
        'border-width.px': borderWidth,
        'border-style': 'solid',
        'border-color': borderColor,
        'padding': '20px',
        'margin': '10px 0',
        'border-radius': '8px',
        'transition': 'all 0.3s ease'
      }" class="styled-box">
        Object Style Binding
      </div>

      <!-- Method binding -->
      <div [ngStyle]="getStyles()" class="styled-box">
        Method Style Binding
      </div>

      <!-- Conditional styles -->
      <div [ngStyle]="isHighlighted ? highlightStyles : normalStyles" class="styled-box">
        <button (click)="toggleHighlight()">Toggle Highlight</button>
        Conditional Styles
      </div>

      <!-- Dynamic styles based on data -->
      <div class="progress-bars">
        <div *ngFor="let item of progressItems" class="progress-container">
          <label>{{ item.label }}</label>
          <div class="progress-bar">
            <div class="progress-fill"
                 [ngStyle]="{
                   'width.%': item.value,
                   'background-color': getProgressColor(item.value),
                   'transition': 'all 0.5s ease'
                 }">
              {{ item.value }}%
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .controls {
      display: grid;
      grid-template-columns: auto 1fr;
      gap: 8px 16px;
      align-items: center;
      margin-bottom: 20px;
      padding: 16px;
      background-color: #f5f5f5;
      border-radius: 8px;
    }
    .styled-box {
      min-height: 60px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .progress-container {
      margin: 8px 0;
    }
    .progress-bar {
      width: 100%;
      height: 30px;
      background-color: #e0e0e0;
      border-radius: 15px;
      overflow: hidden;
      position: relative;
    }
    .progress-fill {
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-weight: bold;
      min-width: 50px;
    }
  `]
})
export class StyleDemoComponent {
  backgroundColor = '#ffffff';
  textColor = '#000000';
  fontSize = 16;
  borderWidth = 2;
  borderColor = '#cccccc';
  isHighlighted = false;

  progressItems = [
    { label: 'HTML', value: 90 },
    { label: 'CSS', value: 85 },
    { label: 'JavaScript', value: 80 },
    { label: 'Angular', value: 75 },
    { label: 'TypeScript', value: 70 }
  ];

  highlightStyles = {
    'background-color': '#ffeb3b',
    'color': '#333',
    'box-shadow': '0 0 20px rgba(255, 235, 59, 0.8)',
    'transform': 'scale(1.05)'
  };

  normalStyles = {
    'background-color': '#f5f5f5',
    'color': '#666',
    'box-shadow': 'none',
    'transform': 'scale(1)'
  };

  getStyles(): { [key: string]: string } {
    return {
      'background': `linear-gradient(45deg, ${this.backgroundColor}, ${this.textColor})`,
      'color': this.getContrastColor(this.backgroundColor),
      'font-size': `${this.fontSize}px`,
      'border': `${this.borderWidth}px solid ${this.borderColor}`,
      'padding': '20px',
      'margin': '10px 0',
      'border-radius': '8px',
      'text-align': 'center'
    };
  }

  toggleHighlight(): void {
    this.isHighlighted = !this.isHighlighted;
  }

  randomizeStyles(): void {
    this.backgroundColor = this.getRandomColor();
    this.textColor = this.getRandomColor();
    this.fontSize = Math.floor(Math.random() * 20) + 12;
    this.borderWidth = Math.floor(Math.random() * 10);
    this.borderColor = this.getRandomColor();
  }

  getProgressColor(value: number): string {
    if (value >= 80) return '#4caf50';
    if (value >= 60) return '#ff9800';
    if (value >= 40) return '#ffc107';
    return '#f44336';
  }

  private getRandomColor(): string {
    return '#' + Math.floor(Math.random() * 16777215).toString(16).padStart(6, '0');
  }

  private getContrastColor(hexColor: string): string {
    const r = parseInt(hexColor.slice(1, 3), 16);
    const g = parseInt(hexColor.slice(3, 5), 16);
    const b = parseInt(hexColor.slice(5, 7), 16);
    const brightness = (r * 299 + g * 587 + b * 114) / 1000;
    return brightness > 128 ? '#000000' : '#ffffff';
  }
}
```

## Custom Attribute Directive Enchantments

### Basic Attribute Directive Spell

```typescript
// highlight.directive.ts
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() appHighlight = 'yellow';
  @Input() defaultColor = 'transparent';

  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.appHighlight);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(this.defaultColor);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}

// Usage in template
@Component({
  template: `
    <div class="demo">
      <p appHighlight>Hover over me (default yellow)</p>
      <p appHighlight="lightblue">Hover over me (light blue)</p>
      <p [appHighlight]="color" defaultColor="lightgray">
        Hover over me (dynamic color)
      </p>

      <input [(ngModel)]="color" placeholder="Enter color">
    </div>
  `
})
export class HighlightDemoComponent {
  color = 'lightgreen';
}
```

### Advanced Attribute Directive

```typescript
// tooltip.directive.ts
import {
  Directive, ElementRef, HostListener, Input,
  Renderer2, ViewContainerRef, ComponentFactoryResolver,
  ComponentRef, OnDestroy
} from '@angular/core';

@Component({
  selector: 'app-tooltip',
  template: `
    <div class="tooltip" [class]="'tooltip-' + position">
      {{ content }}
      <div class="tooltip-arrow"></div>
    </div>
  `,
  styles: [`
    .tooltip {
      position: absolute;
      background-color: #333;
      color: white;
      padding: 8px 12px;
      border-radius: 4px;
      font-size: 14px;
      z-index: 1000;
      max-width: 200px;
      word-wrap: break-word;
    }
    .tooltip-arrow {
      position: absolute;
      width: 0;
      height: 0;
    }
    .tooltip-top {
      margin-bottom: 8px;
    }
    .tooltip-top .tooltip-arrow {
      bottom: -8px;
      left: 50%;
      transform: translateX(-50%);
      border-left: 8px solid transparent;
      border-right: 8px solid transparent;
      border-top: 8px solid #333;
    }
    .tooltip-bottom {
      margin-top: 8px;
    }
    .tooltip-bottom .tooltip-arrow {
      top: -8px;
      left: 50%;
      transform: translateX(-50%);
      border-left: 8px solid transparent;
      border-right: 8px solid transparent;
      border-bottom: 8px solid #333;
    }
  `]
})
export class TooltipComponent {
  @Input() content = '';
  @Input() position: 'top' | 'bottom' | 'left' | 'right' = 'top';
}

@Directive({
  selector: '[appTooltip]'
})
export class TooltipDirective implements OnDestroy {
  @Input() appTooltip = '';
  @Input() tooltipPosition: 'top' | 'bottom' | 'left' | 'right' = 'top';
  @Input() tooltipDelay = 500;

  private tooltipComponent: ComponentRef<TooltipComponent> | null = null;
  private showTimeout: any;
  private hideTimeout: any;

  constructor(
    private el: ElementRef,
    private renderer: Renderer2,
    private viewContainer: ViewContainerRef,
    private componentFactoryResolver: ComponentFactoryResolver
  ) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.clearTimeouts();
    this.showTimeout = setTimeout(() => {
      this.showTooltip();
    }, this.tooltipDelay);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.clearTimeouts();
    this.hideTimeout = setTimeout(() => {
      this.hideTooltip();
    }, 100);
  }

  private showTooltip() {
    if (this.tooltipComponent || !this.appTooltip) return;

    const factory = this.componentFactoryResolver.resolveComponentFactory(TooltipComponent);
    this.tooltipComponent = this.viewContainer.createComponent(factory);

    this.tooltipComponent.instance.content = this.appTooltip;
    this.tooltipComponent.instance.position = this.tooltipPosition;

    const tooltipElement = this.tooltipComponent.location.nativeElement;
    this.renderer.appendChild(document.body, tooltipElement);

    this.positionTooltip(tooltipElement);
  }

  private hideTooltip() {
    if (this.tooltipComponent) {
      this.tooltipComponent.destroy();
      this.tooltipComponent = null;
    }
  }

  private positionTooltip(tooltipElement: HTMLElement) {
    const hostRect = this.el.nativeElement.getBoundingClientRect();
    const tooltipRect = tooltipElement.getBoundingClientRect();

    let top: number;
    let left: number;

    switch (this.tooltipPosition) {
      case 'top':
        top = hostRect.top - tooltipRect.height - 8;
        left = hostRect.left + (hostRect.width - tooltipRect.width) / 2;
        break;
      case 'bottom':
        top = hostRect.bottom + 8;
        left = hostRect.left + (hostRect.width - tooltipRect.width) / 2;
        break;
      case 'left':
        top = hostRect.top + (hostRect.height - tooltipRect.height) / 2;
        left = hostRect.left - tooltipRect.width - 8;
        break;
      case 'right':
        top = hostRect.top + (hostRect.height - tooltipRect.height) / 2;
        left = hostRect.right + 8;
        break;
    }

    this.renderer.setStyle(tooltipElement, 'top', `${top + window.scrollY}px`);
    this.renderer.setStyle(tooltipElement, 'left', `${left + window.scrollX}px`);
  }

  private clearTimeouts() {
    if (this.showTimeout) {
      clearTimeout(this.showTimeout);
      this.showTimeout = null;
    }
    if (this.hideTimeout) {
      clearTimeout(this.hideTimeout);
      this.hideTimeout = null;
    }
  }

  ngOnDestroy() {
    this.clearTimeouts();
    this.hideTooltip();
  }
}

// Usage
@Component({
  template: `
    <div class="demo">
      <button appTooltip="This is a tooltip" tooltipPosition="top">
        Hover me (top)
      </button>

      <button appTooltip="Bottom tooltip" tooltipPosition="bottom">
        Hover me (bottom)
      </button>

      <button [appTooltip]="dynamicTooltip" [tooltipDelay]="1000">
        Hover me (dynamic)
      </button>
    </div>
  `
})
export class TooltipDemoComponent {
  dynamicTooltip = 'This tooltip has a longer delay';
}

## Custom Structural Directive Spells

### Basic Structural Directive Magic

```typescript
// unless.directive.ts
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]'
})
export class UnlessDirective {
  private hasView = false;

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
  ) {}

  @Input() set appUnless(condition: boolean) {
    if (!condition && !this.hasView) {
      this.viewContainer.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (condition && this.hasView) {
      this.viewContainer.clear();
      this.hasView = false;
    }
  }
}

// Usage
@Component({
  template: `
    <div class="demo">
      <button (click)="toggleCondition()">Toggle Condition</button>
      <p>Condition is: {{ condition }}</p>

      <div *appUnless="condition">
        <p>This content is shown when condition is FALSE</p>
      </div>

      <div *ngIf="!condition">
        <p>This is equivalent using ngIf</p>
      </div>
    </div>
  `
})
export class UnlessDemoComponent {
  condition = false;

  toggleCondition(): void {
    this.condition = !this.condition;
  }
}
```

### Advanced Structural Directive with Context

```typescript
// repeat.directive.ts
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

export interface RepeatContext {
  $implicit: number;
  index: number;
  count: number;
  first: boolean;
  last: boolean;
  even: boolean;
  odd: boolean;
}

@Directive({
  selector: '[appRepeat]'
})
export class RepeatDirective {
  constructor(
    private templateRef: TemplateRef<RepeatContext>,
    private viewContainer: ViewContainerRef
  ) {}

  @Input() set appRepeat(count: number) {
    this.viewContainer.clear();

    for (let i = 0; i < count; i++) {
      const context: RepeatContext = {
        $implicit: i + 1,
        index: i,
        count: count,
        first: i === 0,
        last: i === count - 1,
        even: i % 2 === 0,
        odd: i % 2 === 1
      };

      this.viewContainer.createEmbeddedView(this.templateRef, context);
    }
  }
}

// range.directive.ts
@Directive({
  selector: '[appRange]'
})
export class RangeDirective {
  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
  ) {}

  @Input() set appRange(range: { from: number; to: number; step?: number }) {
    this.viewContainer.clear();

    const step = range.step || 1;
    let index = 0;

    for (let i = range.from; i <= range.to; i += step) {
      const context = {
        $implicit: i,
        index: index++,
        value: i
      };

      this.viewContainer.createEmbeddedView(this.templateRef, context);
    }
  }
}

// Usage
@Component({
  template: `
    <div class="demo">
      <h3>Repeat Directive</h3>
      <div class="repeat-demo">
        <div *appRepeat="5; let num; let i = index; let isFirst = first; let isLast = last"
             [class.first]="isFirst"
             [class.last]="isLast"
             class="repeat-item">
          Item {{ num }} (index: {{ i }})
        </div>
      </div>

      <h3>Range Directive</h3>
      <div class="range-demo">
        <span *appRange="{ from: 1, to: 10, step: 2 }; let value"
              class="range-item">
          {{ value }}
        </span>
      </div>

      <h3>Dynamic Range</h3>
      <div class="controls">
        <label>From: <input type="number" [(ngModel)]="rangeFrom"></label>
        <label>To: <input type="number" [(ngModel)]="rangeTo"></label>
        <label>Step: <input type="number" [(ngModel)]="rangeStep"></label>
      </div>

      <div class="dynamic-range">
        <div *appRange="{ from: rangeFrom, to: rangeTo, step: rangeStep }; let value; let i = index"
             class="range-value">
          {{ value }}
        </div>
      </div>
    </div>
  `,
  styles: [`
    .repeat-item {
      padding: 8px 16px;
      margin: 4px;
      background-color: #f0f0f0;
      border-radius: 4px;
      display: inline-block;
    }
    .repeat-item.first { background-color: #4caf50; color: white; }
    .repeat-item.last { background-color: #f44336; color: white; }
    .range-item {
      display: inline-block;
      padding: 4px 8px;
      margin: 2px;
      background-color: #2196f3;
      color: white;
      border-radius: 4px;
    }
    .controls {
      display: flex;
      gap: 16px;
      margin: 16px 0;
    }
    .range-value {
      display: inline-block;
      padding: 8px;
      margin: 4px;
      background-color: #ff9800;
      color: white;
      border-radius: 4px;
      min-width: 40px;
      text-align: center;
    }
  `]
})
export class StructuralDirectiveDemoComponent {
  rangeFrom = 1;
  rangeTo = 10;
  rangeStep = 1;
}
```

### Permission-based Structural Directive

```typescript
// has-permission.directive.ts
import { Directive, Input, TemplateRef, ViewContainerRef, OnInit, OnDestroy } from '@angular/core';
import { Subject } from 'rxjs';
import { takeUntil } from 'rxjs/operators';
import { AuthService } from '../services/auth.service';

@Directive({
  selector: '[appHasPermission]'
})
export class HasPermissionDirective implements OnInit, OnDestroy {
  private destroy$ = new Subject<void>();
  private hasView = false;
  private permissions: string[] = [];

  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef,
    private authService: AuthService
  ) {}

  @Input() set appHasPermission(permissions: string | string[]) {
    this.permissions = Array.isArray(permissions) ? permissions : [permissions];
    this.updateView();
  }

  ngOnInit(): void {
    // Listen for permission changes
    this.authService.permissions$
      .pipe(takeUntil(this.destroy$))
      .subscribe(() => {
        this.updateView();
      });
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }

  private updateView(): void {
    const hasPermission = this.checkPermissions();

    if (hasPermission && !this.hasView) {
      this.viewContainer.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (!hasPermission && this.hasView) {
      this.viewContainer.clear();
      this.hasView = false;
    }
  }

  private checkPermissions(): boolean {
    if (this.permissions.length === 0) return true;

    const userPermissions = this.authService.getUserPermissions();
    return this.permissions.some(permission =>
      userPermissions.includes(permission)
    );
  }
}

// Usage
@Component({
  template: `
    <div class="demo">
      <div class="user-info">
        <p>Current user permissions: {{ userPermissions | json }}</p>
        <button (click)="toggleAdminPermission()">Toggle Admin Permission</button>
      </div>

      <div class="permission-demo">
        <div *appHasPermission="'read'">
          <p>✅ You can read content</p>
        </div>

        <div *appHasPermission="'write'">
          <p>✅ You can write content</p>
        </div>

        <div *appHasPermission="'admin'">
          <p>✅ You have admin access</p>
          <button>Admin Panel</button>
        </div>

        <div *appHasPermission="['admin', 'moderator']">
          <p>✅ You can moderate content</p>
        </div>

        <div *appHasPermission="'delete'">
          <p>✅ You can delete content</p>
          <button class="danger">Delete</button>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .user-info {
      background-color: #f5f5f5;
      padding: 16px;
      border-radius: 8px;
      margin-bottom: 16px;
    }
    .permission-demo > div {
      padding: 12px;
      margin: 8px 0;
      border: 1px solid #ddd;
      border-radius: 4px;
      background-color: #e8f5e8;
    }
    .danger {
      background-color: #f44336;
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 4px;
      cursor: pointer;
    }
  `]
})
export class PermissionDemoComponent {
  get userPermissions(): string[] {
    return this.authService.getUserPermissions();
  }

  constructor(private authService: AuthService) {}

  toggleAdminPermission(): void {
    const currentPermissions = this.authService.getUserPermissions();
    if (currentPermissions.includes('admin')) {
      this.authService.removePermission('admin');
    } else {
      this.authService.addPermission('admin');
    }
  }
}
```

## Directive Composition and Host Mystical Directives

### Host Directive Enchantments (Angular 15+)

```typescript
// base-button.directive.ts
@Directive({
  selector: '[appBaseButton]',
  host: {
    'class': 'base-button',
    '[class.disabled]': 'disabled',
    '(click)': 'onClick($event)'
  }
})
export class BaseButtonDirective {
  @Input() disabled = false;
  @Output() buttonClick = new EventEmitter<Event>();

  onClick(event: Event): void {
    if (!this.disabled) {
      this.buttonClick.emit(event);
    }
  }
}

// loading.directive.ts
@Directive({
  selector: '[appLoading]',
  host: {
    '[class.loading]': 'isLoading',
    '[attr.aria-busy]': 'isLoading'
  }
})
export class LoadingDirective {
  @Input() isLoading = false;
}

// primary-button.directive.ts
@Directive({
  selector: '[appPrimaryButton]',
  hostDirectives: [
    BaseButtonDirective,
    {
      directive: LoadingDirective,
      inputs: ['isLoading']
    }
  ],
  host: {
    'class': 'primary-button'
  }
})
export class PrimaryButtonDirective {
  constructor(private baseButton: BaseButtonDirective) {
    // Access host directive instance
    this.baseButton.buttonClick.subscribe(event => {
      console.log('Primary button clicked');
    });
  }
}

// Usage
@Component({
  template: `
    <button appPrimaryButton
            [disabled]="isDisabled"
            [isLoading]="isLoading"
            (buttonClick)="onPrimaryClick()">
      Primary Button
    </button>
  `
})
export class HostDirectiveDemoComponent {
  isDisabled = false;
  isLoading = false;

  onPrimaryClick(): void {
    this.isLoading = true;
    setTimeout(() => {
      this.isLoading = false;
    }, 2000);
  }
}

## Testing Directive Enchantments

### Testing Attribute Directive Spells

```typescript
// highlight.directive.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { Component, DebugElement } from '@angular/core';
import { By } from '@angular/platform-browser';
import { HighlightDirective } from './highlight.directive';

@Component({
  template: `
    <p appHighlight>Default highlight</p>
    <p appHighlight="blue">Blue highlight</p>
    <p [appHighlight]="color" defaultColor="gray">Dynamic highlight</p>
  `
})
class TestComponent {
  color = 'red';
}

describe('HighlightDirective', () => {
  let fixture: ComponentFixture<TestComponent>;
  let component: TestComponent;
  let elements: DebugElement[];

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [HighlightDirective, TestComponent]
    }).compileComponents();

    fixture = TestBed.createComponent(TestComponent);
    component = fixture.componentInstance;
    elements = fixture.debugElement.queryAll(By.directive(HighlightDirective));
  });

  it('should create directive instances', () => {
    expect(elements.length).toBe(3);
  });

  it('should highlight with default color on mouseenter', () => {
    const firstElement = elements[0];

    firstElement.triggerEventHandler('mouseenter', null);

    expect(firstElement.nativeElement.style.backgroundColor).toBe('yellow');
  });

  it('should highlight with custom color', () => {
    const secondElement = elements[1];

    secondElement.triggerEventHandler('mouseenter', null);

    expect(secondElement.nativeElement.style.backgroundColor).toBe('blue');
  });

  it('should remove highlight on mouseleave', () => {
    const firstElement = elements[0];

    firstElement.triggerEventHandler('mouseenter', null);
    firstElement.triggerEventHandler('mouseleave', null);

    expect(firstElement.nativeElement.style.backgroundColor).toBe('transparent');
  });

  it('should use default color on mouseleave', () => {
    const thirdElement = elements[2];

    thirdElement.triggerEventHandler('mouseenter', null);
    thirdElement.triggerEventHandler('mouseleave', null);

    expect(thirdElement.nativeElement.style.backgroundColor).toBe('gray');
  });

  it('should update highlight color when input changes', () => {
    const thirdElement = elements[2];

    component.color = 'green';
    fixture.detectChanges();

    thirdElement.triggerEventHandler('mouseenter', null);

    expect(thirdElement.nativeElement.style.backgroundColor).toBe('green');
  });
});
```

### Testing Structural Directives

```typescript
// unless.directive.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { Component } from '@angular/core';
import { UnlessDirective } from './unless.directive';

@Component({
  template: `
    <div *appUnless="condition" class="content">
      Content to show/hide
    </div>
  `
})
class TestComponent {
  condition = false;
}

describe('UnlessDirective', () => {
  let fixture: ComponentFixture<TestComponent>;
  let component: TestComponent;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [UnlessDirective, TestComponent]
    }).compileComponents();

    fixture = TestBed.createComponent(TestComponent);
    component = fixture.componentInstance;
  });

  it('should show content when condition is false', () => {
    component.condition = false;
    fixture.detectChanges();

    const contentElement = fixture.nativeElement.querySelector('.content');
    expect(contentElement).toBeTruthy();
    expect(contentElement.textContent.trim()).toBe('Content to show/hide');
  });

  it('should hide content when condition is true', () => {
    component.condition = true;
    fixture.detectChanges();

    const contentElement = fixture.nativeElement.querySelector('.content');
    expect(contentElement).toBeFalsy();
  });

  it('should toggle content visibility when condition changes', () => {
    // Initially false - content should be visible
    component.condition = false;
    fixture.detectChanges();
    expect(fixture.nativeElement.querySelector('.content')).toBeTruthy();

    // Change to true - content should be hidden
    component.condition = true;
    fixture.detectChanges();
    expect(fixture.nativeElement.querySelector('.content')).toBeFalsy();

    // Change back to false - content should be visible again
    component.condition = false;
    fixture.detectChanges();
    expect(fixture.nativeElement.querySelector('.content')).toBeTruthy();
  });
});

// repeat.directive.spec.ts
describe('RepeatDirective', () => {
  let fixture: ComponentFixture<TestComponent>;
  let component: TestComponent;

  @Component({
    template: `
      <div *appRepeat="count; let num; let i = index" class="repeat-item">
        Item {{ num }} ({{ i }})
      </div>
    `
  })
  class TestComponent {
    count = 3;
  }

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [RepeatDirective, TestComponent]
    }).compileComponents();

    fixture = TestBed.createComponent(TestComponent);
    component = fixture.componentInstance;
  });

  it('should create correct number of elements', () => {
    fixture.detectChanges();

    const elements = fixture.nativeElement.querySelectorAll('.repeat-item');
    expect(elements.length).toBe(3);
  });

  it('should provide correct context values', () => {
    fixture.detectChanges();

    const elements = fixture.nativeElement.querySelectorAll('.repeat-item');
    expect(elements[0].textContent.trim()).toBe('Item 1 (0)');
    expect(elements[1].textContent.trim()).toBe('Item 2 (1)');
    expect(elements[2].textContent.trim()).toBe('Item 3 (2)');
  });

  it('should update when count changes', () => {
    fixture.detectChanges();
    expect(fixture.nativeElement.querySelectorAll('.repeat-item').length).toBe(3);

    component.count = 5;
    fixture.detectChanges();
    expect(fixture.nativeElement.querySelectorAll('.repeat-item').length).toBe(5);

    component.count = 1;
    fixture.detectChanges();
    expect(fixture.nativeElement.querySelectorAll('.repeat-item').length).toBe(1);
  });
});
```

## Wisdom of the Directive Ancients

### Directive Design Mystical Principles

```typescript
// ✅ Good: Single Responsibility
@Directive({
  selector: '[appClickOutside]'
})
export class ClickOutsideDirective {
  @Output() clickOutside = new EventEmitter<Event>();

  @HostListener('document:click', ['$event'])
  onDocumentClick(event: Event): void {
    if (!this.el.nativeElement.contains(event.target)) {
      this.clickOutside.emit(event);
    }
  }

  constructor(private el: ElementRef) {}
}

// ✅ Good: Configurable and Flexible
@Directive({
  selector: '[appAutoFocus]'
})
export class AutoFocusDirective implements AfterViewInit {
  @Input() autoFocusDelay = 0;
  @Input() autoFocusCondition = true;

  constructor(private el: ElementRef) {}

  ngAfterViewInit(): void {
    if (this.autoFocusCondition) {
      setTimeout(() => {
        this.el.nativeElement.focus();
      }, this.autoFocusDelay);
    }
  }
}

// ✅ Good: Proper cleanup
@Directive({
  selector: '[appResizeObserver]'
})
export class ResizeObserverDirective implements OnInit, OnDestroy {
  @Output() resize = new EventEmitter<ResizeObserverEntry>();

  private resizeObserver?: ResizeObserver;

  constructor(private el: ElementRef) {}

  ngOnInit(): void {
    if ('ResizeObserver' in window) {
      this.resizeObserver = new ResizeObserver(entries => {
        this.resize.emit(entries[0]);
      });
      this.resizeObserver.observe(this.el.nativeElement);
    }
  }

  ngOnDestroy(): void {
    if (this.resizeObserver) {
      this.resizeObserver.disconnect();
    }
  }
}
```

### Performance Optimization

```typescript
// ✅ Use OnPush-friendly patterns
@Directive({
  selector: '[appOptimizedHighlight]'
})
export class OptimizedHighlightDirective {
  private _color = 'yellow';

  @Input()
  set appOptimizedHighlight(color: string) {
    if (color !== this._color) {
      this._color = color;
      this.updateHighlight();
    }
  }

  get appOptimizedHighlight(): string {
    return this._color;
  }

  private updateHighlight(): void {
    this.renderer.setStyle(this.el.nativeElement, 'background-color', this._color);
  }

  constructor(
    private el: ElementRef,
    private renderer: Renderer2
  ) {}
}

// ✅ Debounce expensive operations
@Directive({
  selector: '[appDebouncedInput]'
})
export class DebouncedInputDirective implements OnInit, OnDestroy {
  @Input() debounceTime = 300;
  @Output() debouncedInput = new EventEmitter<string>();

  private inputSubject = new Subject<string>();
  private destroy$ = new Subject<void>();

  constructor(private el: ElementRef) {}

  ngOnInit(): void {
    this.inputSubject.pipe(
      debounceTime(this.debounceTime),
      distinctUntilChanged(),
      takeUntil(this.destroy$)
    ).subscribe(value => {
      this.debouncedInput.emit(value);
    });
  }

  @HostListener('input', ['$event.target.value'])
  onInput(value: string): void {
    this.inputSubject.next(value);
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

## Common Curses & Their Remedies

### Curse 1: Memory Drain Leaks
Not cleaning up event listeners and mystical subscriptions.

**Counter-Spell:**
```typescript
// ❌ Bad: No cleanup
@Directive({})
export class BadDirective implements OnInit {
  ngOnInit(): void {
    document.addEventListener('scroll', this.onScroll);

    this.dataService.getData().subscribe(data => {
      // This subscription will never be cleaned up
    });
  }

  onScroll = () => {
    // Handle scroll
  }
}

// ✅ Good: Proper cleanup
@Directive({})
export class GoodDirective implements OnInit, OnDestroy {
  private destroy$ = new Subject<void>();

  ngOnInit(): void {
    fromEvent(document, 'scroll')
      .pipe(takeUntil(this.destroy$))
      .subscribe(this.onScroll);

    this.dataService.getData()
      .pipe(takeUntil(this.destroy$))
      .subscribe(data => {
        // Subscription will be cleaned up
      });
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }

  private onScroll = () => {
    // Handle scroll
  }
}
```

### Issue 2: Direct DOM Manipulation
Manipulating DOM directly instead of using Renderer2.

**Solution:**
```typescript
// ❌ Bad: Direct DOM manipulation
@Directive({})
export class BadDirective {
  constructor(private el: ElementRef) {}

  highlight(): void {
    this.el.nativeElement.style.backgroundColor = 'yellow'; // Not SSR-safe
    this.el.nativeElement.classList.add('highlighted'); // Not SSR-safe
  }
}

// ✅ Good: Use Renderer2
@Directive({})
export class GoodDirective {
  constructor(
    private el: ElementRef,
    private renderer: Renderer2
  ) {}

  highlight(): void {
    this.renderer.setStyle(this.el.nativeElement, 'background-color', 'yellow');
    this.renderer.addClass(this.el.nativeElement, 'highlighted');
  }
}
```

### Issue 3: Not Handling Edge Cases
Failing to handle null/undefined values and edge cases.

**Solution:**
```typescript
// ❌ Bad: No validation
@Directive({})
export class BadDirective {
  @Input() set value(val: any) {
    this.el.nativeElement.textContent = val.toString(); // Can throw error
  }
}

// ✅ Good: Proper validation
@Directive({})
export class GoodDirective {
  @Input() set value(val: any) {
    if (val != null && this.el?.nativeElement) {
      this.renderer.setProperty(
        this.el.nativeElement,
        'textContent',
        String(val)
      );
    }
  }

  constructor(
    private el: ElementRef,
    private renderer: Renderer2
  ) {}
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://angular.io/guide/attribute-directives" %}

{% embed url="https://angular.io/guide/structural-directives" %}

{% embed url="https://angular.io/api/core/Directive" %}

{% embed url="https://angular.io/guide/testing-attribute-directives" %}
```
```
```
