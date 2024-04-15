---
description: Because React doesn’t own reactivity in the front-end space
---

# Functional Reactive Programming in Angular with RxJS

<figure><img src="../.gitbook/assets/1_JgR_Ctepe7D_ceU8IvrUUA.png" alt=""><figcaption></figcaption></figure>

Alongside React and Vue, **Angular** is considered to be one of the top JS UI frameworks. However, it differentiates itself through a couple of factors. Unlike the other top 2, it’s a full-blown framework, coming with everything you’d want a framework to have. It also has a unique architecture and ideology. This can be seen in its primary language being TypeScript instead of JavaScript, its heavy use of decorators, and its noticeable influence of the **Functional Reactive Programming** paradigm and adoption of many of its patterns.

## Functional Reactive Programming <a href="#ff56" id="ff56"></a>

Functional Reactive Programming (FRP) focuses on the concept of **streams** and how they can be used to handle both synchronous and asynchronous data to build applications in a declarative manner. It’s a combination of Reactive Programming, which focuses on over-time change management and async data flows, and Functional Programming, which provides ways to control it.

In Angular, FRP is used mostly through **Observables**. These, in turn, make for an interface for handling async operations such as AJAX requests or user input events. All of that and more is implemented with the **RxJS** library.

## What is RxJS? <a href="#id-2bdd" id="id-2bdd"></a>

The [RxJS](https://rxjs.dev/) (Reactive Extensions for JavaScript) library provides an implementation of Observables for JavaScript, alongside other “satellite” types and operators to work with them. All these features make for a solid toolkit for dealing with async operations in a functional and reactive manner.

Overall, it’s **the library** to go to whenever you’re dealing with FRP in JS and any other language supported by the [ReactiveX](http://reactivex.io/) project. That’s why Angular, and many other projects, choose it to be their observable implementation and form their foundation for a clean FRP codebase.

## Observables <a href="#id-3623" id="id-3623"></a>

To properly understand RxJS and FRP in general, we have to start with the concept of **observables**.

Behind every observable, there’s a **stream** that represents a sequence of values spread over time. Examples of such sequences include simple arbitrary data, user input events, fetched data, timeouts, intervals, etc. Essentially everything async, even more so if it repeats over time.

Observables play the role of functional wrappers around such streams. They provide you a way to subscribe to and unsubscribe from the stream (to listen for incoming values) and APIs to transform the incoming data to your needs.

### Creating observables <a href="#id-5ee0" id="id-5ee0"></a>

When creating an observable, it’s important to remember that at its core, it’s just a function binding an **observer** (listening for changes) to the **producer** (sourcing values).

Take a look at how we can create a basic observable from an interval that will emit increasing values every 1s:

```javascript
import { Observable } from "rxjs";

const observable$ = new Observable((subscriber) => {
  let count = 0;
  const interval = setInterval(() => {
    subscriber.next(count);
    count++;
  }, 1000);

  return () => {
    clearInterval(interval);
  };
});
```

You can see that our observable is created with the `Observable` interface from RxJS. Inside the function, we access the subscriber and control the notification flow through the methods like `next()`. The returned callback is meant for disposing of the observable - a perfect place to clear the running interval.

All that is assigned to a variable ending with `$` - not a requirement, but a common naming convention for observables.

### Creation functions <a href="#e60e" id="e60e"></a>

The code is pretty clear already, but why write so much if we can just use one of RxJS’s creation functions to do it faster? Check it out:

```javascript
import { interval } from "rxjs";

const observable$ = interval(1000);
```

Short and simple. RxJS is full of such functions. Let me show you some examples:

```javascript
import { from, fromEvent, of } from "rxjs";
import { fromFetch } from "rxjs/fetch";

from(fetch("/example.json"));
from([1, 2, 3, 4]);
fromEvent(document.getElementById("btn"), "click");
of(1, 2, 3, 4);
fromFetch("/example.json");
```

Using the `from` function, we can create an observable from an array-like, iterable, observable, or promise value.

`of` seems fairly similar, though a bit different and more limited. It’ll also let you create observable from arbitrary values, but this time, each argument will become a separate item in the resulting sequence.

So, `of([1,2,3])` will result in an observable sequence of 1 item (`[1,2,3]`), while `from([1,2,3])` will result in a sequence of 3 (`1`, `2`, `3`). Keep in mind that the same applies for Array-like strings (e.g. `”test”` vs. `”t”`, `”e”`, `”s”`, `”t”`).

`fromEvent()` comes closer to the UI, as it creates an observable from **UI events**. Provide the target with an event name, and you’re good to go!

Last but not least, `fromFetch()` provides a shortcut for creating observable from `fetch()` directly. That’s simpler and cleaner than using `from(fetch())`.

RxJS is full of functions like `from()`, `of()`, or `fromFetch()`. It provides you with multiple ways of doing the same thing, where each one of them is good for different scenarios. It’s like Lodash for observables - very helpful, with tons of choice going around.

But if an observable is just a function binding producer with an observer, then none of the observables we’ve already created do anything. We’ve got the producer and wrapping observable, but without the observer, it’s useless. Let’s see how we can fix this while learning about observers and **subscriptions** along the way.

### Subscriptions <a href="#id-40c5" id="id-40c5"></a>

Let’s come back to our simple `interval()` example, and see how we can **subscribe** to it to receive updates.

```javascript
import { interval } from "rxjs";

const observable$ = interval(1000);
const subscription = observable$.subscribe((value) => {
  console.log(value);
});
```

As you can see, subscribing to an observable is really easy. Just pass your observer to the `subscribe()` call, and you’re done! The call will return a subscription object to represent the execution of the observable and control the subscription.

**Observers**

Now, it’s important to note that an **observer** is an object — a set of callbacks — rather than a singular function. The above snippet represents a common shortcut to handle only one type of notification (`next`) instead of them all.

```javascript
// ...

// next, error, and complete callbacks as separate arguments
observable$.subscribe(
  (value) => {
    console.log(value);
  },
  (error) => {
    console.error(error);
  },
  () => {
    console.log("complete");
  }
);
// or
observable$.subscribe({
  next(value) {
    console.log(value);
  },
  error(error) {
    console.error(error);
  },
  complete() {
    console.log("complete");
  },
});
```

None of the mentioned callbacks are required, though you’ll most certainly want at least one.

Also, callback names (`next`, `error`, and `complete`) are equivalent to the methods you use to control notifications flow from the observable side. In our custom interval observable, we’ve used only `next()`, but `error()` and `complete()` are also available, alongside [other methods](https://rxjs.dev/api/index/class/Subscriber).

**Unsubscribe**

The returned subscription object (the result of a `subscribe()` call) is handy for controlling the subscription - most importantly - unsubscribing from it.

Having access to the subscription object, we can **unsubscribe** through a simple `unsubscribe()` call.

```javascript
subscription.unsubscribe();
```

Unsubscribing is important, as it manages the proper execution of the observable and the dispose function calls to prevent **memory leaks**. So whenever you’re done using your subscription, remember to call `unsubscribe()`.

**Multiple subscriptions**

One more thing related to subscriptions worth discussing is what happens when you subscribe to the same observable more than 1 time?

```javascript
// ...

const observable$ = interval(1000);

observable$.subscribe((value) => {
  console.log("Subscription A", value);
});
setTimeout(() => {
  observable$.subscribe((value) => {
    console.log("Subscription B", value);
  });
}, 2000);
/* Console output:
    Subscription A 0
    Subscription A 1
    Subscription A 2
    Subscription B 0
    Subscription A 3
    Subscription B 1
    ...
 */
```

Notice that we’ve got different values from both subscriptions even though we’ve subscribed to the same observable. Both of them have separate intervals, which can be seen in the output — especially with the added `setTimeout()` call.

What you see is a result of our observable being **cold** — let’s discuss what it means.

### Hot or cold <a href="#id-529c" id="id-529c"></a>

To determine whether an observable is hot or cold, we have to look at how it handles its **producer**.

Being the source of the observable’s data, the producer can be created either _“inside”_ or _“outside”_ the observable.

You can see this clearly in our example of creating the interval observable from scratch.

```javascript
import { Observable } from "rxjs";

const observable$ = new Observable((subscriber) => {
  let count = 0;
  const interval = setInterval(() => {
    subscriber.next(count);
    count++;
  }, 1000);

  return () => {
    clearInterval(interval);
  };
});
```

Here, you can think of a producer as a combination of the interval and `count` variable. It’s clearly created inside our observable’s “blueprint” function, making the observable cold.

This brings along several interesting properties:

* A new producer is created for every subscription;
* For a new subscription, the same sequence of values will be returned;
* Values start being emitted only after the first subscription (as no producer exists before that).

Because of those properties, cold observable are desirable for some scenarios and undesirable for others. E.g., in the interval example above, the cold approach is usually the one you’ll want.

However, when we’re considering observables wrapping user input events or AJAX requests, we’ll be respectively forced to use hot observables or be very careful not to cause unnecessary requests with cold observables. There’s also a potential for memory leaks, when we forget about unused, running observable (especially a cold one).

To better understand hot observables and their properties, let’s make some changes to our interval observable to make it **hot**.

```javascript
import { Observable } from "rxjs";

let count = 0;

const intervalCallbacks = [];
setInterval(() => {
  intervalCallbacks.forEach((callback) => callback(count));
  count++;
}, 1000);
const observable$ = new Observable((subscriber) => {
  const callback = (count) => {
    subscriber.next(count);
  };
  intervalCallbacks.push(callback);

  return () => {
    intervalCallbacks.splice(intervalCallbacks.indexOf(callback), 1);
  };
});
```

With the new hot interval observable above, all subscribers will receive the same values no matter when they subscribe. However, that’s not all. Hot observables also have some other properties:

* There’s a single, existing producer to handle all subscriptions
* The producer generates values even if there’s no subscriber.

**Built-in creation functions**

Knowing the difference between cold and hot observables becomes even more important when considering different creation functions and other sources of observables beyond our direct control.

Generally, most questionable observable sources should document whether they’re hot or cold, but if not, then a rule of thumb would be:

> _Everything is cold, unless it makes real sense to be otherwise._

And so, all of the creation functions we’ve covered, but `fromEvent()` are cold. It makes sense, as you wouldn’t make user input events wait until you subscribe to the observable.

Another important thing to notice is in the `fromFetch()` and other HTTP request-related observables like those from Angular’s `HttpClient`. They’re all **cold**, and so, even though you can manage your requests easily with them, you still have to watch yourself, as every subscription will result in **another request**.

There is a way to make cold observable hot, and we’ll cover it in a bit, but first, let’s talk about how you can manage your subscriptions even better.

### AsyncPipe <a href="#id-4e8d" id="id-4e8d"></a>

We’ve already covered how you can subscribe to an observable and how you can manage the subscription with the `unsubscribe()` method on the subscription object, or, e.g., `complete()` inside the observable’s “blueprint” function.

However, in Angular, there’s one more way to manage your subscriptions that you should know about, and that’s [**AsyncPipe**](https://angular.io/api/common/AsyncPipe). It allows you to subscribe to an observable right from the Angular template. More than that, it’ll automatically call `unsubscribe()` when your component will be destroyed, making subscription management and memory leak prevention that much easier. It’ll also automatically use the latest value from the stream, update the view as needed, and even resubscribe to a new observable if that’s necessary.

As for the usage, AsyncPipe has a simple form of `| async`, placed right after the observable (or promise) of choice. In the following example, we use it to access `products$` observable’s data, loop through it, and list the results.

```javascript
// ...

@Component({
  template: `<ul>
    <li *ngFor="let product of products$ | async">{{ product.name }}</li>
  </ul>`,
  // ...
})
export class AsyncObservablePipeComponent {
  products$: Observable<Product[]> = this.productsService.getProducts();
  // ...
}
```

## Operators <a href="#ff0c" id="ff0c"></a>

So we’ve got the basics of observables pretty well covered. We know how to create them, subscribe/unsubscribe from them, and generally control the notification flow.

Now it’s time to talk about **operators** — the **manipulation APIs** which make the “Functional” in FRP really shine.

Operators are the bread and butter of the RxJS library. Whereas the `Observable` interface forms a solid foundation, operators are how the magic happens. They allow you to manipulate your collections through composable code in a declarative manner.

The fun fact is, we’ve already got to know some operators. The so-called _“creation functions”_ are really one of two types of operators — the **creation operators**.

Now we’ll be discovering the second type — the **pipable operators** — functions that you can pipe to observables to create new, altered observables with desired behaviors.

### Pipe method <a href="#id-1f98" id="id-1f98"></a>

There are two ways to use the operator. The first one is just to call it and pass an observable to it. Given an operator named `operator()`, it would look like this:

```javascript
const newObservable$ = operator()(observable$);
```

Notice the first pairs of rounded brackets. It’s used to construct/configure the operator. If any arguments are needed, they’ll go here. If not — the call stays for API consistency.

However, given the composable nature of operators, you can see how the above syntax could quickly become unreadable due to deep call nesting.

That’s where the `Observable`’s `pipe()` method comes in. It allows you to **pipe multiple operators** with clean, readable syntax.

```javascript
const newObservable$ = observable$.pipe(
  operator(),
  operatorA(),
  operatorB(),
  operatorC(),
  operatorD()
);
```

The `pipe()` syntax is so preferable, in fact, that it’s a **recommended practice** to use it even for single operators.

### Common operators <a href="#id-24c1" id="id-24c1"></a>

Now that we know how to pipe operators, it’s time to learn some of them. RxJS provides [so](https://rxjs-dev.firebaseapp.com/api/operators/) [many operators](https://rxjs.dev/api/operators/) that it’s impossible to cover them all in a single blog post. Instead, we’ll only cover some to give you an example.

Like arrays, observables and underlying streams represent sequences of data — just asynchronously. That’s why many operators share their use-cases and naming with array methods. So we’ve got `filter()`, `map()`, `every()`, `find()`, `reduce()`, and many more.

As for some usage examples:

```javascript
import { of } from "rxjs";
import { every, filter, find, map, reduce } from "rxjs/operators";

const observable$ = of(1, 2, 3, 4, 5);

// Filter for multiples of 2
const filteredObservable$ = observable$.pipe(
  filter((value) => value % 2 === 0)
); // Potential output: 2, 4

// Other examples

// Map to the power of 2
observable$.pipe(map((value) => value * value)); // 1, 4, 9, 16, 25
// Is every number greater than 1
observable$.pipe(every((value) => value > 1)); // false
// Find 4
observable$.pipe(find((value) => value === 4)); // 4
// Sum all values
observable$.pipe(reduce((accumulator, value) => accumulator + value)); // 15

// Cold observables output only when subscribed to
filteredObservable$.subscribe((value) => {
  console.log(value);
});
```

Notice how operators are imported from a separate module — `rxjs/operators` - likely for the sake of organization as there are so many!

As for the operators themselves — you can see that the way I’ve used them heavily resembles the use of array methods — especially with the numeric observable sequence.

However, the magic of observables is that it doesn’t have to be synchronous numeric data you’re dealing with, nor that you have to pipe only a single operator at a time. You can mix and match and do all kinds of complex async or sync operations in a clean FRP manner.

Take a look at the following example, where we process the output from async fetch request directly:

```javascript
import { fromFetch } from "rxjs/fetch";
import { concatAll, filter, map } from "rxjs/operators";

const observable$ = fromFetch("/example.json", {
  selector(response) {
    return response.json();
  },
})
  .pipe(
    concatAll(),
    filter((value) => {
      value.experience >= 10;
    }),
    map((value) => value.name)
  )
  .subscribe((value) => {
    console.log(value);
  });
```

To `fromFetch()`, aside from the request URL, we pass config object with `selector()` callback for “unwrapping” the fetched data.

Then, presuming the JSON data is an array of objects describing users, we process it with several operators. First, `concatAll()` “splits” the input array into separate sequence items, to then be processed through `filter()` and `map()`. All in a clean, functional way.

### Operators in UI <a href="#f259" id="f259"></a>

Apart from processing data, observables and operators can also be useful in the UI — especially when dealing with user input events.

Consider the following example, where we use `fromEvent()` creation function and `filter()` operator, to determine the number of clicks on an Angular button:

```javascript
import {
  AfterViewInit,
  Component,
  ElementRef,
  EventEmitter,
  Input,
  Output,
  ViewChild,
} from "@angular/core";
import { fromEvent } from "rxjs";
import { filter } from "rxjs/operators";

@Component({
  selector: "btn",
  template: `
    <button #btn>Button</button>
  `,
})
export class ButtonComponent implements AfterViewInit {
  @ViewChild("btn", { static: true }) el: ElementRef<HTMLButtonElement>;
  @Output("multiClick") multiClick = new EventEmitter<MouseEvent>();
  @Input("requiredClicks") requiredClicks: number;

  ngAfterViewInit() {
    fromEvent<MouseEvent>(this.el.nativeElement, "click")
      .pipe(filter((value) => value.detail === this.requiredClicks))
      .subscribe((event) => {
        this.multiClick.emit(event);
      });
  }
}
```

We first use `ElementRef` and `@ViewChild` decorator to gain access to the DOM button element. Then, in the `ngAfterViewInit()` hook, when the reference is ready, we access it and start listening for click events with `fromEvent()`. By piping the observable through `filter()`, we check if the number of clicks (under `detail` property) matches the required number of clicks. Lastly, the resulting observable is subscribed to and emits events whenever necessary.

The code is functional, simple and very readable. Observers integrate well with Angular’s components and overall structure.

As for an example usage of this component:

```html
<btn [requiredClicks]="3" (multiClick)="handleTripleClick($event)"></btn>
```

### Converting hot observable to cold <a href="#ff87" id="ff87"></a>

When discussing hot and cold observables, I told you there’s a way to convert a cold observable to a hot one. That’s possible thanks to some complex transformations, which we can benefit from through a simple `share()` operator.

```javascript
import { interval } from "rxjs";
import { share } from "rxjs/operators";

const observable$ = interval(1000).pipe(share());

observable$.subscribe((value) => {
  console.log("Subscription A", value);
});
setTimeout(() => {
  observable$.subscribe((value) => {
    console.log("Subscription B", value);
  });
}, 2000);
/* Console output:
    Subscription A 0
    Subscription A 1
    Subscription A 2
    Subscription B 2
    Subscription A 3
    Subscription B 3
    ...
 */
```

The example from above is the same one we’ve analyzed when discussing multiple subscriptions to one observable. Notice the small difference — piping of the `share()` operator.

Thanks to this little change, all our subscriptions will now get the same synchronized values, no matter when they subscribe. We’ve just made a hot observable out of a cold one.

## Error handling <a href="#id-83ba" id="id-83ba"></a>

With the knowledge of operators, we can now discuss how we should handle errors in observables.

We’ve already got a glimpse of that with observers in `subscribe()` method and their `error` callback. However, this way of handling errors has some big disadvantages. First off, it’s **finite**, meaning you can’t easily recover from error even if you want to. And also, it goes against Angular’s philosophy of **separation of concerns**.

There’s a special operator that can be used for handling errors and solves both of these issues — `catchError()`. To show its use, let’s bring up the `fromFetch()` creation function:

```javascript
import { fromFetch } from "rxjs/fetch";
import { catchError, retry } from "rxjs/operators";

const observable$ = fromFetch("/example.json").pipe(
  retry(3),
  catchError(() => {
    return Promise.resolve([]);
  })
);ja
```

We pipe both `retry()` and `catchError()` to our `fromFetch()` observable. `retry()` is another useful operator that will resubscribe to the source observer upon error and retry running it up to the specified number of resubscriptions.

`catchError()` gets handed a callback, that upon receiving the caught error, and source observable decides whether to throw this or new error, return the source observable and try again, or return any different kind of value.

In the example above, I return `Promise.resolve([])` as the _“fallback value”_ upon error. This will, in turn, result in an empty array being retrieved on the subscription’s end when a fetching error happens, as `fromFetch()` will automatically unwrap the promise.

This short overview should provide you with a basic notion of what `catchError()` and other error-handling operators are all about. They might seem a bit unnecessary right here, but in more complex scenarios with nested observables, when used alongside AsyncPipe, or in other edge cases, they become **invaluable**.

## Summary <a href="#bafa" id="bafa"></a>

This detailed primer on RxJS and FRP in Angular should leave you with a solid understanding of the basics. It’s enough to start experimenting on your own or go to learn more advanced topics, such as observable nesting, [Subjects](https://rxjs.dev/guide/subject), [Schedulers](https://rxjs.dev/guide/scheduler), or complex operators.

RxJS and Angular form a great combo that can lead to simple, clean, and enjoyable code when used correctly. Sure, it requires learning a few new concepts, but this knowledge then pays off in better code and a better understanding of it.
