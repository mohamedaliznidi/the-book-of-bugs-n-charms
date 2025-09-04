---
description: >-
  RxJS Reactive Stream Magic provides reactive programming capabilities for Angular
  applications using Observables. Master operators, subjects, and reactive patterns
  for handling asynchronous data stream enchantments.
---

# RxJS - Reactive Stream Magic Extensions for JavaScript

## The Ancient Knowledge

RxJS (Reactive Extensions for JavaScript) is a mystical library for reactive programming using Observables, making it easier to compose asynchronous or callback-based code through magical streams. It's the foundation of Angular's reactive architecture and provides powerful mystical tools for handling data streams, events, and asynchronous operations.

## Core Mystical Concepts

- **Observable**: A mystical collection of future values or events
- **Observer**: A mystical consumer of values delivered by an Observable
- **Subscription**: Represents the mystical execution of an Observable
- **Operators**: Pure mystical functions that enable functional programming style
- **Subject**: A special type of Observable that allows mystical multicasting
- **Scheduler**: Controls when a mystical subscription starts and when notifications are delivered

## Observable Mystical Fundamentals

### Creating Observable Streams

```typescript
import { Observable, of, from, interval, fromEvent, timer } from 'rxjs';
import { map, filter, take } from 'rxjs/operators';

@Component({
  selector: 'app-observables-demo',
  template: `
    <div class="demo-container">
      <h2>Observable Creation</h2>
      
      <div class="section">
        <h3>Basic Observables</h3>
        <button (click)="demonstrateBasicObservables()">Run Basic Examples</button>
        <div class="output" #basicOutput></div>
      </div>

      <div class="section">
        <h3>From Events</h3>
        <button #clickButton>Click Me!</button>
        <div class="output" #eventOutput></div>
      </div>

      <div class="section">
        <h3>Timer and Interval</h3>
        <button (click)="startTimer()">Start Timer</button>
        <button (click)="stopTimer()">Stop Timer</button>
        <div class="output">Timer: {{ timerValue }}</div>
      </div>

      <div class="section">
        <h3>HTTP Requests</h3>
        <button (click)="makeHttpRequest()">Fetch Data</button>
        <div class="output" *ngIf="httpData">
          <pre>{{ httpData | json }}</pre>
        </div>
      </div>
    </div>
  `,
  styles: [`
    .demo-container { padding: 20px; max-width: 1000px; margin: 0 auto; }
    .section { margin: 24px 0; padding: 16px; border: 1px solid #ddd; border-radius: 8px; }
    .output { margin-top: 12px; padding: 12px; background: #f5f5f5; border-radius: 4px; min-height: 40px; }
    button { margin: 4px; padding: 8px 16px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; }
    pre { white-space: pre-wrap; word-break: break-all; }
    h2 { color: #1976d2; }
    h3 { color: #424242; }
  `]
})
export class ObservablesDemoComponent implements OnInit, AfterViewInit, OnDestroy {
  @ViewChild('basicOutput') basicOutput!: ElementRef;
  @ViewChild('eventOutput') eventOutput!: ElementRef;
  @ViewChild('clickButton') clickButton!: ElementRef;

  timerValue = 0;
  httpData: any = null;
  private subscriptions: Subscription[] = [];

  constructor(private http: HttpClient) {}

  ngOnInit(): void {
    this.demonstrateObservableCreation();
  }

  ngAfterViewInit(): void {
    this.setupEventObservables();
  }

  ngOnDestroy(): void {
    this.subscriptions.forEach(sub => sub.unsubscribe());
  }

  private demonstrateObservableCreation(): void {
    // Creating observables from scratch
    const customObservable = new Observable<number>(observer => {
      let count = 0;
      const intervalId = setInterval(() => {
        observer.next(count++);
        if (count > 5) {
          observer.complete();
          clearInterval(intervalId);
        }
      }, 1000);

      // Cleanup function
      return () => {
        clearInterval(intervalId);
      };
    });

    // Subscribe to custom observable
    const sub = customObservable.subscribe({
      next: value => console.log('Custom Observable:', value),
      complete: () => console.log('Custom Observable completed')
    });

    this.subscriptions.push(sub);
  }

  demonstrateBasicObservables(): void {
    const output = this.basicOutput.nativeElement;
    output.innerHTML = '';

    // of() - creates observable from values
    const ofExample = of(1, 2, 3, 4, 5);
    ofExample.subscribe(value => {
      output.innerHTML += `of(): ${value}<br>`;
    });

    // from() - creates observable from array, promise, or iterable
    const fromExample = from([10, 20, 30, 40, 50]);
    fromExample.pipe(
      map(x => x * 2),
      filter(x => x > 40)
    ).subscribe(value => {
      output.innerHTML += `from() with operators: ${value}<br>`;
    });

    // range() equivalent using from
    const rangeExample = from(Array.from({length: 5}, (_, i) => i + 1));
    rangeExample.subscribe(value => {
      output.innerHTML += `range(): ${value}<br>`;
    });
  }

  private setupEventObservables(): void {
    // fromEvent() - creates observable from DOM events
    const clickObservable = fromEvent(this.clickButton.nativeElement, 'click');
    
    const clickSub = clickObservable.pipe(
      map((event: Event) => {
        const target = event.target as HTMLElement;
        return {
          timestamp: new Date().toLocaleTimeString(),
          element: target.tagName
        };
      }),
      take(10) // Only take first 10 clicks
    ).subscribe(data => {
      this.eventOutput.nativeElement.innerHTML += 
        `Clicked at ${data.timestamp}<br>`;
    });

    this.subscriptions.push(clickSub);
  }

  startTimer(): void {
    this.stopTimer(); // Stop any existing timer
    
    const timerObservable = interval(1000);
    const timerSub = timerObservable.subscribe(value => {
      this.timerValue = value;
    });

    this.subscriptions.push(timerSub);
  }

  stopTimer(): void {
    this.subscriptions.forEach(sub => sub.unsubscribe());
    this.subscriptions = [];
    this.timerValue = 0;
  }

  makeHttpRequest(): void {
    const httpSub = this.http.get('https://jsonplaceholder.typicode.com/posts/1')
      .subscribe(data => {
        this.httpData = data;
      });

    this.subscriptions.push(httpSub);
  }
}
```

### Observable Operators

```typescript
@Component({
  selector: 'app-operators-demo',
  template: `
    <div class="demo-container">
      <h2>RxJS Operators</h2>
      
      <div class="section">
        <h3>Transformation Operators</h3>
        <button (click)="demonstrateTransformationOperators()">Run Examples</button>
        <div class="output" #transformOutput></div>
      </div>

      <div class="section">
        <h3>Filtering Operators</h3>
        <button (click)="demonstrateFilteringOperators()">Run Examples</button>
        <div class="output" #filterOutput></div>
      </div>

      <div class="section">
        <h3>Combination Operators</h3>
        <button (click)="demonstrateCombinationOperators()">Run Examples</button>
        <div class="output" #combineOutput></div>
      </div>

      <div class="section">
        <h3>Error Handling</h3>
        <button (click)="demonstrateErrorHandling()">Run Examples</button>
        <div class="output" #errorOutput></div>
      </div>

      <div class="section">
        <h3>Utility Operators</h3>
        <button (click)="demonstrateUtilityOperators()">Run Examples</button>
        <div class="output" #utilityOutput></div>
      </div>
    </div>
  `,
  styles: [`
    .demo-container { padding: 20px; max-width: 1000px; margin: 0 auto; }
    .section { margin: 24px 0; padding: 16px; border: 1px solid #ddd; border-radius: 8px; }
    .output { margin-top: 12px; padding: 12px; background: #f5f5f5; border-radius: 4px; min-height: 40px; font-family: monospace; }
    button { margin: 4px; padding: 8px 16px; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; }
    h2 { color: #1976d2; }
    h3 { color: #424242; }
  `]
})
export class OperatorsDemoComponent {
  @ViewChild('transformOutput') transformOutput!: ElementRef;
  @ViewChild('filterOutput') filterOutput!: ElementRef;
  @ViewChild('combineOutput') combineOutput!: ElementRef;
  @ViewChild('errorOutput') errorOutput!: ElementRef;
  @ViewChild('utilityOutput') utilityOutput!: ElementRef;

  demonstrateTransformationOperators(): void {
    const output = this.transformOutput.nativeElement;
    output.innerHTML = '';

    // map() - transforms each value
    of(1, 2, 3, 4, 5).pipe(
      map(x => x * x)
    ).subscribe(value => {
      output.innerHTML += `map(x => x * x): ${value}<br>`;
    });

    // mergeMap() - flattens inner observables
    of('a', 'b', 'c').pipe(
      mergeMap(letter => of(1, 2, 3).pipe(
        map(num => `${letter}${num}`)
      ))
    ).subscribe(value => {
      output.innerHTML += `mergeMap: ${value}<br>`;
    });

    // switchMap() - switches to new inner observable
    of('x', 'y', 'z').pipe(
      switchMap(letter => timer(0, 500).pipe(
        map(num => `${letter}${num}`),
        take(3)
      ))
    ).subscribe(value => {
      output.innerHTML += `switchMap: ${value}<br>`;
    });

    // scan() - accumulates values over time
    of(1, 2, 3, 4, 5).pipe(
      scan((acc, curr) => acc + curr, 0)
    ).subscribe(value => {
      output.innerHTML += `scan (running sum): ${value}<br>`;
    });
  }

  demonstrateFilteringOperators(): void {
    const output = this.filterOutput.nativeElement;
    output.innerHTML = '';

    // filter() - filters values based on predicate
    of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10).pipe(
      filter(x => x % 2 === 0)
    ).subscribe(value => {
      output.innerHTML += `filter (even numbers): ${value}<br>`;
    });

    // take() - takes first n values
    interval(500).pipe(
      take(5)
    ).subscribe(value => {
      output.innerHTML += `take(5): ${value}<br>`;
    });

    // skip() - skips first n values
    of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10).pipe(
      skip(5)
    ).subscribe(value => {
      output.innerHTML += `skip(5): ${value}<br>`;
    });

    // distinct() - filters out duplicate values
    of(1, 2, 2, 3, 3, 3, 4, 4, 5).pipe(
      distinct()
    ).subscribe(value => {
      output.innerHTML += `distinct: ${value}<br>`;
    });

    // debounceTime() - emits only after specified time has passed
    const searchInput = new Subject<string>();
    searchInput.pipe(
      debounceTime(300),
      distinctUntilChanged()
    ).subscribe(value => {
      output.innerHTML += `debounced search: ${value}<br>`;
    });

    // Simulate search input
    setTimeout(() => searchInput.next('a'), 100);
    setTimeout(() => searchInput.next('an'), 200);
    setTimeout(() => searchInput.next('ang'), 300);
    setTimeout(() => searchInput.next('angu'), 400);
    setTimeout(() => searchInput.next('angular'), 500);
  }

  demonstrateCombinationOperators(): void {
    const output = this.combineOutput.nativeElement;
    output.innerHTML = '';

    // combineLatest() - combines latest values from multiple observables
    const obs1 = of('A', 'B', 'C');
    const obs2 = of(1, 2, 3);
    
    combineLatest([obs1, obs2]).subscribe(([letter, number]) => {
      output.innerHTML += `combineLatest: ${letter}${number}<br>`;
    });

    // merge() - merges multiple observables
    const fast = interval(1000).pipe(map(x => `Fast: ${x}`), take(3));
    const slow = interval(2000).pipe(map(x => `Slow: ${x}`), take(2));
    
    merge(fast, slow).subscribe(value => {
      output.innerHTML += `merge: ${value}<br>`;
    });

    // zip() - combines values at same index
    const letters = of('A', 'B', 'C', 'D');
    const numbers = of(1, 2, 3);
    
    zip(letters, numbers).subscribe(([letter, number]) => {
      output.innerHTML += `zip: ${letter}${number}<br>`;
    });
  }

  demonstrateErrorHandling(): void {
    const output = this.errorOutput.nativeElement;
    output.innerHTML = '';

    // catchError() - handles errors
    const errorObservable = new Observable(observer => {
      observer.next(1);
      observer.next(2);
      observer.error('Something went wrong!');
      observer.next(3); // This won't be emitted
    });

    errorObservable.pipe(
      catchError(error => {
        output.innerHTML += `Caught error: ${error}<br>`;
        return of('Error handled', 'Continuing...');
      })
    ).subscribe(value => {
      output.innerHTML += `Value: ${value}<br>`;
    });

    // retry() - retries on error
    let attemptCount = 0;
    const retryObservable = new Observable(observer => {
      attemptCount++;
      output.innerHTML += `Attempt ${attemptCount}<br>`;
      
      if (attemptCount < 3) {
        observer.error(`Failed attempt ${attemptCount}`);
      } else {
        observer.next('Success!');
        observer.complete();
      }
    });

    retryObservable.pipe(
      retry(2)
    ).subscribe({
      next: value => output.innerHTML += `Result: ${value}<br>`,
      error: error => output.innerHTML += `Final error: ${error}<br>`
    });
  }

  demonstrateUtilityOperators(): void {
    const output = this.utilityOutput.nativeElement;
    output.innerHTML = '';

    // tap() - performs side effects
    of(1, 2, 3, 4, 5).pipe(
      tap(value => output.innerHTML += `tap (side effect): Processing ${value}<br>`),
      map(x => x * 2),
      tap(value => output.innerHTML += `tap (after map): Result ${value}<br>`)
    ).subscribe(value => {
      output.innerHTML += `Final value: ${value}<br>`;
    });

    // delay() - delays emission
    of('Delayed message').pipe(
      delay(1000)
    ).subscribe(value => {
      output.innerHTML += `${value} (after 1 second)<br>`;
    });

    // timeout() - throws error if no emission within time
    timer(2000).pipe(
      timeout(1500),
      catchError(error => {
        output.innerHTML += `Timeout error: ${error.name}<br>`;
        return of('Timeout handled');
      })
    ).subscribe(value => {
      output.innerHTML += `Timeout result: ${value}<br>`;
    });
  }
}
```
