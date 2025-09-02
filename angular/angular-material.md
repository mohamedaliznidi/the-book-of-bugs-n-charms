---
description: >-
  Angular Material provides a comprehensive UI component library following Material Design principles. 
  Learn about components, theming, and creating beautiful, accessible user interfaces.
---

# Angular Material

## Introduction

Angular Material is a UI component library for Angular developers. Built and maintained by the Angular team, it provides a set of reusable, well-tested, and accessible UI components based on Google's Material Design specification.

## Key Features

- **Material Design**: Components follow Google's Material Design guidelines
- **Accessibility**: Built-in accessibility features and ARIA support
- **Theming**: Comprehensive theming system with custom color palettes
- **Responsive**: Mobile-first responsive design
- **TypeScript**: Full TypeScript support with type definitions
- **CDK**: Component Development Kit for building custom components

## Installation and Setup

### Installing Angular Material

```bash
# Install Angular Material, CDK, and Animations
ng add @angular/material

# Or install manually
npm install @angular/material @angular/cdk @angular/animations
```

### Basic Setup

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';

// Material modules
import { MatToolbarModule } from '@angular/material/toolbar';
import { MatButtonModule } from '@angular/material/button';
import { MatIconModule } from '@angular/material/icon';
import { MatCardModule } from '@angular/material/card';
import { MatInputModule } from '@angular/material/input';
import { MatFormFieldModule } from '@angular/material/form-field';

@NgModule({
  imports: [
    BrowserModule,
    BrowserAnimationsModule,
    MatToolbarModule,
    MatButtonModule,
    MatIconModule,
    MatCardModule,
    MatInputModule,
    MatFormFieldModule
  ],
  // ...
})
export class AppModule { }
```

### Global Styles Setup

```scss
/* styles.scss */
@import '~@angular/material/theming';

// Include the common styles for Angular Material
@include mat-core();

// Define a light theme
$primary: mat-palette($mat-indigo);
$accent: mat-palette($mat-pink, A200, A100, A400);
$warn: mat-palette($mat-red);
$theme: mat-light-theme($primary, $accent, $warn);

// Include theme styles for core and each component used in your app
@include angular-material-theme($theme);

// Custom global styles
html, body { height: 100%; }
body { margin: 0; font-family: Roboto, "Helvetica Neue", sans-serif; }
```

## Core Components

### Buttons and Indicators

```typescript
@Component({
  selector: 'app-buttons-demo',
  template: `
    <div class="demo-container">
      <h2>Buttons</h2>
      
      <div class="button-section">
        <h3>Basic Buttons</h3>
        <div class="button-row">
          <button mat-button>Basic</button>
          <button mat-button color="primary">Primary</button>
          <button mat-button color="accent">Accent</button>
          <button mat-button color="warn">Warn</button>
          <button mat-button disabled>Disabled</button>
        </div>
      </div>

      <div class="button-section">
        <h3>Raised Buttons</h3>
        <div class="button-row">
          <button mat-raised-button>Basic</button>
          <button mat-raised-button color="primary">Primary</button>
          <button mat-raised-button color="accent">Accent</button>
          <button mat-raised-button color="warn">Warn</button>
          <button mat-raised-button disabled>Disabled</button>
        </div>
      </div>

      <div class="button-section">
        <h3>Stroked Buttons</h3>
        <div class="button-row">
          <button mat-stroked-button>Basic</button>
          <button mat-stroked-button color="primary">Primary</button>
          <button mat-stroked-button color="accent">Accent</button>
          <button mat-stroked-button color="warn">Warn</button>
        </div>
      </div>

      <div class="button-section">
        <h3>Flat Buttons</h3>
        <div class="button-row">
          <button mat-flat-button>Basic</button>
          <button mat-flat-button color="primary">Primary</button>
          <button mat-flat-button color="accent">Accent</button>
          <button mat-flat-button color="warn">Warn</button>
        </div>
      </div>

      <div class="button-section">
        <h3>Icon Buttons</h3>
        <div class="button-row">
          <button mat-icon-button>
            <mat-icon>favorite</mat-icon>
          </button>
          <button mat-icon-button color="primary">
            <mat-icon>bookmark</mat-icon>
          </button>
          <button mat-icon-button color="accent">
            <mat-icon>home</mat-icon>
          </button>
          <button mat-icon-button color="warn">
            <mat-icon>delete</mat-icon>
          </button>
        </div>
      </div>

      <div class="button-section">
        <h3>FAB Buttons</h3>
        <div class="button-row">
          <button mat-fab>
            <mat-icon>add</mat-icon>
          </button>
          <button mat-fab color="primary">
            <mat-icon>edit</mat-icon>
          </button>
          <button mat-fab color="accent">
            <mat-icon>navigation</mat-icon>
          </button>
          <button mat-fab color="warn">
            <mat-icon>delete</mat-icon>
          </button>
        </div>
      </div>

      <div class="button-section">
        <h3>Mini FAB Buttons</h3>
        <div class="button-row">
          <button mat-mini-fab>
            <mat-icon>add</mat-icon>
          </button>
          <button mat-mini-fab color="primary">
            <mat-icon>edit</mat-icon>
          </button>
          <button mat-mini-fab color="accent">
            <mat-icon>navigation</mat-icon>
          </button>
        </div>
      </div>

      <h2>Progress Indicators</h2>
      
      <div class="progress-section">
        <h3>Progress Bar</h3>
        <mat-progress-bar mode="determinate" [value]="progressValue"></mat-progress-bar>
        <mat-progress-bar mode="indeterminate"></mat-progress-bar>
        <mat-progress-bar mode="buffer" [value]="progressValue" [bufferValue]="bufferValue"></mat-progress-bar>
        
        <div class="progress-controls">
          <button mat-button (click)="updateProgress()">Update Progress</button>
          <span>Progress: {{ progressValue }}%</span>
        </div>
      </div>

      <div class="progress-section">
        <h3>Progress Spinner</h3>
        <div class="spinner-row">
          <mat-progress-spinner mode="determinate" [value]="progressValue"></mat-progress-spinner>
          <mat-progress-spinner mode="indeterminate"></mat-progress-spinner>
          <mat-progress-spinner mode="indeterminate" diameter="30"></mat-progress-spinner>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo-container {
      padding: 20px;
      max-width: 1200px;
      margin: 0 auto;
    }
    .button-section, .progress-section {
      margin: 24px 0;
    }
    .button-row, .spinner-row {
      display: flex;
      gap: 12px;
      align-items: center;
      flex-wrap: wrap;
      margin: 16px 0;
    }
    .progress-controls {
      display: flex;
      align-items: center;
      gap: 16px;
      margin-top: 16px;
    }
    h2 {
      color: #1976d2;
      border-bottom: 2px solid #1976d2;
      padding-bottom: 8px;
    }
    h3 {
      color: #424242;
      margin: 16px 0 8px 0;
    }
    mat-progress-bar {
      margin: 8px 0;
    }
  `]
})
export class ButtonsDemoComponent {
  progressValue = 40;
  bufferValue = 60;

  updateProgress(): void {
    this.progressValue = Math.floor(Math.random() * 100);
    this.bufferValue = Math.min(this.progressValue + 20, 100);
  }
}
```

### Form Controls

```typescript
@Component({
  selector: 'app-form-controls-demo',
  template: `
    <div class="demo-container">
      <h2>Form Controls</h2>
      
      <div class="form-section">
        <h3>Input Fields</h3>
        <div class="form-row">
          <mat-form-field appearance="fill">
            <mat-label>Fill appearance</mat-label>
            <input matInput placeholder="Placeholder">
            <mat-icon matSuffix>sentiment_very_satisfied</mat-icon>
            <mat-hint>Hint text</mat-hint>
          </mat-form-field>

          <mat-form-field appearance="outline">
            <mat-label>Outline appearance</mat-label>
            <input matInput placeholder="Placeholder">
            <mat-icon matSuffix>sentiment_very_satisfied</mat-icon>
          </mat-form-field>
        </div>

        <div class="form-row">
          <mat-form-field appearance="fill">
            <mat-label>Password</mat-label>
            <input matInput [type]="hidePassword ? 'password' : 'text'">
            <button mat-icon-button matSuffix (click)="hidePassword = !hidePassword">
              <mat-icon>{{hidePassword ? 'visibility_off' : 'visibility'}}</mat-icon>
            </button>
          </mat-form-field>

          <mat-form-field appearance="outline">
            <mat-label>Email</mat-label>
            <input matInput type="email" placeholder="user@example.com">
            <mat-error>Please enter a valid email</mat-error>
          </mat-form-field>
        </div>
      </div>

      <div class="form-section">
        <h3>Textarea</h3>
        <mat-form-field appearance="fill" class="full-width">
          <mat-label>Message</mat-label>
          <textarea matInput rows="4" placeholder="Enter your message"></textarea>
        </mat-form-field>
      </div>

      <div class="form-section">
        <h3>Select</h3>
        <div class="form-row">
          <mat-form-field appearance="fill">
            <mat-label>Favorite food</mat-label>
            <mat-select>
              <mat-option *ngFor="let food of foods" [value]="food.value">
                {{ food.viewValue }}
              </mat-option>
            </mat-select>
          </mat-form-field>

          <mat-form-field appearance="outline">
            <mat-label>Multiple selection</mat-label>
            <mat-select multiple>
              <mat-option *ngFor="let topping of toppings" [value]="topping">
                {{ topping }}
              </mat-option>
            </mat-select>
          </mat-form-field>
        </div>
      </div>

      <div class="form-section">
        <h3>Autocomplete</h3>
        <mat-form-field appearance="fill" class="full-width">
          <mat-label>State</mat-label>
          <input type="text" matInput [formControl]="stateCtrl" [matAutocomplete]="auto">
          <mat-autocomplete #auto="matAutocomplete">
            <mat-option *ngFor="let state of filteredStates | async" [value]="state">
              {{ state }}
            </mat-option>
          </mat-autocomplete>
        </mat-form-field>
      </div>

      <div class="form-section">
        <h3>Checkboxes and Radio Buttons</h3>
        <div class="checkbox-section">
          <mat-checkbox [(ngModel)]="checked">Check me!</mat-checkbox>
          <mat-checkbox [(ngModel)]="indeterminate" [indeterminate]="true">
            Indeterminate
          </mat-checkbox>
          <mat-checkbox disabled>Disabled</mat-checkbox>
        </div>

        <div class="radio-section">
          <mat-radio-group [(ngModel)]="selectedSeason">
            <mat-radio-button *ngFor="let season of seasons" [value]="season">
              {{ season }}
            </mat-radio-button>
          </mat-radio-group>
        </div>
      </div>

      <div class="form-section">
        <h3>Sliders</h3>
        <div class="slider-section">
          <mat-slider min="1" max="100" step="1" [value]="sliderValue">
            <input matSliderThumb [(ngModel)]="sliderValue">
          </mat-slider>
          <span class="slider-value">Value: {{ sliderValue }}</span>
        </div>

        <div class="slider-section">
          <mat-slider min="0" max="100" step="5" discrete>
            <input matSliderThumb [(ngModel)]="discreteValue">
          </mat-slider>
          <span class="slider-value">Discrete Value: {{ discreteValue }}</span>
        </div>
      </div>

      <div class="form-section">
        <h3>Slide Toggle</h3>
        <div class="toggle-section">
          <mat-slide-toggle [(ngModel)]="toggleValue">Slide me!</mat-slide-toggle>
          <mat-slide-toggle disabled>Disabled</mat-slide-toggle>
          <span>Toggle Value: {{ toggleValue }}</span>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo-container {
      padding: 20px;
      max-width: 1200px;
      margin: 0 auto;
    }
    .form-section {
      margin: 32px 0;
    }
    .form-row {
      display: flex;
      gap: 16px;
      flex-wrap: wrap;
    }
    .full-width {
      width: 100%;
    }
    .checkbox-section {
      display: flex;
      flex-direction: column;
      gap: 8px;
      margin: 16px 0;
    }
    .radio-section {
      margin: 16px 0;
    }
    .radio-section mat-radio-button {
      margin-right: 16px;
    }
    .slider-section {
      display: flex;
      align-items: center;
      gap: 16px;
      margin: 16px 0;
    }
    .slider-section mat-slider {
      flex: 1;
      max-width: 300px;
    }
    .slider-value {
      min-width: 120px;
      font-weight: 500;
    }
    .toggle-section {
      display: flex;
      align-items: center;
      gap: 16px;
      flex-wrap: wrap;
    }
    h2 {
      color: #1976d2;
      border-bottom: 2px solid #1976d2;
      padding-bottom: 8px;
    }
    h3 {
      color: #424242;
      margin: 24px 0 16px 0;
    }
  `]
})
export class FormControlsDemoComponent {
  hidePassword = true;
  checked = false;
  indeterminate = false;
  selectedSeason = 'Winter';
  sliderValue = 50;
  discreteValue = 20;
  toggleValue = false;

  foods = [
    {value: 'steak-0', viewValue: 'Steak'},
    {value: 'pizza-1', viewValue: 'Pizza'},
    {value: 'tacos-2', viewValue: 'Tacos'}
  ];

  toppings = ['Extra cheese', 'Mushroom', 'Onion', 'Pepperoni', 'Sausage', 'Tomato'];

  seasons = ['Winter', 'Spring', 'Summer', 'Autumn'];

  stateCtrl = new FormControl('');
  states = [
    'Alabama', 'Alaska', 'Arizona', 'Arkansas', 'California', 'Colorado', 'Connecticut',
    'Delaware', 'Florida', 'Georgia', 'Hawaii', 'Idaho', 'Illinois', 'Indiana', 'Iowa'
  ];

  filteredStates = this.stateCtrl.valueChanges.pipe(
    startWith(''),
    map(state => state ? this._filterStates(state) : this.states.slice())
  );

  private _filterStates(value: string): string[] {
    const filterValue = value.toLowerCase();
    return this.states.filter(state => state.toLowerCase().includes(filterValue));
  }
}
```
