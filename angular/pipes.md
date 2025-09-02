---
description: >-
  Angular Pipes transform data in templates for display purposes. 
  Learn about built-in pipes and how to create custom pipes for data transformation.
---

# Angular Pipes

## Introduction

Pipes are a useful feature in Angular that allow you to transform data in your templates. They take data as input and transform it to a desired output format. Pipes are simple functions that accept an input value and return a transformed value, making them perfect for formatting data for display.

## Key Concepts

- **Pure Pipes**: Only execute when Angular detects a pure change to the input value (default behavior)
- **Impure Pipes**: Execute on every change detection cycle
- **Parameterized Pipes**: Accept additional parameters to customize transformation
- **Chaining Pipes**: Multiple pipes can be chained together

## Built-in Pipes

### String Pipes

```typescript
@Component({
  selector: 'app-string-pipes-demo',
  template: `
    <div class="demo">
      <h3>String Pipes</h3>
      
      <div class="pipe-example">
        <h4>Original Text</h4>
        <p>{{ originalText }}</p>
        
        <h4>Uppercase Pipe</h4>
        <p>{{ originalText | uppercase }}</p>
        
        <h4>Lowercase Pipe</h4>
        <p>{{ originalText | lowercase }}</p>
        
        <h4>Titlecase Pipe</h4>
        <p>{{ originalText | titlecase }}</p>
        
        <h4>Slice Pipe</h4>
        <p>First 10 characters: {{ originalText | slice:0:10 }}</p>
        <p>From index 5: {{ originalText | slice:5 }}</p>
        <p>Last 5 characters: {{ originalText | slice:-5 }}</p>
        
        <h4>JSON Pipe</h4>
        <pre>{{ userObject | json }}</pre>
      </div>
      
      <div class="interactive-section">
        <h4>Interactive String Transformation</h4>
        <input [(ngModel)]="userInput" placeholder="Enter text to transform">
        
        <div class="transformations">
          <div class="transformation">
            <label>Uppercase:</label>
            <span>{{ userInput | uppercase }}</span>
          </div>
          <div class="transformation">
            <label>Lowercase:</label>
            <span>{{ userInput | lowercase }}</span>
          </div>
          <div class="transformation">
            <label>Title Case:</label>
            <span>{{ userInput | titlecase }}</span>
          </div>
          <div class="transformation">
            <label>First 15 chars:</label>
            <span>{{ userInput | slice:0:15 }}</span>
          </div>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo { padding: 20px; }
    .pipe-example { margin: 20px 0; }
    .pipe-example h4 { color: #2196f3; margin: 16px 0 8px 0; }
    .pipe-example p { margin: 4px 0; padding: 8px; background-color: #f5f5f5; border-radius: 4px; }
    .pipe-example pre { background-color: #f5f5f5; padding: 12px; border-radius: 4px; overflow-x: auto; }
    .interactive-section { margin-top: 30px; }
    .interactive-section input { width: 100%; padding: 8px; margin: 8px 0; border: 1px solid #ddd; border-radius: 4px; }
    .transformations { display: grid; gap: 8px; margin-top: 16px; }
    .transformation { display: flex; align-items: center; gap: 12px; padding: 8px; background-color: #f9f9f9; border-radius: 4px; }
    .transformation label { font-weight: bold; min-width: 120px; }
    .transformation span { flex: 1; padding: 4px 8px; background-color: white; border-radius: 4px; }
  `]
})
export class StringPipesDemoComponent {
  originalText = 'Angular Pipes are AWESOME for data transformation!';
  userInput = 'Hello World';
  
  userObject = {
    name: 'John Doe',
    age: 30,
    email: 'john@example.com',
    skills: ['Angular', 'TypeScript', 'JavaScript'],
    address: {
      street: '123 Main St',
      city: 'New York',
      country: 'USA'
    }
  };
}
```

### Number Pipes

```typescript
@Component({
  selector: 'app-number-pipes-demo',
  template: `
    <div class="demo">
      <h3>Number Pipes</h3>
      
      <div class="pipe-examples">
        <div class="example-group">
          <h4>Decimal Pipe</h4>
          <div class="examples">
            <div>{{ pi | number }}</div>
            <div>{{ pi | number:'1.2-4' }}</div>
            <div>{{ pi | number:'3.1-2' }}</div>
            <div>{{ largeNumber | number }}</div>
          </div>
          <div class="descriptions">
            <div>Default formatting</div>
            <div>Min 1 integer, 2-4 decimal places</div>
            <div>Min 3 integers, 1-2 decimal places</div>
            <div>Large number with separators</div>
          </div>
        </div>
        
        <div class="example-group">
          <h4>Currency Pipe</h4>
          <div class="examples">
            <div>{{ price | currency }}</div>
            <div>{{ price | currency:'EUR' }}</div>
            <div>{{ price | currency:'USD':'symbol':'1.0-0' }}</div>
            <div>{{ price | currency:'GBP':'symbol-narrow':'1.2-2' }}</div>
            <div>{{ price | currency:'JPY':'code' }}</div>
          </div>
          <div class="descriptions">
            <div>Default USD</div>
            <div>Euro currency</div>
            <div>USD, no decimals</div>
            <div>British Pound, narrow symbol</div>
            <div>Japanese Yen with code</div>
          </div>
        </div>
        
        <div class="example-group">
          <h4>Percent Pipe</h4>
          <div class="examples">
            <div>{{ percentage | percent }}</div>
            <div>{{ percentage | percent:'1.2-2' }}</div>
            <div>{{ smallPercentage | percent:'1.3-3' }}</div>
            <div>{{ largePercentage | percent:'1.0-0' }}</div>
          </div>
          <div class="descriptions">
            <div>Default formatting</div>
            <div>2 decimal places</div>
            <div>3 decimal places</div>
            <div>No decimal places</div>
          </div>
        </div>
      </div>
      
      <div class="interactive-section">
        <h4>Interactive Number Formatting</h4>
        <div class="controls">
          <label>
            Number: 
            <input type="number" [(ngModel)]="userNumber" step="0.01">
          </label>
          <label>
            Currency: 
            <select [(ngModel)]="selectedCurrency">
              <option value="USD">USD</option>
              <option value="EUR">EUR</option>
              <option value="GBP">GBP</option>
              <option value="JPY">JPY</option>
            </select>
          </label>
          <label>
            Decimal Places: 
            <input type="number" [(ngModel)]="decimalPlaces" min="0" max="4">
          </label>
        </div>
        
        <div class="results">
          <div class="result">
            <label>Number:</label>
            <span>{{ userNumber | number:'1.' + decimalPlaces + '-' + decimalPlaces }}</span>
          </div>
          <div class="result">
            <label>Currency:</label>
            <span>{{ userNumber | currency:selectedCurrency:'symbol':'1.' + decimalPlaces + '-' + decimalPlaces }}</span>
          </div>
          <div class="result">
            <label>Percentage:</label>
            <span>{{ (userNumber / 100) | percent:'1.' + decimalPlaces + '-' + decimalPlaces }}</span>
          </div>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo { padding: 20px; }
    .pipe-examples { display: grid; gap: 24px; margin: 20px 0; }
    .example-group h4 { color: #2196f3; margin-bottom: 12px; }
    .example-group { display: grid; grid-template-columns: 1fr 1fr; gap: 16px; }
    .examples, .descriptions { display: grid; gap: 8px; }
    .examples div { padding: 8px 12px; background-color: #e3f2fd; border-radius: 4px; font-family: monospace; }
    .descriptions div { padding: 8px 12px; background-color: #f5f5f5; border-radius: 4px; font-size: 14px; color: #666; }
    .interactive-section { margin-top: 30px; padding: 20px; background-color: #f9f9f9; border-radius: 8px; }
    .controls { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 16px; margin-bottom: 20px; }
    .controls label { display: flex; flex-direction: column; gap: 4px; }
    .controls input, .controls select { padding: 8px; border: 1px solid #ddd; border-radius: 4px; }
    .results { display: grid; gap: 12px; }
    .result { display: flex; justify-content: space-between; align-items: center; padding: 12px; background-color: white; border-radius: 4px; }
    .result label { font-weight: bold; }
    .result span { font-family: monospace; background-color: #e8f5e8; padding: 4px 8px; border-radius: 4px; }
  `]
})
export class NumberPipesDemoComponent {
  pi = 3.14159265359;
  largeNumber = 1234567.89;
  price = 1234.56;
  percentage = 0.1234;
  smallPercentage = 0.001234;
  largePercentage = 1.234;
  
  // Interactive controls
  userNumber = 1234.567;
  selectedCurrency = 'USD';
  decimalPlaces = 2;
}
```

### Date Pipes

```typescript
@Component({
  selector: 'app-date-pipes-demo',
  template: `
    <div class="demo">
      <h3>Date Pipes</h3>
      
      <div class="current-date">
        <h4>Current Date: {{ currentDate }}</h4>
      </div>
      
      <div class="date-formats">
        <h4>Built-in Date Formats</h4>
        <div class="format-grid">
          <div class="format-item">
            <label>Default:</label>
            <span>{{ currentDate | date }}</span>
          </div>
          <div class="format-item">
            <label>Short:</label>
            <span>{{ currentDate | date:'short' }}</span>
          </div>
          <div class="format-item">
            <label>Medium:</label>
            <span>{{ currentDate | date:'medium' }}</span>
          </div>
          <div class="format-item">
            <label>Long:</label>
            <span>{{ currentDate | date:'long' }}</span>
          </div>
          <div class="format-item">
            <label>Full:</label>
            <span>{{ currentDate | date:'full' }}</span>
          </div>
          <div class="format-item">
            <label>Short Date:</label>
            <span>{{ currentDate | date:'shortDate' }}</span>
          </div>
          <div class="format-item">
            <label>Medium Date:</label>
            <span>{{ currentDate | date:'mediumDate' }}</span>
          </div>
          <div class="format-item">
            <label>Long Date:</label>
            <span>{{ currentDate | date:'longDate' }}</span>
          </div>
          <div class="format-item">
            <label>Full Date:</label>
            <span>{{ currentDate | date:'fullDate' }}</span>
          </div>
          <div class="format-item">
            <label>Short Time:</label>
            <span>{{ currentDate | date:'shortTime' }}</span>
          </div>
          <div class="format-item">
            <label>Medium Time:</label>
            <span>{{ currentDate | date:'mediumTime' }}</span>
          </div>
          <div class="format-item">
            <label>Long Time:</label>
            <span>{{ currentDate | date:'longTime' }}</span>
          </div>
        </div>
      </div>
      
      <div class="custom-formats">
        <h4>Custom Date Formats</h4>
        <div class="format-grid">
          <div class="format-item">
            <label>yyyy-MM-dd:</label>
            <span>{{ currentDate | date:'yyyy-MM-dd' }}</span>
          </div>
          <div class="format-item">
            <label>dd/MM/yyyy:</label>
            <span>{{ currentDate | date:'dd/MM/yyyy' }}</span>
          </div>
          <div class="format-item">
            <label>MMM d, y:</label>
            <span>{{ currentDate | date:'MMM d, y' }}</span>
          </div>
          <div class="format-item">
            <label>EEEE, MMMM d:</label>
            <span>{{ currentDate | date:'EEEE, MMMM d' }}</span>
          </div>
          <div class="format-item">
            <label>HH:mm:ss:</label>
            <span>{{ currentDate | date:'HH:mm:ss' }}</span>
          </div>
          <div class="format-item">
            <label>h:mm a:</label>
            <span>{{ currentDate | date:'h:mm a' }}</span>
          </div>
        </div>
      </div>
      
      <div class="timezone-demo">
        <h4>Timezone Support</h4>
        <div class="timezone-grid">
          <div class="timezone-item">
            <label>UTC:</label>
            <span>{{ currentDate | date:'medium':'UTC' }}</span>
          </div>
          <div class="timezone-item">
            <label>New York:</label>
            <span>{{ currentDate | date:'medium':'America/New_York' }}</span>
          </div>
          <div class="timezone-item">
            <label>London:</label>
            <span>{{ currentDate | date:'medium':'Europe/London' }}</span>
          </div>
          <div class="timezone-item">
            <label>Tokyo:</label>
            <span>{{ currentDate | date:'medium':'Asia/Tokyo' }}</span>
          </div>
        </div>
      </div>
      
      <div class="interactive-section">
        <h4>Interactive Date Formatting</h4>
        <div class="controls">
          <label>
            Select Date:
            <input type="datetime-local" [(ngModel)]="selectedDateString" (change)="updateSelectedDate()">
          </label>
          <label>
            Custom Format:
            <input type="text" [(ngModel)]="customFormat" placeholder="yyyy-MM-dd HH:mm">
          </label>
          <label>
            Timezone:
            <select [(ngModel)]="selectedTimezone">
              <option value="">Local</option>
              <option value="UTC">UTC</option>
              <option value="America/New_York">New York</option>
              <option value="Europe/London">London</option>
              <option value="Asia/Tokyo">Tokyo</option>
            </select>
          </label>
        </div>
        
        <div class="result">
          <label>Formatted Date:</label>
          <span>{{ selectedDate | date:customFormat:selectedTimezone }}</span>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo { padding: 20px; }
    .current-date { margin: 20px 0; padding: 16px; background-color: #e3f2fd; border-radius: 8px; text-align: center; }
    .format-grid, .timezone-grid { display: grid; gap: 8px; margin: 16px 0; }
    .format-item, .timezone-item { display: flex; justify-content: space-between; align-items: center; padding: 8px 12px; background-color: #f5f5f5; border-radius: 4px; }
    .format-item label, .timezone-item label { font-weight: bold; min-width: 150px; }
    .format-item span, .timezone-item span { font-family: monospace; background-color: white; padding: 4px 8px; border-radius: 4px; }
    .interactive-section { margin-top: 30px; padding: 20px; background-color: #f9f9f9; border-radius: 8px; }
    .controls { display: grid; gap: 16px; margin-bottom: 20px; }
    .controls label { display: flex; flex-direction: column; gap: 4px; }
    .controls input, .controls select { padding: 8px; border: 1px solid #ddd; border-radius: 4px; }
    .result { display: flex; justify-content: space-between; align-items: center; padding: 12px; background-color: white; border-radius: 4px; }
    .result label { font-weight: bold; }
    .result span { font-family: monospace; background-color: #e8f5e8; padding: 8px 12px; border-radius: 4px; font-size: 16px; }
  `]
})
export class DatePipesDemoComponent {
  currentDate = new Date();
  selectedDate = new Date();
  selectedDateString = this.formatDateForInput(new Date());
  customFormat = 'yyyy-MM-dd HH:mm:ss';
  selectedTimezone = '';

  private formatDateForInput(date: Date): string {
    const year = date.getFullYear();
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const day = String(date.getDate()).padStart(2, '0');
    const hours = String(date.getHours()).padStart(2, '0');
    const minutes = String(date.getMinutes()).padStart(2, '0');
    return `${year}-${month}-${day}T${hours}:${minutes}`;
  }

  updateSelectedDate(): void {
    this.selectedDate = new Date(this.selectedDateString);
  }
}

### Async Pipe

```typescript
@Component({
  selector: 'app-async-pipe-demo',
  template: `
    <div class="demo">
      <h3>Async Pipe</h3>

      <div class="section">
        <h4>Observable Data</h4>
        <div class="controls">
          <button (click)="loadUsers()">Load Users</button>
          <button (click)="refreshUsers()">Refresh</button>
          <button (click)="clearUsers()">Clear</button>
        </div>

        <div class="content">
          <div *ngIf="users$ | async as users; else loading">
            <div *ngIf="users.length > 0; else noUsers">
              <h5>Users ({{ users.length }})</h5>
              <div class="user-list">
                <div *ngFor="let user of users" class="user-card">
                  <h6>{{ user.name }}</h6>
                  <p>{{ user.email }}</p>
                  <span class="role">{{ user.role }}</span>
                </div>
              </div>
            </div>
            <ng-template #noUsers>
              <p class="no-data">No users found</p>
            </ng-template>
          </div>

          <ng-template #loading>
            <div class="loading">Loading users...</div>
          </ng-template>
        </div>
      </div>

      <div class="section">
        <h4>Promise Data</h4>
        <div class="controls">
          <button (click)="loadConfig()">Load Config</button>
        </div>

        <div class="content">
          <div *ngIf="config$ | async as config; else configLoading">
            <h5>Application Configuration</h5>
            <pre>{{ config | json }}</pre>
          </div>

          <ng-template #configLoading>
            <div class="loading">Loading configuration...</div>
          </ng-template>
        </div>
      </div>

      <div class="section">
        <h4>Real-time Data</h4>
        <div class="controls">
          <button (click)="startTimer()">Start Timer</button>
          <button (click)="stopTimer()">Stop Timer</button>
        </div>

        <div class="timer-display">
          <h5>Timer: {{ timer$ | async | date:'HH:mm:ss' }}</h5>
          <p>Count: {{ counter$ | async }}</p>
        </div>
      </div>

      <div class="section">
        <h4>HTTP Requests</h4>
        <div class="controls">
          <input [(ngModel)]="searchTerm" placeholder="Search posts...">
          <button (click)="searchPosts()">Search</button>
        </div>

        <div class="search-results">
          <div *ngIf="searchResults$ | async as results">
            <h5>Search Results ({{ results.length }})</h5>
            <div class="post-list">
              <div *ngFor="let post of results" class="post-card">
                <h6>{{ post.title }}</h6>
                <p>{{ post.body | slice:0:100 }}...</p>
                <small>Post ID: {{ post.id }}</small>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo { padding: 20px; }
    .section { margin: 24px 0; padding: 20px; border: 1px solid #ddd; border-radius: 8px; }
    .section h4 { color: #2196f3; margin-top: 0; }
    .controls { display: flex; gap: 12px; margin: 16px 0; flex-wrap: wrap; }
    .controls button { padding: 8px 16px; border: 1px solid #2196f3; background: #2196f3; color: white; border-radius: 4px; cursor: pointer; }
    .controls button:hover { background: #1976d2; }
    .controls input { padding: 8px; border: 1px solid #ddd; border-radius: 4px; flex: 1; min-width: 200px; }
    .content { margin-top: 16px; }
    .loading { text-align: center; padding: 20px; color: #666; font-style: italic; }
    .no-data { text-align: center; padding: 20px; color: #999; }
    .user-list { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 12px; }
    .user-card { padding: 12px; border: 1px solid #ddd; border-radius: 4px; background: #f9f9f9; }
    .user-card h6 { margin: 0 0 8px 0; color: #333; }
    .user-card p { margin: 0 0 8px 0; color: #666; font-size: 14px; }
    .role { background: #4caf50; color: white; padding: 2px 8px; border-radius: 12px; font-size: 12px; }
    .timer-display { text-align: center; padding: 20px; background: #f0f0f0; border-radius: 8px; }
    .timer-display h5 { font-size: 24px; margin: 0 0 8px 0; color: #2196f3; }
    .post-list { display: grid; gap: 12px; }
    .post-card { padding: 16px; border: 1px solid #ddd; border-radius: 4px; background: #fafafa; }
    .post-card h6 { margin: 0 0 8px 0; color: #333; }
    .post-card p { margin: 0 0 8px 0; color: #666; line-height: 1.4; }
    .post-card small { color: #999; }
    pre { background: #f5f5f5; padding: 12px; border-radius: 4px; overflow-x: auto; }
  `]
})
export class AsyncPipeDemoComponent implements OnDestroy {
  users$: Observable<User[]> = of([]);
  config$: Observable<any> | null = null;
  timer$: Observable<Date> = of(new Date());
  counter$: Observable<number> = of(0);
  searchResults$: Observable<any[]> = of([]);

  searchTerm = '';
  private timerSubscription?: Subscription;
  private destroy$ = new Subject<void>();

  constructor(
    private http: HttpClient,
    private userService: UserService
  ) {}

  loadUsers(): void {
    this.users$ = this.userService.getUsers().pipe(
      delay(1000), // Simulate network delay
      catchError(error => {
        console.error('Error loading users:', error);
        return of([]);
      })
    );
  }

  refreshUsers(): void {
    this.loadUsers();
  }

  clearUsers(): void {
    this.users$ = of([]);
  }

  loadConfig(): void {
    this.config$ = from(this.loadConfigData()).pipe(
      delay(800),
      catchError(error => {
        console.error('Error loading config:', error);
        return of({ error: 'Failed to load configuration' });
      })
    );
  }

  private async loadConfigData(): Promise<any> {
    // Simulate async config loading
    return {
      apiUrl: 'https://api.example.com',
      version: '1.0.0',
      features: ['auth', 'notifications', 'analytics'],
      theme: 'default'
    };
  }

  startTimer(): void {
    this.stopTimer();

    this.timer$ = interval(1000).pipe(
      map(() => new Date()),
      takeUntil(this.destroy$)
    );

    this.counter$ = interval(1000).pipe(
      scan((count) => count + 1, 0),
      takeUntil(this.destroy$)
    );
  }

  stopTimer(): void {
    this.timer$ = of(new Date());
    this.counter$ = of(0);
  }

  searchPosts(): void {
    if (!this.searchTerm.trim()) {
      this.searchResults$ = of([]);
      return;
    }

    this.searchResults$ = this.http.get<any[]>('https://jsonplaceholder.typicode.com/posts').pipe(
      map(posts => posts.filter(post =>
        post.title.toLowerCase().includes(this.searchTerm.toLowerCase()) ||
        post.body.toLowerCase().includes(this.searchTerm.toLowerCase())
      )),
      catchError(error => {
        console.error('Error searching posts:', error);
        return of([]);
      })
    );
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

## Custom Pipes

### Basic Custom Pipe

```typescript
// truncate.pipe.ts
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'truncate'
})
export class TruncatePipe implements PipeTransform {
  transform(value: string, limit: number = 50, trail: string = '...'): string {
    if (!value) return '';

    if (value.length <= limit) {
      return value;
    }

    return value.substring(0, limit) + trail;
  }
}

// word-count.pipe.ts
@Pipe({
  name: 'wordCount'
})
export class WordCountPipe implements PipeTransform {
  transform(value: string): number {
    if (!value) return 0;

    return value.trim().split(/\s+/).filter(word => word.length > 0).length;
  }
}

// highlight.pipe.ts
@Pipe({
  name: 'highlight'
})
export class HighlightPipe implements PipeTransform {
  transform(value: string, searchTerm: string, className: string = 'highlight'): string {
    if (!value || !searchTerm) return value;

    const regex = new RegExp(`(${searchTerm})`, 'gi');
    return value.replace(regex, `<span class="${className}">$1</span>`);
  }
}

// Usage
@Component({
  selector: 'app-custom-pipes-demo',
  template: `
    <div class="demo">
      <h3>Custom Pipes</h3>

      <div class="section">
        <h4>Text Processing Pipes</h4>
        <div class="text-input">
          <textarea [(ngModel)]="sampleText" rows="4" cols="50"></textarea>
        </div>

        <div class="pipe-results">
          <div class="result">
            <label>Original ({{ sampleText | wordCount }} words):</label>
            <p>{{ sampleText }}</p>
          </div>

          <div class="result">
            <label>Truncated (20 chars):</label>
            <p>{{ sampleText | truncate:20 }}</p>
          </div>

          <div class="result">
            <label>Truncated (50 chars, custom trail):</label>
            <p>{{ sampleText | truncate:50:' [more...]' }}</p>
          </div>

          <div class="result">
            <label>Highlighted (search: "{{ searchTerm }}"):</label>
            <p [innerHTML]="sampleText | highlight:searchTerm"></p>
          </div>
        </div>

        <div class="controls">
          <input [(ngModel)]="searchTerm" placeholder="Enter search term">
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo { padding: 20px; }
    .section { margin: 24px 0; }
    .text-input textarea { width: 100%; padding: 12px; border: 1px solid #ddd; border-radius: 4px; font-family: inherit; }
    .pipe-results { margin: 20px 0; }
    .result { margin: 16px 0; padding: 16px; background: #f9f9f9; border-radius: 4px; }
    .result label { font-weight: bold; color: #2196f3; display: block; margin-bottom: 8px; }
    .result p { margin: 0; line-height: 1.5; }
    .controls input { padding: 8px; border: 1px solid #ddd; border-radius: 4px; width: 200px; }
    :host ::ng-deep .highlight { background-color: yellow; font-weight: bold; }
  `]
})
export class CustomPipesDemoComponent {
  sampleText = 'Angular pipes are a great way to transform data in templates. They provide a clean and reusable way to format and process data for display purposes.';
  searchTerm = 'Angular';
}

### Advanced Custom Pipes

```typescript
// filter.pipe.ts
@Pipe({
  name: 'filter',
  pure: false // Impure pipe to detect array changes
})
export class FilterPipe implements PipeTransform {
  transform<T>(items: T[], searchTerm: string, property?: keyof T): T[] {
    if (!items || !searchTerm) return items;

    return items.filter(item => {
      if (property) {
        const value = item[property];
        return String(value).toLowerCase().includes(searchTerm.toLowerCase());
      } else {
        return String(item).toLowerCase().includes(searchTerm.toLowerCase());
      }
    });
  }
}

// sort.pipe.ts
@Pipe({
  name: 'sort',
  pure: false
})
export class SortPipe implements PipeTransform {
  transform<T>(items: T[], property?: keyof T, direction: 'asc' | 'desc' = 'asc'): T[] {
    if (!items || items.length <= 1) return items;

    return [...items].sort((a, b) => {
      let valueA: any;
      let valueB: any;

      if (property) {
        valueA = a[property];
        valueB = b[property];
      } else {
        valueA = a;
        valueB = b;
      }

      // Handle different data types
      if (typeof valueA === 'string' && typeof valueB === 'string') {
        valueA = valueA.toLowerCase();
        valueB = valueB.toLowerCase();
      }

      if (valueA < valueB) {
        return direction === 'asc' ? -1 : 1;
      }
      if (valueA > valueB) {
        return direction === 'asc' ? 1 : -1;
      }
      return 0;
    });
  }
}

// file-size.pipe.ts
@Pipe({
  name: 'fileSize'
})
export class FileSizePipe implements PipeTransform {
  transform(bytes: number, precision: number = 2): string {
    if (!bytes || bytes === 0) return '0 B';

    const units = ['B', 'KB', 'MB', 'GB', 'TB', 'PB'];
    const base = 1024;
    const unitIndex = Math.floor(Math.log(bytes) / Math.log(base));
    const size = bytes / Math.pow(base, unitIndex);

    return `${size.toFixed(precision)} ${units[unitIndex]}`;
  }
}

// time-ago.pipe.ts
@Pipe({
  name: 'timeAgo',
  pure: false // Updates over time
})
export class TimeAgoPipe implements PipeTransform {
  transform(value: Date | string | number): string {
    if (!value) return '';

    const date = new Date(value);
    const now = new Date();
    const diffInSeconds = Math.floor((now.getTime() - date.getTime()) / 1000);

    if (diffInSeconds < 60) {
      return 'just now';
    }

    const intervals = [
      { label: 'year', seconds: 31536000 },
      { label: 'month', seconds: 2592000 },
      { label: 'week', seconds: 604800 },
      { label: 'day', seconds: 86400 },
      { label: 'hour', seconds: 3600 },
      { label: 'minute', seconds: 60 }
    ];

    for (const interval of intervals) {
      const count = Math.floor(diffInSeconds / interval.seconds);
      if (count >= 1) {
        return `${count} ${interval.label}${count > 1 ? 's' : ''} ago`;
      }
    }

    return 'just now';
  }
}

// Usage
@Component({
  selector: 'app-advanced-pipes-demo',
  template: `
    <div class="demo">
      <h3>Advanced Custom Pipes</h3>

      <div class="section">
        <h4>Filter and Sort Pipes</h4>
        <div class="controls">
          <input [(ngModel)]="filterTerm" placeholder="Filter users...">
          <select [(ngModel)]="sortProperty">
            <option value="name">Sort by Name</option>
            <option value="age">Sort by Age</option>
            <option value="email">Sort by Email</option>
          </select>
          <select [(ngModel)]="sortDirection">
            <option value="asc">Ascending</option>
            <option value="desc">Descending</option>
          </select>
          <button (click)="addUser()">Add Random User</button>
        </div>

        <div class="user-grid">
          <div *ngFor="let user of users | filter:filterTerm:'name' | sort:sortProperty:sortDirection"
               class="user-card">
            <h5>{{ user.name }}</h5>
            <p>Age: {{ user.age }}</p>
            <p>{{ user.email }}</p>
            <small>Added {{ user.createdAt | timeAgo }}</small>
          </div>
        </div>
      </div>

      <div class="section">
        <h4>File Size Pipe</h4>
        <div class="file-sizes">
          <div *ngFor="let file of sampleFiles" class="file-item">
            <span class="filename">{{ file.name }}</span>
            <span class="filesize">{{ file.size | fileSize }}</span>
            <span class="filesize-precise">{{ file.size | fileSize:4 }}</span>
          </div>
        </div>
      </div>

      <div class="section">
        <h4>Time Ago Pipe</h4>
        <div class="time-examples">
          <div *ngFor="let event of timeEvents" class="time-item">
            <span class="event">{{ event.description }}</span>
            <span class="time">{{ event.timestamp | timeAgo }}</span>
            <span class="exact">{{ event.timestamp | date:'medium' }}</span>
          </div>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo { padding: 20px; }
    .section { margin: 24px 0; padding: 20px; border: 1px solid #ddd; border-radius: 8px; }
    .controls { display: flex; gap: 12px; margin-bottom: 20px; flex-wrap: wrap; }
    .controls input, .controls select { padding: 8px; border: 1px solid #ddd; border-radius: 4px; }
    .controls button { padding: 8px 16px; background: #4caf50; color: white; border: none; border-radius: 4px; cursor: pointer; }
    .user-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(250px, 1fr)); gap: 16px; }
    .user-card { padding: 16px; border: 1px solid #ddd; border-radius: 8px; background: #f9f9f9; }
    .user-card h5 { margin: 0 0 8px 0; color: #333; }
    .user-card p { margin: 4px 0; color: #666; }
    .user-card small { color: #999; font-style: italic; }
    .file-sizes { display: grid; gap: 8px; }
    .file-item { display: grid; grid-template-columns: 1fr auto auto; gap: 16px; padding: 8px; background: #f5f5f5; border-radius: 4px; align-items: center; }
    .filename { font-weight: bold; }
    .filesize { font-family: monospace; background: #e3f2fd; padding: 4px 8px; border-radius: 4px; }
    .filesize-precise { font-family: monospace; background: #e8f5e8; padding: 4px 8px; border-radius: 4px; font-size: 12px; }
    .time-examples { display: grid; gap: 8px; }
    .time-item { display: grid; grid-template-columns: 1fr auto auto; gap: 16px; padding: 8px; background: #f5f5f5; border-radius: 4px; align-items: center; }
    .event { font-weight: bold; }
    .time { font-family: monospace; background: #fff3e0; padding: 4px 8px; border-radius: 4px; }
    .exact { font-family: monospace; background: #f3e5f5; padding: 4px 8px; border-radius: 4px; font-size: 12px; }
  `]
})
export class AdvancedPipesDemoComponent {
  filterTerm = '';
  sortProperty: 'name' | 'age' | 'email' = 'name';
  sortDirection: 'asc' | 'desc' = 'asc';

  users = [
    { name: 'John Doe', age: 30, email: 'john@example.com', createdAt: new Date(Date.now() - 86400000) },
    { name: 'Jane Smith', age: 25, email: 'jane@example.com', createdAt: new Date(Date.now() - 172800000) },
    { name: 'Bob Johnson', age: 35, email: 'bob@example.com', createdAt: new Date(Date.now() - 259200000) }
  ];

  sampleFiles = [
    { name: 'document.pdf', size: 2048576 },
    { name: 'image.jpg', size: 1536000 },
    { name: 'video.mp4', size: 104857600 },
    { name: 'archive.zip', size: 52428800 },
    { name: 'text.txt', size: 1024 }
  ];

  timeEvents = [
    { description: 'User logged in', timestamp: new Date(Date.now() - 300000) },
    { description: 'File uploaded', timestamp: new Date(Date.now() - 3600000) },
    { description: 'Profile updated', timestamp: new Date(Date.now() - 86400000) },
    { description: 'Account created', timestamp: new Date(Date.now() - 604800000) }
  ];

  addUser(): void {
    const names = ['Alice Brown', 'Charlie Wilson', 'Diana Davis', 'Eve Miller', 'Frank Garcia'];
    const randomName = names[Math.floor(Math.random() * names.length)];
    const randomAge = Math.floor(Math.random() * 40) + 20;
    const randomEmail = `${randomName.toLowerCase().replace(' ', '.')}@example.com`;

    this.users.push({
      name: randomName,
      age: randomAge,
      email: randomEmail,
      createdAt: new Date()
    });
  }
}
```

## Pure vs Impure Pipes

### Understanding Pure Pipes

```typescript
// Pure pipe (default behavior)
@Pipe({
  name: 'purePipe',
  pure: true // This is the default
})
export class PurePipe implements PipeTransform {
  transform(value: any): any {
    console.log('Pure pipe executed'); // Only logs when input reference changes
    return value;
  }
}

// Impure pipe
@Pipe({
  name: 'impurePipe',
  pure: false
})
export class ImpurePipe implements PipeTransform {
  transform(value: any): any {
    console.log('Impure pipe executed'); // Logs on every change detection cycle
    return value;
  }
}

@Component({
  selector: 'app-pure-impure-demo',
  template: `
    <div class="demo">
      <h3>Pure vs Impure Pipes</h3>

      <div class="section">
        <h4>Pure Pipe Behavior</h4>
        <p>Pure pipes only execute when the input reference changes.</p>

        <div class="controls">
          <button (click)="addItem()">Add Item (changes array reference)</button>
          <button (click)="modifyItem()">Modify Item (same reference)</button>
          <button (click)="updateCounter()">Update Counter</button>
        </div>

        <div class="results">
          <div class="result">
            <label>Array with Pure Pipe:</label>
            <p>{{ items | purePipe | json }}</p>
          </div>

          <div class="result">
            <label>Array with Impure Pipe:</label>
            <p>{{ items | impurePipe | json }}</p>
          </div>

          <div class="result">
            <label>Counter:</label>
            <p>{{ counter }}</p>
          </div>
        </div>
      </div>

      <div class="section">
        <h4>Performance Comparison</h4>
        <p>Open browser console to see execution logs.</p>

        <div class="performance-info">
          <div class="info-card">
            <h5>Pure Pipes</h5>
            <ul>
              <li>Execute only when input reference changes</li>
              <li>Better performance for expensive operations</li>
              <li>Default behavior</li>
              <li>Suitable for most use cases</li>
            </ul>
          </div>

          <div class="info-card">
            <h5>Impure Pipes</h5>
            <ul>
              <li>Execute on every change detection cycle</li>
              <li>Can impact performance if overused</li>
              <li>Necessary for time-based or stateful operations</li>
              <li>Use sparingly and optimize carefully</li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo { padding: 20px; }
    .section { margin: 24px 0; padding: 20px; border: 1px solid #ddd; border-radius: 8px; }
    .controls { display: flex; gap: 12px; margin: 16px 0; flex-wrap: wrap; }
    .controls button { padding: 8px 16px; border: 1px solid #2196f3; background: #2196f3; color: white; border-radius: 4px; cursor: pointer; }
    .results { margin: 20px 0; }
    .result { margin: 12px 0; padding: 12px; background: #f9f9f9; border-radius: 4px; }
    .result label { font-weight: bold; display: block; margin-bottom: 8px; }
    .result p { margin: 0; font-family: monospace; background: white; padding: 8px; border-radius: 4px; }
    .performance-info { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin-top: 20px; }
    .info-card { padding: 16px; background: #f5f5f5; border-radius: 8px; }
    .info-card h5 { margin-top: 0; color: #2196f3; }
    .info-card ul { margin: 0; padding-left: 20px; }
    .info-card li { margin: 4px 0; }
  `]
})
export class PureImpureDemoComponent {
  items = [{ id: 1, name: 'Item 1' }, { id: 2, name: 'Item 2' }];
  counter = 0;

  addItem(): void {
    // Creates new array reference - pure pipe will execute
    this.items = [...this.items, { id: this.items.length + 1, name: `Item ${this.items.length + 1}` }];
  }

  modifyItem(): void {
    // Modifies existing array - pure pipe won't execute, impure pipe will
    if (this.items.length > 0) {
      this.items[0].name = `Modified Item ${Date.now()}`;
    }
  }

  updateCounter(): void {
    // Simple value change - both pipes will execute
    this.counter++;
  }
}

## Testing Pipes

### Testing Pure Pipes

```typescript
// truncate.pipe.spec.ts
import { TruncatePipe } from './truncate.pipe';

describe('TruncatePipe', () => {
  let pipe: TruncatePipe;

  beforeEach(() => {
    pipe = new TruncatePipe();
  });

  it('should create an instance', () => {
    expect(pipe).toBeTruthy();
  });

  it('should return original string if shorter than limit', () => {
    const result = pipe.transform('Hello', 10);
    expect(result).toBe('Hello');
  });

  it('should truncate string if longer than limit', () => {
    const result = pipe.transform('Hello World', 5);
    expect(result).toBe('Hello...');
  });

  it('should use custom trail', () => {
    const result = pipe.transform('Hello World', 5, ' [more]');
    expect(result).toBe('Hello [more]');
  });

  it('should handle empty string', () => {
    const result = pipe.transform('', 10);
    expect(result).toBe('');
  });

  it('should handle null/undefined', () => {
    expect(pipe.transform(null as any, 10)).toBe('');
    expect(pipe.transform(undefined as any, 10)).toBe('');
  });

  it('should use default limit when not provided', () => {
    const longText = 'a'.repeat(60);
    const result = pipe.transform(longText);
    expect(result).toBe('a'.repeat(50) + '...');
  });
});

// file-size.pipe.spec.ts
describe('FileSizePipe', () => {
  let pipe: FileSizePipe;

  beforeEach(() => {
    pipe = new FileSizePipe();
  });

  it('should format bytes correctly', () => {
    expect(pipe.transform(0)).toBe('0 B');
    expect(pipe.transform(1024)).toBe('1.00 KB');
    expect(pipe.transform(1048576)).toBe('1.00 MB');
    expect(pipe.transform(1073741824)).toBe('1.00 GB');
  });

  it('should respect precision parameter', () => {
    expect(pipe.transform(1536, 0)).toBe('2 KB');
    expect(pipe.transform(1536, 1)).toBe('1.5 KB');
    expect(pipe.transform(1536, 3)).toBe('1.500 KB');
  });

  it('should handle edge cases', () => {
    expect(pipe.transform(null as any)).toBe('0 B');
    expect(pipe.transform(undefined as any)).toBe('0 B');
  });
});
```

### Testing Pipes in Components

```typescript
// component-with-pipes.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { By } from '@angular/platform-browser';
import { TruncatePipe } from './truncate.pipe';
import { FileSizePipe } from './file-size.pipe';
import { Component } from '@angular/core';

@Component({
  template: `
    <div class="truncated">{{ text | truncate:limit }}</div>
    <div class="file-size">{{ fileSize | fileSize }}</div>
  `
})
class TestComponent {
  text = 'This is a long text that should be truncated';
  limit = 10;
  fileSize = 2048;
}

describe('Component with Pipes', () => {
  let component: TestComponent;
  let fixture: ComponentFixture<TestComponent>;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [TestComponent, TruncatePipe, FileSizePipe]
    }).compileComponents();

    fixture = TestBed.createComponent(TestComponent);
    component = fixture.componentInstance;
  });

  it('should apply truncate pipe', () => {
    fixture.detectChanges();

    const truncatedElement = fixture.debugElement.query(By.css('.truncated'));
    expect(truncatedElement.nativeElement.textContent).toBe('This is a ...');
  });

  it('should apply file size pipe', () => {
    fixture.detectChanges();

    const fileSizeElement = fixture.debugElement.query(By.css('.file-size'));
    expect(fileSizeElement.nativeElement.textContent).toBe('2.00 KB');
  });

  it('should update when input changes', () => {
    component.limit = 20;
    fixture.detectChanges();

    const truncatedElement = fixture.debugElement.query(By.css('.truncated'));
    expect(truncatedElement.nativeElement.textContent).toBe('This is a long text ...');
  });
});
```

## Best Practices

### Pipe Design Principles

```typescript
// ✅ Good: Pure pipe for simple transformations
@Pipe({
  name: 'capitalize',
  pure: true
})
export class CapitalizePipe implements PipeTransform {
  transform(value: string): string {
    if (!value) return '';
    return value.charAt(0).toUpperCase() + value.slice(1).toLowerCase();
  }
}

// ✅ Good: Handle edge cases
@Pipe({
  name: 'safeHtml'
})
export class SafeHtmlPipe implements PipeTransform {
  constructor(private sanitizer: DomSanitizer) {}

  transform(value: string): SafeHtml {
    if (!value) return '';
    return this.sanitizer.bypassSecurityTrustHtml(value);
  }
}

// ✅ Good: Parameterized pipe with defaults
@Pipe({
  name: 'round'
})
export class RoundPipe implements PipeTransform {
  transform(value: number, precision: number = 0): number {
    if (value == null || isNaN(value)) return 0;

    const factor = Math.pow(10, precision);
    return Math.round(value * factor) / factor;
  }
}

// ✅ Good: Impure pipe for time-sensitive data
@Pipe({
  name: 'relativeTime',
  pure: false
})
export class RelativeTimePipe implements PipeTransform {
  transform(value: Date | string | number): string {
    // Implementation for relative time
    return this.calculateRelativeTime(new Date(value));
  }

  private calculateRelativeTime(date: Date): string {
    // Time calculation logic
    return 'relative time';
  }
}
```

### Performance Optimization

```typescript
// ✅ Use pure pipes when possible
@Pipe({
  name: 'expensiveCalculation',
  pure: true // Only recalculates when input changes
})
export class ExpensiveCalculationPipe implements PipeTransform {
  transform(value: number[]): number {
    console.log('Expensive calculation executed');
    return value.reduce((sum, num) => sum + Math.pow(num, 2), 0);
  }
}

// ✅ Optimize impure pipes
@Pipe({
  name: 'optimizedFilter',
  pure: false
})
export class OptimizedFilterPipe implements PipeTransform {
  private lastItems: any[] = [];
  private lastFilter = '';
  private cachedResult: any[] = [];

  transform(items: any[], filter: string): any[] {
    // Use caching to avoid unnecessary recalculations
    if (items === this.lastItems && filter === this.lastFilter) {
      return this.cachedResult;
    }

    this.lastItems = items;
    this.lastFilter = filter;
    this.cachedResult = items.filter(item =>
      item.name.toLowerCase().includes(filter.toLowerCase())
    );

    return this.cachedResult;
  }
}

// ✅ Use memoization for complex calculations
@Pipe({
  name: 'memoized'
})
export class MemoizedPipe implements PipeTransform {
  private cache = new Map<string, any>();

  transform(value: any, ...args: any[]): any {
    const key = JSON.stringify([value, ...args]);

    if (this.cache.has(key)) {
      return this.cache.get(key);
    }

    const result = this.performExpensiveOperation(value, ...args);
    this.cache.set(key, result);

    return result;
  }

  private performExpensiveOperation(value: any, ...args: any[]): any {
    // Expensive operation here
    return value;
  }
}
```

## Common Pitfalls

### Issue 1: Overusing Impure Pipes
Using impure pipes unnecessarily can hurt performance.

**Solution:**
```typescript
// ❌ Bad: Impure pipe for simple transformation
@Pipe({
  name: 'badUppercase',
  pure: false // Unnecessary for simple string transformation
})
export class BadUppercasePipe implements PipeTransform {
  transform(value: string): string {
    return value?.toUpperCase() || '';
  }
}

// ✅ Good: Pure pipe for simple transformation
@Pipe({
  name: 'goodUppercase',
  pure: true // Default, but explicit for clarity
})
export class GoodUppercasePipe implements PipeTransform {
  transform(value: string): string {
    return value?.toUpperCase() || '';
  }
}
```

### Issue 2: Not Handling Null/Undefined Values
Pipes should gracefully handle edge cases.

**Solution:**
```typescript
// ❌ Bad: No null checking
@Pipe({
  name: 'badLength'
})
export class BadLengthPipe implements PipeTransform {
  transform(value: string): number {
    return value.length; // Will throw error if value is null/undefined
  }
}

// ✅ Good: Proper null checking
@Pipe({
  name: 'goodLength'
})
export class GoodLengthPipe implements PipeTransform {
  transform(value: string | null | undefined): number {
    return value?.length ?? 0;
  }
}
```

### Issue 3: Expensive Operations in Impure Pipes
Performing expensive operations in impure pipes without optimization.

**Solution:**
```typescript
// ❌ Bad: Expensive operation on every change detection
@Pipe({
  name: 'badExpensive',
  pure: false
})
export class BadExpensivePipe implements PipeTransform {
  transform(items: any[]): any[] {
    // This runs on every change detection cycle!
    return items.map(item => ({
      ...item,
      processed: this.expensiveCalculation(item)
    }));
  }

  private expensiveCalculation(item: any): any {
    // Expensive operation
    return item;
  }
}

// ✅ Good: Cached expensive operation
@Pipe({
  name: 'goodExpensive',
  pure: false
})
export class GoodExpensivePipe implements PipeTransform {
  private cache = new WeakMap();

  transform(items: any[]): any[] {
    return items.map(item => {
      if (!this.cache.has(item)) {
        this.cache.set(item, {
          ...item,
          processed: this.expensiveCalculation(item)
        });
      }
      return this.cache.get(item);
    });
  }

  private expensiveCalculation(item: any): any {
    // Expensive operation
    return item;
  }
}
```

### Issue 4: Mutating Input Data
Pipes should not mutate the input data.

**Solution:**
```typescript
// ❌ Bad: Mutating input array
@Pipe({
  name: 'badSort'
})
export class BadSortPipe implements PipeTransform {
  transform(items: any[]): any[] {
    return items.sort(); // Mutates original array!
  }
}

// ✅ Good: Creating new array
@Pipe({
  name: 'goodSort'
})
export class GoodSortPipe implements PipeTransform {
  transform(items: any[]): any[] {
    return [...items].sort(); // Creates new array
  }
}
```

## References

{% embed url="https://angular.io/guide/pipes" %}

{% embed url="https://angular.io/guide/pipes-overview" %}

{% embed url="https://angular.io/api/core/Pipe" %}

{% embed url="https://angular.io/guide/testing-pipes" %}
```
```
```
