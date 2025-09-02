---
description: >-
  Angular Components are the fundamental building blocks of Angular applications. 
  Learn about component lifecycle, data binding, templates, and communication patterns.
---

# Angular Components

## Introduction

Components are the fundamental building blocks of Angular applications. A component controls a patch of screen called a view through its associated template. Components define views, which are sets of screen elements that Angular can choose among and modify according to your program logic and data.

## Component Basics

### Component Structure

Every Angular component consists of:
- **TypeScript Class**: Contains the component logic and data
- **HTML Template**: Defines the component's view
- **CSS Styles**: Defines the component's appearance
- **Metadata**: Provided by the `@Component` decorator

### Creating a Component

```bash
# Generate a new component
ng generate component user-profile

# Short form
ng g c user-profile

# Generate with options
ng g c user-profile --skip-tests --inline-style --inline-template
```

### Basic Component Example

```typescript
// user-profile.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-user-profile',
  templateUrl: './user-profile.component.html',
  styleUrls: ['./user-profile.component.css']
})
export class UserProfileComponent {
  // Component properties
  user = {
    name: 'John Doe',
    email: 'john@example.com',
    avatar: 'assets/avatars/john.jpg',
    isActive: true
  };

  // Component methods
  toggleStatus() {
    this.user.isActive = !this.user.isActive;
  }

  updateProfile(name: string, email: string) {
    this.user.name = name;
    this.user.email = email;
  }
}
```

```html
<!-- user-profile.component.html -->
<div class="user-profile" [class.active]="user.isActive">
  <img [src]="user.avatar" [alt]="user.name" class="avatar">
  
  <div class="user-info">
    <h3>{{ user.name }}</h3>
    <p>{{ user.email }}</p>
    <span class="status" [class.online]="user.isActive">
      {{ user.isActive ? 'Online' : 'Offline' }}
    </span>
  </div>
  
  <button (click)="toggleStatus()" class="toggle-btn">
    {{ user.isActive ? 'Go Offline' : 'Go Online' }}
  </button>
</div>
```

```css
/* user-profile.component.css */
.user-profile {
  display: flex;
  align-items: center;
  padding: 16px;
  border: 1px solid #ddd;
  border-radius: 8px;
  margin: 8px 0;
  transition: all 0.3s ease;
}

.user-profile.active {
  border-color: #4caf50;
  background-color: #f9fff9;
}

.avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  margin-right: 16px;
}

.user-info {
  flex: 1;
}

.user-info h3 {
  margin: 0 0 4px 0;
  color: #333;
}

.user-info p {
  margin: 0 0 8px 0;
  color: #666;
}

.status {
  padding: 4px 8px;
  border-radius: 12px;
  font-size: 12px;
  background-color: #f44336;
  color: white;
}

.status.online {
  background-color: #4caf50;
}

.toggle-btn {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  background-color: #2196f3;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.toggle-btn:hover {
  background-color: #1976d2;
}
```

## Component Metadata

The `@Component` decorator provides metadata that tells Angular how to process the component:

```typescript
@Component({
  selector: 'app-user-profile',           // CSS selector for the component
  templateUrl: './user-profile.component.html',  // Path to template file
  styleUrls: ['./user-profile.component.css'],   // Array of style files
  
  // Alternative inline options
  template: '<h1>{{ title }}</h1>',      // Inline template
  styles: ['h1 { color: blue; }'],       // Inline styles
  
  // Additional options
  changeDetection: ChangeDetectionStrategy.OnPush,
  encapsulation: ViewEncapsulation.Emulated,
  providers: [UserService],
  animations: [slideInAnimation]
})
export class UserProfileComponent { }
```

### Selector Types

```typescript
// Element selector (most common)
selector: 'app-user-profile'
// Usage: <app-user-profile></app-user-profile>

// Attribute selector
selector: '[app-user-profile]'
// Usage: <div app-user-profile></div>

// Class selector
selector: '.app-user-profile'
// Usage: <div class="app-user-profile"></div>
```

## Data Binding

Angular provides several types of data binding to coordinate communication between components and templates:

### Interpolation (One-way: Component → Template)

```typescript
export class DataBindingComponent {
  title = 'Angular Data Binding';
  user = { name: 'John', age: 30 };
  items = ['Apple', 'Banana', 'Orange'];
  
  getCurrentTime() {
    return new Date().toLocaleTimeString();
  }
}
```

```html
<!-- String interpolation -->
<h1>{{ title }}</h1>
<p>User: {{ user.name }}, Age: {{ user.age }}</p>
<p>Items count: {{ items.length }}</p>
<p>Current time: {{ getCurrentTime() }}</p>

<!-- Expressions -->
<p>Next year: {{ user.age + 1 }}</p>
<p>Uppercase name: {{ user.name.toUpperCase() }}</p>
<p>First item: {{ items[0] }}</p>
```

### Property Binding (One-way: Component → Template)

```typescript
export class PropertyBindingComponent {
  imageUrl = 'assets/logo.png';
  isDisabled = false;
  userClass = 'highlight';
  buttonId = 'submit-btn';
  maxLength = 50;
}
```

```html
<!-- Element properties -->
<img [src]="imageUrl" [alt]="title">
<button [disabled]="isDisabled" [id]="buttonId">Submit</button>
<input [maxlength]="maxLength" [value]="title">

<!-- CSS classes and styles -->
<div [class]="userClass">Styled div</div>
<div [class.active]="isActive">Conditional class</div>
<div [style.color]="textColor">Colored text</div>
<div [style.font-size.px]="fontSize">Sized text</div>

<!-- Multiple classes -->
<div [ngClass]="{
  'active': isActive,
  'disabled': isDisabled,
  'highlight': shouldHighlight
}">Multiple classes</div>

<!-- Multiple styles -->
<div [ngStyle]="{
  'color': textColor,
  'font-size': fontSize + 'px',
  'background-color': bgColor
}">Multiple styles</div>
```

### Event Binding (One-way: Template → Component)

```typescript
export class EventBindingComponent {
  message = '';
  clickCount = 0;
  
  onClick() {
    this.clickCount++;
    this.message = `Button clicked ${this.clickCount} times`;
  }
  
  onInput(event: Event) {
    const target = event.target as HTMLInputElement;
    this.message = target.value;
  }
  
  onKeyUp(event: KeyboardEvent) {
    if (event.key === 'Enter') {
      this.message = 'Enter key pressed!';
    }
  }
  
  onSubmit(event: Event) {
    event.preventDefault();
    console.log('Form submitted');
  }
}
```

```html
<!-- Basic event binding -->
<button (click)="onClick()">Click me</button>
<input (input)="onInput($event)" placeholder="Type something">
<input (keyup)="onKeyUp($event)" placeholder="Press Enter">

<!-- Form events -->
<form (submit)="onSubmit($event)">
  <input type="text" name="username">
  <button type="submit">Submit</button>
</form>

<!-- Mouse events -->
<div (mouseenter)="onMouseEnter()" 
     (mouseleave)="onMouseLeave()"
     (dblclick)="onDoubleClick()">
  Hover and double-click me
</div>

<!-- Custom events -->
<app-child-component (customEvent)="onCustomEvent($event)">
</app-child-component>

<p>{{ message }}</p>
```

### Two-way Binding

```typescript
export class TwoWayBindingComponent {
  name = 'Angular';
  email = '';
  isSubscribed = false;
  selectedColor = 'blue';
  colors = ['red', 'green', 'blue', 'yellow'];
}
```

```html
<!-- Requires FormsModule -->
<input [(ngModel)]="name" placeholder="Enter name">
<input [(ngModel)]="email" type="email" placeholder="Enter email">
<input [(ngModel)]="isSubscribed" type="checkbox"> Subscribe to newsletter

<select [(ngModel)]="selectedColor">
  <option *ngFor="let color of colors" [value]="color">
    {{ color }}
  </option>
</select>

<div>
  <p>Name: {{ name }}</p>
  <p>Email: {{ email }}</p>
  <p>Subscribed: {{ isSubscribed }}</p>
  <p>Selected Color: {{ selectedColor }}</p>
</div>
```

## Template Syntax

### Template Reference Variables

```html
<!-- Template reference variables -->
<input #nameInput type="text" placeholder="Enter name">
<button (click)="greet(nameInput.value)">Greet</button>

<!-- Reference to component -->
<app-child-component #childComponent></app-child-component>
<button (click)="childComponent.doSomething()">Call child method</button>

<!-- Reference in template -->
<input #phone placeholder="phone number">
<button (click)="callPhone(phone.value)">Call</button>
<button (click)="phone.focus()">Focus input</button>
```

### Safe Navigation Operator

```typescript
export class SafeNavigationComponent {
  user: any = null;
  
  loadUser() {
    // Simulate async data loading
    setTimeout(() => {
      this.user = {
        name: 'John Doe',
        profile: {
          avatar: 'assets/john.jpg',
          bio: 'Software Developer'
        }
      };
    }, 2000);
  }
}
```

```html
<!-- Safe navigation operator (?.) -->
<div *ngIf="user">
  <h3>{{ user?.name }}</h3>
  <img [src]="user?.profile?.avatar" [alt]="user?.name">
  <p>{{ user?.profile?.bio }}</p>
</div>

<!-- Without safe navigation (would cause errors) -->
<!-- <h3>{{ user.name }}</h3> This would throw error if user is null -->

<!-- Alternative with ngIf -->
<div *ngIf="user && user.profile">
  <img [src]="user.profile.avatar" [alt]="user.name">
</div>

## Component Lifecycle

Angular components have a well-defined lifecycle managed by Angular. Understanding these lifecycle hooks is crucial for building robust applications.

### Lifecycle Hooks Overview

```typescript
import {
  Component, OnInit, OnDestroy, OnChanges,
  AfterViewInit, AfterViewChecked, AfterContentInit,
  AfterContentChecked, DoCheck, SimpleChanges
} from '@angular/core';

@Component({
  selector: 'app-lifecycle-demo',
  template: `
    <h3>Lifecycle Demo Component</h3>
    <p>Check console for lifecycle events</p>
    <input [(ngModel)]="inputValue" placeholder="Type something">
    <p>Input value: {{ inputValue }}</p>
  `
})
export class LifecycleDemoComponent implements
  OnInit, OnDestroy, OnChanges, AfterViewInit,
  AfterViewChecked, AfterContentInit, AfterContentChecked, DoCheck {

  inputValue = '';

  constructor() {
    console.log('1. Constructor called');
  }

  ngOnChanges(changes: SimpleChanges) {
    console.log('2. ngOnChanges called', changes);
  }

  ngOnInit() {
    console.log('3. ngOnInit called');
    // Initialize component
    // Make HTTP calls
    // Set up subscriptions
  }

  ngDoCheck() {
    console.log('4. ngDoCheck called');
    // Custom change detection logic
  }

  ngAfterContentInit() {
    console.log('5. ngAfterContentInit called');
    // Content projection is initialized
  }

  ngAfterContentChecked() {
    console.log('6. ngAfterContentChecked called');
    // Content projection is checked
  }

  ngAfterViewInit() {
    console.log('7. ngAfterViewInit called');
    // View is initialized
    // Access ViewChild elements
  }

  ngAfterViewChecked() {
    console.log('8. ngAfterViewChecked called');
    // View is checked
  }

  ngOnDestroy() {
    console.log('9. ngOnDestroy called');
    // Cleanup subscriptions
    // Remove event listeners
    // Clear timers
  }
}
```

### Practical Lifecycle Usage

```typescript
import { Component, OnInit, OnDestroy, ViewChild, ElementRef } from '@angular/core';
import { Subscription, interval } from 'rxjs';
import { UserService } from './user.service';

@Component({
  selector: 'app-user-dashboard',
  template: `
    <div class="dashboard">
      <h2>User Dashboard</h2>
      <div #chartContainer class="chart-container"></div>
      <p>Timer: {{ timer }}</p>
      <div *ngIf="user">
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
      </div>
    </div>
  `
})
export class UserDashboardComponent implements OnInit, OnDestroy {
  @ViewChild('chartContainer', { static: true }) chartContainer!: ElementRef;

  user: any;
  timer = 0;
  private timerSubscription!: Subscription;
  private userSubscription!: Subscription;

  constructor(private userService: UserService) {}

  ngOnInit() {
    // Initialize data
    this.loadUser();
    this.startTimer();
  }

  ngAfterViewInit() {
    // Initialize chart after view is ready
    this.initializeChart();
  }

  ngOnDestroy() {
    // Clean up subscriptions to prevent memory leaks
    if (this.timerSubscription) {
      this.timerSubscription.unsubscribe();
    }
    if (this.userSubscription) {
      this.userSubscription.unsubscribe();
    }
  }

  private loadUser() {
    this.userSubscription = this.userService.getCurrentUser()
      .subscribe(user => this.user = user);
  }

  private startTimer() {
    this.timerSubscription = interval(1000)
      .subscribe(count => this.timer = count);
  }

  private initializeChart() {
    // Initialize chart using chartContainer.nativeElement
    console.log('Chart container:', this.chartContainer.nativeElement);
  }
}
```

## Component Communication

### Parent to Child Communication

#### Using Input Properties

```typescript
// Child Component
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-user-card',
  template: `
    <div class="user-card">
      <h3>{{ user.name }}</h3>
      <p>{{ user.email }}</p>
      <p>Role: {{ role }}</p>
      <div *ngIf="showActions">
        <button (click)="editUser()">Edit</button>
        <button (click)="deleteUser()">Delete</button>
      </div>
    </div>
  `
})
export class UserCardComponent {
  @Input() user!: { name: string; email: string; id: number };
  @Input() role = 'User';
  @Input() showActions = false;

  editUser() {
    console.log('Edit user:', this.user.id);
  }

  deleteUser() {
    console.log('Delete user:', this.user.id);
  }
}

// Parent Component
@Component({
  selector: 'app-user-list',
  template: `
    <div class="user-list">
      <app-user-card
        *ngFor="let user of users"
        [user]="user"
        [role]="user.role"
        [showActions]="isAdmin">
      </app-user-card>
    </div>
  `
})
export class UserListComponent {
  isAdmin = true;
  users = [
    { id: 1, name: 'John Doe', email: 'john@example.com', role: 'Admin' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com', role: 'User' }
  ];
}
```

#### Input Validation and Transformation

```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-progress-bar',
  template: `
    <div class="progress-bar">
      <div class="progress-fill" [style.width.%]="progress">
        {{ progress }}%
      </div>
    </div>
  `
})
export class ProgressBarComponent {
  private _progress = 0;

  @Input()
  get progress(): number {
    return this._progress;
  }

  set progress(value: number) {
    // Validate and transform input
    this._progress = Math.max(0, Math.min(100, value));
  }

  @Input() color = 'blue';
  @Input() height = '20px';
}
```

### Child to Parent Communication

#### Using Output Properties and EventEmitter

```typescript
// Child Component
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-counter',
  template: `
    <div class="counter">
      <button (click)="decrement()">-</button>
      <span class="count">{{ count }}</span>
      <button (click)="increment()">+</button>
      <button (click)="reset()">Reset</button>
    </div>
  `
})
export class CounterComponent {
  @Input() count = 0;
  @Input() step = 1;

  @Output() countChange = new EventEmitter<number>();
  @Output() reset = new EventEmitter<void>();
  @Output() milestone = new EventEmitter<{ count: number; message: string }>();

  increment() {
    this.count += this.step;
    this.countChange.emit(this.count);
    this.checkMilestone();
  }

  decrement() {
    this.count -= this.step;
    this.countChange.emit(this.count);
  }

  onReset() {
    this.count = 0;
    this.countChange.emit(this.count);
    this.reset.emit();
  }

  private checkMilestone() {
    if (this.count % 10 === 0 && this.count > 0) {
      this.milestone.emit({
        count: this.count,
        message: `Reached milestone: ${this.count}!`
      });
    }
  }
}

// Parent Component
@Component({
  selector: 'app-counter-parent',
  template: `
    <div class="counter-parent">
      <h2>Counter Demo</h2>
      <app-counter
        [count]="currentCount"
        [step]="stepSize"
        (countChange)="onCountChange($event)"
        (reset)="onReset()"
        (milestone)="onMilestone($event)">
      </app-counter>

      <div class="controls">
        <label>
          Step size:
          <input type="number" [(ngModel)]="stepSize" min="1">
        </label>
      </div>

      <div *ngIf="milestoneMessage" class="milestone">
        {{ milestoneMessage }}
      </div>
    </div>
  `
})
export class CounterParentComponent {
  currentCount = 0;
  stepSize = 1;
  milestoneMessage = '';

  onCountChange(newCount: number) {
    this.currentCount = newCount;
    console.log('Count changed to:', newCount);
  }

  onReset() {
    console.log('Counter was reset');
    this.milestoneMessage = '';
  }

  onMilestone(event: { count: number; message: string }) {
    this.milestoneMessage = event.message;
    console.log('Milestone reached:', event);

    // Clear message after 3 seconds
    setTimeout(() => {
      this.milestoneMessage = '';
    }, 3000);
  }
}

### ViewChild and ViewChildren

#### Accessing Child Components and Elements

```typescript
import { Component, ViewChild, ViewChildren, QueryList, ElementRef, AfterViewInit } from '@angular/core';
import { CounterComponent } from './counter.component';

@Component({
  selector: 'app-view-child-demo',
  template: `
    <div class="demo">
      <h2>ViewChild Demo</h2>

      <!-- Reference to element -->
      <input #nameInput type="text" placeholder="Enter name">

      <!-- Reference to component -->
      <app-counter #counter [count]="5"></app-counter>

      <!-- Multiple components -->
      <app-counter *ngFor="let item of items; let i = index"
                   [count]="item.count"
                   #counters>
      </app-counter>

      <div class="controls">
        <button (click)="focusInput()">Focus Input</button>
        <button (click)="incrementCounter()">Increment Counter</button>
        <button (click)="incrementAllCounters()">Increment All</button>
        <button (click)="getTotalCount()">Get Total Count</button>
      </div>

      <p>Total count: {{ totalCount }}</p>
    </div>
  `
})
export class ViewChildDemoComponent implements AfterViewInit {
  // ViewChild for element reference
  @ViewChild('nameInput') nameInput!: ElementRef<HTMLInputElement>;

  // ViewChild for component reference
  @ViewChild('counter') counter!: CounterComponent;

  // ViewChildren for multiple components
  @ViewChildren('counters') counters!: QueryList<CounterComponent>;

  items = [
    { count: 0 },
    { count: 5 },
    { count: 10 }
  ];

  totalCount = 0;

  ngAfterViewInit() {
    // ViewChild elements are available here
    console.log('Name input element:', this.nameInput.nativeElement);
    console.log('Counter component:', this.counter);
    console.log('All counters:', this.counters.toArray());

    // Subscribe to changes in ViewChildren
    this.counters.changes.subscribe(() => {
      console.log('Counters changed:', this.counters.length);
    });
  }

  focusInput() {
    this.nameInput.nativeElement.focus();
  }

  incrementCounter() {
    this.counter.increment();
  }

  incrementAllCounters() {
    this.counters.forEach(counter => counter.increment());
  }

  getTotalCount() {
    this.totalCount = this.counters.reduce((total, counter) => {
      return total + counter.count;
    }, 0);
  }
}
```

#### ContentChild and ContentChildren

```typescript
// Card component that projects content
@Component({
  selector: 'app-card',
  template: `
    <div class="card">
      <div class="card-header" *ngIf="hasHeader">
        <ng-content select="[slot=header]"></ng-content>
      </div>

      <div class="card-body">
        <ng-content></ng-content>
      </div>

      <div class="card-footer" *ngIf="hasFooter">
        <ng-content select="[slot=footer]"></ng-content>
      </div>
    </div>
  `
})
export class CardComponent implements AfterContentInit {
  @ContentChild('header') header!: ElementRef;
  @ContentChildren('button') buttons!: QueryList<ElementRef>;

  hasHeader = false;
  hasFooter = false;

  ngAfterContentInit() {
    this.hasHeader = !!this.header;
    this.hasFooter = this.buttons.length > 0;

    console.log('Projected buttons:', this.buttons.length);
  }
}

// Usage of card component
@Component({
  template: `
    <app-card>
      <h3 slot="header" #header>Card Title</h3>
      <p>This is the card content.</p>
      <button #button>Action 1</button>
      <button #button>Action 2</button>
    </app-card>
  `
})
export class CardUsageComponent { }
```

## Content Projection (ng-content)

### Basic Content Projection

```typescript
// Alert component
@Component({
  selector: 'app-alert',
  template: `
    <div class="alert" [class]="'alert-' + type">
      <div class="alert-icon">
        <i [class]="getIconClass()"></i>
      </div>
      <div class="alert-content">
        <ng-content></ng-content>
      </div>
      <button *ngIf="dismissible" class="alert-close" (click)="close()">
        ×
      </button>
    </div>
  `,
  styles: [`
    .alert {
      display: flex;
      align-items: center;
      padding: 12px 16px;
      border-radius: 4px;
      margin: 8px 0;
    }
    .alert-success { background-color: #d4edda; color: #155724; }
    .alert-warning { background-color: #fff3cd; color: #856404; }
    .alert-error { background-color: #f8d7da; color: #721c24; }
    .alert-icon { margin-right: 8px; }
    .alert-content { flex: 1; }
    .alert-close {
      background: none;
      border: none;
      font-size: 20px;
      cursor: pointer;
    }
  `]
})
export class AlertComponent {
  @Input() type: 'success' | 'warning' | 'error' = 'success';
  @Input() dismissible = false;
  @Output() dismissed = new EventEmitter<void>();

  getIconClass(): string {
    const icons = {
      success: 'fas fa-check-circle',
      warning: 'fas fa-exclamation-triangle',
      error: 'fas fa-times-circle'
    };
    return icons[this.type];
  }

  close() {
    this.dismissed.emit();
  }
}

// Usage
@Component({
  template: `
    <app-alert type="success" [dismissible]="true" (dismissed)="onAlertDismissed()">
      <strong>Success!</strong> Your changes have been saved.
    </app-alert>

    <app-alert type="warning">
      <strong>Warning!</strong> Please review your input.
    </app-alert>

    <app-alert type="error">
      <strong>Error!</strong> Something went wrong.
      <a href="/help">Get help</a>
    </app-alert>
  `
})
export class AlertUsageComponent {
  onAlertDismissed() {
    console.log('Alert dismissed');
  }
}
```

### Multi-slot Content Projection

```typescript
// Modal component with multiple projection slots
@Component({
  selector: 'app-modal',
  template: `
    <div class="modal-overlay" *ngIf="isOpen" (click)="onOverlayClick()">
      <div class="modal-dialog" (click)="$event.stopPropagation()">
        <div class="modal-header">
          <ng-content select="[slot=header]"></ng-content>
          <button class="modal-close" (click)="close()">×</button>
        </div>

        <div class="modal-body">
          <ng-content></ng-content>
        </div>

        <div class="modal-footer" *ngIf="hasFooter">
          <ng-content select="[slot=footer]"></ng-content>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .modal-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0, 0, 0, 0.5);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 1000;
    }
    .modal-dialog {
      background: white;
      border-radius: 8px;
      max-width: 500px;
      width: 90%;
      max-height: 90%;
      overflow: hidden;
    }
    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 16px;
      border-bottom: 1px solid #ddd;
    }
    .modal-body {
      padding: 16px;
    }
    .modal-footer {
      padding: 16px;
      border-top: 1px solid #ddd;
      text-align: right;
    }
    .modal-close {
      background: none;
      border: none;
      font-size: 24px;
      cursor: pointer;
    }
  `]
})
export class ModalComponent implements AfterContentInit {
  @Input() isOpen = false;
  @Input() closeOnOverlayClick = true;
  @Output() closed = new EventEmitter<void>();

  @ContentChildren('[slot=footer]') footerContent!: QueryList<any>;

  hasFooter = false;

  ngAfterContentInit() {
    this.hasFooter = this.footerContent.length > 0;
  }

  close() {
    this.isOpen = false;
    this.closed.emit();
  }

  onOverlayClick() {
    if (this.closeOnOverlayClick) {
      this.close();
    }
  }
}

// Usage
@Component({
  template: `
    <button (click)="openModal()">Open Modal</button>

    <app-modal [isOpen]="isModalOpen" (closed)="onModalClosed()">
      <h2 slot="header">Confirm Action</h2>

      <p>Are you sure you want to delete this item?</p>
      <p>This action cannot be undone.</p>

      <div slot="footer">
        <button (click)="closeModal()">Cancel</button>
        <button (click)="confirmDelete()" class="btn-danger">Delete</button>
      </div>
    </app-modal>
  `
})
export class ModalUsageComponent {
  isModalOpen = false;

  openModal() {
    this.isModalOpen = true;
  }

  closeModal() {
    this.isModalOpen = false;
  }

  onModalClosed() {
    console.log('Modal closed');
  }

  confirmDelete() {
    console.log('Item deleted');
    this.closeModal();
  }
}

## Change Detection

### Understanding Change Detection

Angular's change detection mechanism automatically updates the DOM when component data changes. Understanding how it works helps you optimize your applications.

```typescript
import { Component, ChangeDetectionStrategy, ChangeDetectorRef } from '@angular/core';

@Component({
  selector: 'app-change-detection-demo',
  template: `
    <div class="demo">
      <h3>Change Detection Demo</h3>
      <p>Counter: {{ counter }}</p>
      <p>Last updated: {{ lastUpdated }}</p>

      <button (click)="increment()">Increment</button>
      <button (click)="updateTime()">Update Time</button>
      <button (click)="triggerChangeDetection()">Manual Detection</button>

      <app-child-component [data]="childData"></app-child-component>
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChangeDetectionDemoComponent {
  counter = 0;
  lastUpdated = new Date().toLocaleTimeString();
  childData = { value: 0 };

  constructor(private cdr: ChangeDetectorRef) {}

  increment() {
    this.counter++;
    this.lastUpdated = new Date().toLocaleTimeString();
    // With OnPush, need to manually trigger change detection
    this.cdr.markForCheck();
  }

  updateTime() {
    this.lastUpdated = new Date().toLocaleTimeString();
    this.cdr.detectChanges(); // Force immediate change detection
  }

  triggerChangeDetection() {
    // Update child data immutably for OnPush to detect changes
    this.childData = { value: this.childData.value + 1 };
    this.cdr.markForCheck();
  }
}
```

### OnPush Change Detection Strategy

```typescript
import { Component, Input, ChangeDetectionStrategy } from '@angular/core';

@Component({
  selector: 'app-optimized-component',
  template: `
    <div class="optimized">
      <h4>Optimized Component</h4>
      <p>User: {{ user.name }}</p>
      <p>Count: {{ count }}</p>
      <p>Items: {{ items.length }}</p>

      <!-- This will only update when inputs change -->
      <div *ngFor="let item of items">
        {{ item.name }}
      </div>
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent {
  @Input() user!: { name: string; email: string };
  @Input() count!: number;
  @Input() items!: { name: string; id: number }[];

  // Component only updates when:
  // 1. Input properties change (reference comparison)
  // 2. Event is triggered from this component
  // 3. Observable emits (with async pipe)
  // 4. Manual change detection is triggered
}

// Parent component using immutable updates
@Component({
  selector: 'app-parent',
  template: `
    <app-optimized-component
      [user]="user"
      [count]="count"
      [items]="items">
    </app-optimized-component>

    <button (click)="updateUser()">Update User</button>
    <button (click)="addItem()">Add Item</button>
  `
})
export class ParentComponent {
  user = { name: 'John', email: 'john@example.com' };
  count = 0;
  items = [{ name: 'Item 1', id: 1 }];

  updateUser() {
    // Create new object reference for OnPush to detect change
    this.user = { ...this.user, name: 'Jane' };
  }

  addItem() {
    // Create new array reference
    this.items = [...this.items, { name: `Item ${this.items.length + 1}`, id: this.items.length + 1 }];
  }
}
```

### Async Pipe and Change Detection

```typescript
import { Component } from '@angular/core';
import { Observable, interval, map } from 'rxjs';
import { HttpClient } from '@angular/common/http';

@Component({
  selector: 'app-async-demo',
  template: `
    <div class="async-demo">
      <h3>Async Pipe Demo</h3>

      <!-- Async pipe automatically handles subscriptions and change detection -->
      <p>Timer: {{ timer$ | async }}</p>
      <p>Current time: {{ currentTime$ | async | date:'medium' }}</p>

      <!-- Loading states with async pipe -->
      <div *ngIf="users$ | async as users; else loading">
        <div *ngFor="let user of users">
          {{ user.name }} - {{ user.email }}
        </div>
      </div>

      <ng-template #loading>
        <p>Loading users...</p>
      </ng-template>

      <!-- Multiple async pipes (not recommended - use single subscription) -->
      <div *ngIf="(users$ | async)?.length">
        Found {{ (users$ | async)?.length }} users
      </div>

      <!-- Better approach - single subscription -->
      <div *ngIf="users$ | async as users">
        Found {{ users.length }} users
      </div>
    </div>
  `
})
export class AsyncDemoComponent {
  timer$ = interval(1000);
  currentTime$ = interval(1000).pipe(
    map(() => new Date())
  );

  users$: Observable<any[]>;

  constructor(private http: HttpClient) {
    this.users$ = this.http.get<any[]>('/api/users');
  }
}
```

## Advanced Component Patterns

### Dynamic Component Loading

```typescript
import {
  Component, ViewChild, ViewContainerRef,
  ComponentFactoryResolver, ComponentRef, Type
} from '@angular/core';

// Components to load dynamically
@Component({
  template: '<p>This is Component A</p>'
})
export class ComponentA { }

@Component({
  template: '<p>This is Component B with data: {{ data }}</p>'
})
export class ComponentB {
  data = 'Hello from B';
}

@Component({
  selector: 'app-dynamic-loader',
  template: `
    <div class="dynamic-loader">
      <h3>Dynamic Component Loader</h3>

      <button (click)="loadComponent('A')">Load Component A</button>
      <button (click)="loadComponent('B')">Load Component B</button>
      <button (click)="clearComponents()">Clear</button>

      <!-- Container for dynamic components -->
      <div #dynamicContainer></div>
    </div>
  `
})
export class DynamicLoaderComponent {
  @ViewChild('dynamicContainer', { read: ViewContainerRef })
  container!: ViewContainerRef;

  private componentRefs: ComponentRef<any>[] = [];

  constructor(private resolver: ComponentFactoryResolver) {}

  loadComponent(type: 'A' | 'B') {
    const componentMap: { [key: string]: Type<any> } = {
      'A': ComponentA,
      'B': ComponentB
    };

    const component = componentMap[type];
    const factory = this.resolver.resolveComponentFactory(component);
    const componentRef = this.container.createComponent(factory);

    this.componentRefs.push(componentRef);
  }

  clearComponents() {
    this.componentRefs.forEach(ref => ref.destroy());
    this.componentRefs = [];
    this.container.clear();
  }
}
```

### Component Inheritance

```typescript
// Base component with common functionality
export abstract class BaseFormComponent {
  isLoading = false;
  errors: string[] = [];

  protected showError(message: string) {
    this.errors.push(message);
  }

  protected clearErrors() {
    this.errors = [];
  }

  protected setLoading(loading: boolean) {
    this.isLoading = loading;
  }

  abstract onSubmit(): void;
}

// Concrete implementation
@Component({
  selector: 'app-user-form',
  template: `
    <form (ngSubmit)="onSubmit()" class="user-form">
      <div class="errors" *ngIf="errors.length">
        <div *ngFor="let error of errors" class="error">
          {{ error }}
        </div>
      </div>

      <input [(ngModel)]="user.name" placeholder="Name" required>
      <input [(ngModel)]="user.email" type="email" placeholder="Email" required>

      <button type="submit" [disabled]="isLoading">
        {{ isLoading ? 'Saving...' : 'Save User' }}
      </button>
    </form>
  `
})
export class UserFormComponent extends BaseFormComponent {
  user = { name: '', email: '' };

  onSubmit() {
    this.clearErrors();

    if (!this.user.name || !this.user.email) {
      this.showError('Name and email are required');
      return;
    }

    this.setLoading(true);

    // Simulate API call
    setTimeout(() => {
      console.log('User saved:', this.user);
      this.setLoading(false);
    }, 2000);
  }
}
```

## Best Practices

### Component Design Principles

```typescript
// ✅ Good: Single Responsibility
@Component({
  selector: 'app-user-avatar',
  template: `
    <img [src]="avatarUrl"
         [alt]="user.name"
         [class.online]="user.isOnline"
         class="avatar">
  `
})
export class UserAvatarComponent {
  @Input() user!: { name: string; avatar: string; isOnline: boolean };

  get avatarUrl(): string {
    return this.user.avatar || 'assets/default-avatar.png';
  }
}

// ✅ Good: Immutable Inputs
@Component({
  selector: 'app-user-list',
  template: `
    <div *ngFor="let user of users; trackBy: trackByUserId">
      <app-user-card [user]="user"></app-user-card>
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserListComponent {
  @Input() users!: User[];

  trackByUserId(index: number, user: User): number {
    return user.id;
  }
}

// ✅ Good: Smart/Dumb Component Pattern
// Smart Component (Container)
@Component({
  selector: 'app-user-management',
  template: `
    <app-user-list
      [users]="users$ | async"
      (userSelected)="onUserSelected($event)"
      (userDeleted)="onUserDeleted($event)">
    </app-user-list>
  `
})
export class UserManagementComponent {
  users$ = this.userService.getUsers();

  constructor(private userService: UserService) {}

  onUserSelected(user: User) {
    // Handle business logic
  }

  onUserDeleted(userId: number) {
    // Handle business logic
  }
}

// Dumb Component (Presentational)
@Component({
  selector: 'app-user-list',
  template: `
    <div *ngFor="let user of users">
      <app-user-card
        [user]="user"
        (selected)="userSelected.emit(user)"
        (deleted)="userDeleted.emit(user.id)">
      </app-user-card>
    </div>
  `,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserListComponent {
  @Input() users!: User[];
  @Output() userSelected = new EventEmitter<User>();
  @Output() userDeleted = new EventEmitter<number>();
}
```

### Performance Optimization

```typescript
// ✅ Use OnPush change detection
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent { }

// ✅ Use trackBy functions
@Component({
  template: `
    <div *ngFor="let item of items; trackBy: trackByFn">
      {{ item.name }}
    </div>
  `
})
export class TrackByComponent {
  trackByFn(index: number, item: any): any {
    return item.id; // Use unique identifier
  }
}

// ✅ Unsubscribe from observables
@Component({})
export class SubscriptionComponent implements OnInit, OnDestroy {
  private destroy$ = new Subject<void>();

  ngOnInit() {
    this.dataService.getData()
      .pipe(takeUntil(this.destroy$))
      .subscribe(data => {
        // Handle data
      });
  }

  ngOnDestroy() {
    this.destroy$.next();
    this.destroy$.complete();
  }
}

// ✅ Use async pipe when possible
@Component({
  template: `
    <div *ngFor="let item of items$ | async">
      {{ item.name }}
    </div>
  `
})
export class AsyncPipeComponent {
  items$ = this.dataService.getItems();
}
```

## Common Pitfalls

### Issue 1: Memory Leaks from Subscriptions
Not unsubscribing from observables can cause memory leaks.

**Solution:**
```typescript
// ❌ Bad: No unsubscription
export class BadComponent implements OnInit {
  ngOnInit() {
    this.dataService.getData().subscribe(data => {
      // This subscription will never be cleaned up
    });
  }
}

// ✅ Good: Proper cleanup
export class GoodComponent implements OnInit, OnDestroy {
  private destroy$ = new Subject<void>();

  ngOnInit() {
    this.dataService.getData()
      .pipe(takeUntil(this.destroy$))
      .subscribe(data => {
        // Subscription will be cleaned up
      });
  }

  ngOnDestroy() {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

### Issue 2: Mutating Input Properties
Directly mutating input properties can cause unexpected behavior.

**Solution:**
```typescript
// ❌ Bad: Mutating input
@Component({})
export class BadComponent {
  @Input() user!: User;

  updateUser() {
    this.user.name = 'New Name'; // Mutates parent's data
  }
}

// ✅ Good: Emit changes
@Component({})
export class GoodComponent {
  @Input() user!: User;
  @Output() userChanged = new EventEmitter<User>();

  updateUser() {
    const updatedUser = { ...this.user, name: 'New Name' };
    this.userChanged.emit(updatedUser);
  }
}
```

### Issue 3: Not Using OnPush with Immutable Data
Missing performance optimizations with OnPush strategy.

**Solution:**
```typescript
// ✅ Use OnPush with immutable updates
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent {
  @Input() items!: Item[];

  // Parent should update like this:
  // this.items = [...this.items, newItem]; // Creates new reference
  // Not: this.items.push(newItem); // Mutates existing reference
}
```

## References

{% embed url="https://angular.io/guide/component-overview" %}

{% embed url="https://angular.io/guide/lifecycle-hooks" %}

{% embed url="https://angular.io/guide/component-interaction" %}

{% embed url="https://angular.io/guide/content-projection" %}
```
```
```
