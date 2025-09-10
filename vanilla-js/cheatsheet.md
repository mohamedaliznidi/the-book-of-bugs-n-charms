---
description: >-
  A comprehensive JavaScript cheat sheet covering essential syntax, methods,
  modern ES6+ features, and advanced concepts for quick reference.

theme: "magic"
---

# The JavaScript Sorcery Cheat Sheet Grimoire

## The Ancient Knowledge

This comprehensive cheat sheet provides a quick reference for JavaScript syntax, methods, and modern features through mystical programming spells. From basic fundamentals to advanced ES6+ concepts, this guide covers everything developers need for efficient JavaScript development through powerful coding enchantments.

## Variables and Data Type Enchantments

### Variable Declaration Spells

```javascript
// var - function scoped, hoisted
var name = 'John';

// let - block scoped, not hoisted
let age = 25;

// const - block scoped, immutable reference
const PI = 3.14159;
const user = { name: 'Alice' }; // Object properties can still be modified
```

### Primitive Data Type Mysticism

```javascript
// String
let message = "Hello World";
let template = `Hello ${name}`; // Template literals

// Number
let integer = 42;
let float = 3.14;
let scientific = 2.5e6; // 2,500,000

// Boolean
let isActive = true;
let isComplete = false;

// Undefined
let undefinedVar;
console.log(undefinedVar); // undefined

// Null
let emptyValue = null;

// Symbol (ES6)
let symbol = Symbol('id');
let uniqueSymbol = Symbol.for('global');

// BigInt (ES2020)
let bigNumber = 123456789012345678901234567890n;
```

### Type Checking Divination

```javascript
typeof 'hello';        // 'string'
typeof 42;             // 'number'
typeof true;           // 'boolean'
typeof undefined;      // 'undefined'
typeof null;           // 'object' (known quirk)
typeof {};             // 'object'
typeof [];             // 'object'
typeof function() {};  // 'function'

// Better type checking
Array.isArray([]);           // true
Number.isInteger(42);        // true
Number.isNaN(NaN);          // true
Object.prototype.toString.call([]); // '[object Array]'
```

## Operator Enchantments

### Arithmetic Operator Spells

```javascript
let a = 10, b = 3;

a + b;    // 13 (Addition)
a - b;    // 7  (Subtraction)
a * b;    // 30 (Multiplication)
a / b;    // 3.333... (Division)
a % b;    // 1  (Modulus)
a ** b;   // 1000 (Exponentiation - ES2016)

// Increment/Decrement
++a;      // Pre-increment
a++;      // Post-increment
--a;      // Pre-decrement
a--;      // Post-decrement
```

### Comparison Operator Divination

```javascript
// Equality
5 == '5';   // true (loose equality)
5 === '5';  // false (strict equality)
5 != '5';   // false (loose inequality)
5 !== '5';  // true (strict inequality)

// Relational
5 > 3;      // true
5 < 3;      // false
5 >= 5;     // true
5 <= 3;     // false
```

### Logical Operator Enchantments

```javascript
// AND (&&)
true && true;   // true
true && false;  // false

// OR (||)
true || false;  // true
false || false; // false

// NOT (!)
!true;          // false
!false;         // true

// Nullish Coalescing (ES2020)
null ?? 'default';      // 'default'
undefined ?? 'default'; // 'default'
0 ?? 'default';         // 0
'' ?? 'default';        // ''

// Optional Chaining (ES2020)
user?.address?.street;  // Safe property access
user?.getName?.();      // Safe method call
```

## String Manipulation Sorcery

### String Method Enchantments

```javascript
let str = 'Hello World';

// Length and access
str.length;              // 11
str[0];                  // 'H'
str.charAt(0);           // 'H'
str.charCodeAt(0);       // 72

// Search and test
str.indexOf('World');    // 6
str.lastIndexOf('l');    // 9
str.includes('World');   // true
str.startsWith('Hello'); // true
str.endsWith('World');   // true
str.search(/world/i);    // 6 (regex search)

// Extract
str.slice(0, 5);         // 'Hello'
str.substring(0, 5);     // 'Hello'
str.substr(0, 5);        // 'Hello' (deprecated)

// Transform
str.toLowerCase();       // 'hello world'
str.toUpperCase();       // 'HELLO WORLD'
str.trim();              // Remove whitespace
str.trimStart();         // Remove leading whitespace
str.trimEnd();           // Remove trailing whitespace

// Replace
str.replace('World', 'JavaScript');     // 'Hello JavaScript'
str.replaceAll('l', 'L');              // 'HeLLo WorLd'

// Split and join
str.split(' ');          // ['Hello', 'World']
str.split('');           // ['H', 'e', 'l', 'l', 'o', ' ', 'W', 'o', 'r', 'l', 'd']

// Repeat and pad
str.repeat(2);           // 'Hello WorldHello World'
str.padStart(15, '*');   // '****Hello World'
str.padEnd(15, '*');     // 'Hello World****'
```

### Template Literal Mysticism

```javascript
const name = 'Alice';
const age = 30;

// Multi-line strings
const multiLine = `
  This is a
  multi-line
  string
`;

// Expression interpolation
const greeting = `Hello, ${name}! You are ${age} years old.`;

// Tagged templates
function highlight(strings, ...values) {
  return strings.reduce((result, string, i) => {
    return result + string + (values[i] ? `<mark>${values[i]}</mark>` : '');
  }, '');
}

const highlighted = highlight`Hello ${name}, you are ${age} years old!`;

## Array Manipulation Enchantments

### Array Creation and Access Spells

```javascript
// Array creation
const fruits = ['apple', 'banana', 'orange'];
const numbers = new Array(1, 2, 3, 4, 5);
const empty = new Array(5); // Creates array with 5 empty slots

// Access and modification
fruits[0];           // 'apple'
fruits.length;       // 3
fruits[fruits.length - 1]; // 'orange' (last element)

// Destructuring
const [first, second, ...rest] = fruits;
// first = 'apple', second = 'banana', rest = ['orange']
```

### Array Methods - Mutating Sorcery

```javascript
let arr = [1, 2, 3];

// Add/remove elements
arr.push(4);         // [1, 2, 3, 4] - add to end
arr.pop();           // [1, 2, 3] - remove from end
arr.unshift(0);      // [0, 1, 2, 3] - add to beginning
arr.shift();         // [1, 2, 3] - remove from beginning

// Splice - add/remove at any position
arr.splice(1, 1);    // [1, 3] - remove 1 element at index 1
arr.splice(1, 0, 2); // [1, 2, 3] - insert 2 at index 1
arr.splice(1, 1, 'two'); // [1, 'two', 3] - replace element at index 1

// Sort and reverse
arr.sort();          // Sort alphabetically
arr.sort((a, b) => a - b); // Sort numerically ascending
arr.reverse();       // Reverse array order

// Fill
arr.fill(0);         // Fill all elements with 0
arr.fill(1, 2, 4);   // Fill elements from index 2 to 4 with 1
```

### Array Methods - Non-mutating Preservation Spells

```javascript
const numbers = [1, 2, 3, 4, 5];

// Transform
const doubled = numbers.map(n => n * 2);        // [2, 4, 6, 8, 10]
const strings = numbers.map(String);            // ['1', '2', '3', '4', '5']

// Filter
const evens = numbers.filter(n => n % 2 === 0); // [2, 4]
const odds = numbers.filter(n => n % 2 !== 0);  // [1, 3, 5]

// Reduce
const sum = numbers.reduce((acc, n) => acc + n, 0);     // 15
const product = numbers.reduce((acc, n) => acc * n, 1); // 120

// Find
const found = numbers.find(n => n > 3);         // 4
const foundIndex = numbers.findIndex(n => n > 3); // 3

// Test
const hasEven = numbers.some(n => n % 2 === 0); // true
const allPositive = numbers.every(n => n > 0);  // true

// Join and slice
const joined = numbers.join('-');               // '1-2-3-4-5'
const sliced = numbers.slice(1, 3);            // [2, 3]

// Includes and indexOf
numbers.includes(3);     // true
numbers.indexOf(3);      // 2
numbers.lastIndexOf(3);  // 2

// Flat and flatMap (ES2019)
const nested = [[1, 2], [3, 4], [5]];
nested.flat();           // [1, 2, 3, 4, 5]
nested.flatMap(arr => arr.map(n => n * 2)); // [2, 4, 6, 8, 10]
```

### Advanced Array Method Sorcery

```javascript
// Array.from - create array from iterable
Array.from('hello');           // ['h', 'e', 'l', 'l', 'o']
Array.from({length: 3}, (_, i) => i); // [0, 1, 2]

// Array.of - create array from arguments
Array.of(1, 2, 3);            // [1, 2, 3]

// Spread operator
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2]; // [1, 2, 3, 4, 5, 6]

// Array destructuring with rest
const [head, ...tail] = [1, 2, 3, 4]; // head = 1, tail = [2, 3, 4]
```

## Object Manifestation Sorcery

### Object Creation and Access Rituals

```javascript
// Object literal
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

// Constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}
const john = new Person('John', 30);

// Object.create
const personPrototype = {
  greet() {
    return `Hello, I'm ${this.name}`;
  }
};
const alice = Object.create(personPrototype);
alice.name = 'Alice';

// Property access
person.name;           // 'John'
person['name'];        // 'John'
person['full-name'];   // For properties with special characters

// Dynamic property names
const prop = 'age';
person[prop];          // 30

// Computed property names (ES6)
const key = 'dynamicKey';
const obj = {
  [key]: 'value',
  [`${key}2`]: 'value2'
};
```

### Object Method Enchantments

```javascript
const user = { name: 'Alice', age: 25, city: 'Boston' };

// Keys, values, entries
Object.keys(user);     // ['name', 'age', 'city']
Object.values(user);   // ['Alice', 25, 'Boston']
Object.entries(user);  // [['name', 'Alice'], ['age', 25], ['city', 'Boston']]

// Object.assign - shallow copy/merge
const copy = Object.assign({}, user);
const merged = Object.assign({}, user, { country: 'USA' });

// Object.freeze, seal, preventExtensions
Object.freeze(user);           // Immutable
Object.seal(user);             // No add/delete, can modify
Object.preventExtensions(user); // No add, can delete/modify

// Property descriptors
Object.defineProperty(user, 'id', {
  value: 123,
  writable: false,
  enumerable: false,
  configurable: false
});

Object.getOwnPropertyDescriptor(user, 'name');
Object.getOwnPropertyDescriptors(user);

// Prototype methods
Object.getPrototypeOf(user);
Object.setPrototypeOf(user, personPrototype);
user.hasOwnProperty('name');   // true
'name' in user;                // true

// Object.fromEntries (ES2019)
const entries = [['name', 'Bob'], ['age', 35]];
const fromEntries = Object.fromEntries(entries); // {name: 'Bob', age: 35}
```

### Object Destructuring Alchemy

```javascript
const person = { name: 'John', age: 30, city: 'NYC', country: 'USA' };

// Basic destructuring
const { name, age } = person;

// Renaming variables
const { name: fullName, age: years } = person;

// Default values
const { name, age, occupation = 'Unknown' } = person;

// Rest operator
const { name, ...details } = person; // details = { age: 30, city: 'NYC', country: 'USA' }

// Nested destructuring
const user = {
  id: 1,
  profile: {
    name: 'Alice',
    settings: {
      theme: 'dark'
    }
  }
};
const { profile: { name, settings: { theme } } } = user;

## Function Incantations

### Function Declaration and Expression Spells

```javascript
// Function declaration (hoisted)
function greet(name) {
  return `Hello, ${name}!`;
}

// Function expression (not hoisted)
const greet2 = function(name) {
  return `Hello, ${name}!`;
};

// Arrow functions (ES6)
const greet3 = (name) => `Hello, ${name}!`;
const greet4 = name => `Hello, ${name}!`; // Single parameter
const add = (a, b) => a + b;              // Single expression
const multiply = (a, b) => {              // Block body
  const result = a * b;
  return result;
};

// IIFE (Immediately Invoked Function Expression)
(function() {
  console.log('IIFE executed');
})();

// Named function expression
const factorial = function fact(n) {
  return n <= 1 ? 1 : n * fact(n - 1);
};
```

### Function Parameter Mysticism

```javascript
// Default parameters
function greet(name = 'World', punctuation = '!') {
  return `Hello, ${name}${punctuation}`;
}

// Rest parameters
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

// Destructuring parameters
function createUser({name, age, email = 'not provided'}) {
  return { name, age, email };
}

// Mixed parameters
function processData(required, optional = 'default', ...rest) {
  return { required, optional, rest };
}
```

### Higher-Order Function Sorcery

```javascript
// Function that returns a function
function createMultiplier(factor) {
  return function(number) {
    return number * factor;
  };
}
const double = createMultiplier(2);
const triple = createMultiplier(3);

// Function that takes a function as parameter
function applyOperation(arr, operation) {
  return arr.map(operation);
}
const numbers = [1, 2, 3, 4];
const doubled = applyOperation(numbers, x => x * 2);

// Currying
const curry = (fn) => {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }
    return function(...nextArgs) {
      return curried.apply(this, args.concat(nextArgs));
    };
  };
};

const add3 = (a, b, c) => a + b + c;
const curriedAdd = curry(add3);
const result = curriedAdd(1)(2)(3); // 6
```

### Function Method Enchantments

```javascript
function greet(greeting, punctuation) {
  return `${greeting}, ${this.name}${punctuation}`;
}

const person = { name: 'Alice' };

// call - invoke with specific 'this' and arguments
greet.call(person, 'Hello', '!'); // "Hello, Alice!"

// apply - invoke with specific 'this' and arguments array
greet.apply(person, ['Hi', '?']); // "Hi, Alice?"

// bind - create new function with bound 'this'
const boundGreet = greet.bind(person, 'Hey');
boundGreet('!!!'); // "Hey, Alice!!!"
```

## Control Flow Mysticism

### Conditional Statement Divination

```javascript
// if...else
if (condition) {
  // code
} else if (anotherCondition) {
  // code
} else {
  // code
}

// Ternary operator
const result = condition ? 'true value' : 'false value';

// Switch statement
switch (value) {
  case 'option1':
    // code
    break;
  case 'option2':
  case 'option3':
    // code for option2 or option3
    break;
  default:
    // default code
}

// Short-circuit evaluation
const name = user.name || 'Anonymous';
const isLoggedIn = user && user.isAuthenticated;

// Nullish coalescing
const username = user.name ?? 'Guest';
```

### Loop Iteration Spells

```javascript
// for loop
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// for...in (object properties)
const obj = { a: 1, b: 2, c: 3 };
for (const key in obj) {
  console.log(key, obj[key]);
}

// for...of (iterable values)
const arr = [1, 2, 3];
for (const value of arr) {
  console.log(value);
}

// while loop
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}

// do...while loop
let j = 0;
do {
  console.log(j);
  j++;
} while (j < 5);

// Array iteration methods
arr.forEach((value, index) => console.log(index, value));

// Break and continue
for (let i = 0; i < 10; i++) {
  if (i === 3) continue; // Skip iteration
  if (i === 7) break;    // Exit loop
  console.log(i);
}
```

## Error Handling Protective Sorcery

### Try-Catch-Finally Guardian Spells

```javascript
try {
  // Code that might throw an error
  const result = riskyOperation();
  console.log(result);
} catch (error) {
  // Handle the error
  console.error('An error occurred:', error.message);
} finally {
  // Always executed
  console.log('Cleanup code');
}

// Specific error types
try {
  JSON.parse(invalidJson);
} catch (error) {
  if (error instanceof SyntaxError) {
    console.log('Invalid JSON syntax');
  } else if (error instanceof ReferenceError) {
    console.log('Reference error');
  } else {
    console.log('Unknown error:', error);
  }
}
```

### Throwing Error Curse Manifestations

```javascript
// Throw basic error
throw new Error('Something went wrong');

// Throw specific error types
throw new TypeError('Expected a number');
throw new ReferenceError('Variable not defined');
throw new RangeError('Number out of range');

// Custom error class
class CustomError extends Error {
  constructor(message, code) {
    super(message);
    this.name = 'CustomError';
    this.code = code;
  }
}

throw new CustomError('Custom error message', 'ERR_001');

## Asynchronous JavaScript Temporal Sorcery

### Promise Covenant Spells

```javascript
// Creating a promise
const myPromise = new Promise((resolve, reject) => {
  const success = Math.random() > 0.5;
  setTimeout(() => {
    if (success) {
      resolve('Operation successful!');
    } else {
      reject(new Error('Operation failed!'));
    }
  }, 1000);
});

// Using promises
myPromise
  .then(result => console.log(result))
  .catch(error => console.error(error))
  .finally(() => console.log('Promise completed'));

// Promise methods
Promise.resolve('immediate value');
Promise.reject(new Error('immediate error'));

// Promise.all - wait for all promises
Promise.all([promise1, promise2, promise3])
  .then(results => console.log(results))
  .catch(error => console.error('One or more promises failed'));

// Promise.allSettled - wait for all promises (ES2020)
Promise.allSettled([promise1, promise2, promise3])
  .then(results => {
    results.forEach((result, index) => {
      if (result.status === 'fulfilled') {
        console.log(`Promise ${index} succeeded:`, result.value);
      } else {
        console.log(`Promise ${index} failed:`, result.reason);
      }
    });
  });

// Promise.race - first to complete
Promise.race([promise1, promise2, promise3])
  .then(result => console.log('First completed:', result));

// Promise.any - first to succeed (ES2021)
Promise.any([promise1, promise2, promise3])
  .then(result => console.log('First successful:', result))
  .catch(error => console.log('All promises failed'));
```

### Async/Await Temporal Enchantments

```javascript
// Async function
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error fetching data:', error);
    throw error;
  }
}

// Using async function
fetchData()
  .then(data => console.log(data))
  .catch(error => console.error(error));

// Async arrow function
const fetchUser = async (id) => {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
};

// Parallel execution with async/await
async function fetchMultipleData() {
  try {
    // Sequential (slower)
    const user = await fetchUser(1);
    const posts = await fetchPosts(user.id);

    // Parallel (faster)
    const [userData, postsData] = await Promise.all([
      fetchUser(1),
      fetchPosts(1)
    ]);

    return { user: userData, posts: postsData };
  } catch (error) {
    console.error('Error:', error);
  }
}
```

### Callbacks and Event Loop Mysticism

```javascript
// Callback function
function processData(data, callback) {
  setTimeout(() => {
    const processed = data.map(item => item.toUpperCase());
    callback(processed);
  }, 1000);
}

processData(['hello', 'world'], (result) => {
  console.log(result); // ['HELLO', 'WORLD']
});

// setTimeout and setInterval
const timeoutId = setTimeout(() => {
  console.log('Executed after 1 second');
}, 1000);

const intervalId = setInterval(() => {
  console.log('Executed every 2 seconds');
}, 2000);

// Clear timers
clearTimeout(timeoutId);
clearInterval(intervalId);

// setImmediate (Node.js) / setTimeout with 0 delay
setTimeout(() => console.log('Next tick'), 0);
```

## Classes (ES6) Blueprint Sorcery

### Class Declaration Manifestations

```javascript
class Person {
  // Constructor
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  // Instance method
  greet() {
    return `Hello, I'm ${this.name}`;
  }

  // Getter
  get info() {
    return `${this.name} is ${this.age} years old`;
  }

  // Setter
  set age(value) {
    if (value < 0) {
      throw new Error('Age cannot be negative');
    }
    this._age = value;
  }

  get age() {
    return this._age;
  }

  // Static method
  static species() {
    return 'Homo sapiens';
  }

  // Private field (ES2022)
  #privateField = 'secret';

  // Private method (ES2022)
  #privateMethod() {
    return this.#privateField;
  }
}

// Usage
const person = new Person('Alice', 30);
console.log(person.greet());
console.log(person.info);
console.log(Person.species());
```

### Inheritance Bloodline Enchantments

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name); // Call parent constructor
    this.breed = breed;
  }

  speak() {
    return `${this.name} barks`;
  }

  // Method that calls parent method
  describe() {
    return `${super.speak()} and is a ${this.breed}`;
  }
}

const dog = new Dog('Rex', 'German Shepherd');
console.log(dog.speak());     // "Rex barks"
console.log(dog.describe());  // "Rex makes a sound and is a German Shepherd"
```

## Modules (ES6) Grimoire Organization

### Export Sharing Spells

```javascript
// Named exports
export const PI = 3.14159;
export function calculateArea(radius) {
  return PI * radius * radius;
}

export class Circle {
  constructor(radius) {
    this.radius = radius;
  }
}

// Export list
const name = 'Math Utils';
const version = '1.0.0';
export { name, version };

// Export with alias
export { calculateArea as area };

// Default export
export default function multiply(a, b) {
  return a * b;
}

// Mixed exports
export default class Calculator {
  add(a, b) { return a + b; }
}
export const helpers = { format: (n) => n.toFixed(2) };
```

### Import Summoning Rituals

```javascript
// Named imports
import { PI, calculateArea } from './math-utils.js';
import { calculateArea as area } from './math-utils.js';

// Default import
import multiply from './math-utils.js';
import Calculator from './calculator.js';

// Mixed imports
import Calculator, { helpers } from './calculator.js';

// Import all
import * as MathUtils from './math-utils.js';

// Dynamic imports
async function loadModule() {
  const module = await import('./dynamic-module.js');
  module.doSomething();
}

// Import for side effects only
import './polyfills.js';

## Modern JavaScript Feature Enchantments

### Destructuring Assignment Alchemy

```javascript
// Array destructuring
const [a, b, c] = [1, 2, 3];
const [first, , third] = [1, 2, 3]; // Skip elements
const [head, ...tail] = [1, 2, 3, 4]; // Rest elements

// Object destructuring
const { name, age } = { name: 'John', age: 30, city: 'NYC' };
const { name: fullName, age: years } = person; // Rename
const { name, age, country = 'USA' } = person; // Default values

// Nested destructuring
const { address: { street, city } } = user;

// Function parameter destructuring
function greet({ name, age = 25 }) {
  return `Hello ${name}, you are ${age}`;
}
```

### Spread and Rest Operator Mysticism

```javascript
// Spread with arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2]; // [1, 2, 3, 4, 5, 6]
const copy = [...arr1]; // Shallow copy

// Spread with objects
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };
const merged = { ...obj1, ...obj2 }; // { a: 1, b: 2, c: 3, d: 4 }
const updated = { ...obj1, b: 20 }; // Override property

// Rest in function parameters
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

// Rest in destructuring
const [first, ...rest] = [1, 2, 3, 4]; // first = 1, rest = [2, 3, 4]
const { name, ...details } = person; // Extract name, rest in details
```

### Map and Set Collection Enchantments

```javascript
// Map
const map = new Map();
map.set('key1', 'value1');
map.set('key2', 'value2');
map.set(42, 'number key');

console.log(map.get('key1')); // 'value1'
console.log(map.has('key1')); // true
console.log(map.size); // 3

// Iterate over Map
for (const [key, value] of map) {
  console.log(key, value);
}

// WeakMap (garbage collected when keys are no longer referenced)
const weakMap = new WeakMap();
const obj = {};
weakMap.set(obj, 'value');

// Set
const set = new Set([1, 2, 3, 3, 4]); // [1, 2, 3, 4] - duplicates removed
set.add(5);
set.delete(1);
console.log(set.has(2)); // true
console.log(set.size); // 4

// Convert Set to Array
const uniqueArray = [...set];

// WeakSet
const weakSet = new WeakSet();
weakSet.add(obj);
```

### Symbol Mystical Identifiers

```javascript
// Create symbols
const sym1 = Symbol();
const sym2 = Symbol('description');
const sym3 = Symbol('description'); // Different from sym2

// Global symbol registry
const globalSym = Symbol.for('global');
const sameGlobalSym = Symbol.for('global'); // Same as globalSym

// Well-known symbols
const obj = {
  [Symbol.iterator]: function* () {
    yield 1;
    yield 2;
    yield 3;
  }
};

// Use symbols as object keys
const mySymbol = Symbol('myKey');
const object = {
  [mySymbol]: 'value',
  regularKey: 'regular value'
};
```

### Generators and Iterator Sorcery

```javascript
// Generator function
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
  return 'done';
}

const gen = numberGenerator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: 'done', done: true }

// Infinite generator
function* fibonacci() {
  let [a, b] = [0, 1];
  while (true) {
    yield a;
    [a, b] = [b, a + b];
  }
}

// Custom iterator
const range = {
  from: 1,
  to: 5,

  [Symbol.iterator]() {
    return {
      current: this.from,
      last: this.to,

      next() {
        if (this.current <= this.last) {
          return { done: false, value: this.current++ };
        } else {
          return { done: true };
        }
      }
    };
  }
};

for (const num of range) {
  console.log(num); // 1, 2, 3, 4, 5
}
```

## DOM Manipulation Sorcery

### Element Selection Divination

```javascript
// Single element selection
const element = document.getElementById('myId');
const element2 = document.querySelector('.myClass');
const element3 = document.querySelector('#myId');

// Multiple elements selection
const elements = document.getElementsByClassName('myClass');
const elements2 = document.getElementsByTagName('div');
const elements3 = document.querySelectorAll('.myClass');
const elements4 = document.querySelectorAll('div[data-type="example"]');
```

### Element Modification Enchantments

```javascript
// Content manipulation
element.textContent = 'New text content';
element.innerHTML = '<strong>New HTML content</strong>';

// Attribute manipulation
element.setAttribute('data-value', '123');
element.getAttribute('data-value');
element.removeAttribute('data-value');
element.hasAttribute('data-value');

// Class manipulation
element.classList.add('newClass');
element.classList.remove('oldClass');
element.classList.toggle('activeClass');
element.classList.contains('someClass');

// Style manipulation
element.style.color = 'red';
element.style.backgroundColor = 'blue';
element.style.cssText = 'color: red; background: blue;';

// Data attributes
element.dataset.userId = '123';
console.log(element.dataset.userId);
```

### Element Creation and Insertion Conjurations

```javascript
// Create elements
const newDiv = document.createElement('div');
const textNode = document.createTextNode('Hello World');

// Set properties
newDiv.textContent = 'New div content';
newDiv.className = 'new-div';
newDiv.id = 'uniqueId';

// Insert elements
parentElement.appendChild(newDiv);
parentElement.insertBefore(newDiv, referenceElement);
element.insertAdjacentHTML('beforebegin', '<p>Before element</p>');
element.insertAdjacentHTML('afterend', '<p>After element</p>');

// Modern insertion methods
element.before(newDiv);
element.after(newDiv);
element.prepend(newDiv);
element.append(newDiv);

// Remove elements
element.remove();
parentElement.removeChild(element);
```

### Event Handling Oracles

```javascript
// Add event listeners
element.addEventListener('click', handleClick);
element.addEventListener('click', handleClick, { once: true });

function handleClick(event) {
  event.preventDefault(); // Prevent default behavior
  event.stopPropagation(); // Stop event bubbling
  console.log('Element clicked:', event.target);
}

// Remove event listeners
element.removeEventListener('click', handleClick);

// Event delegation
document.addEventListener('click', function(event) {
  if (event.target.matches('.button')) {
    console.log('Button clicked:', event.target);
  }
});

// Custom events
const customEvent = new CustomEvent('myEvent', {
  detail: { message: 'Hello from custom event' }
});
element.dispatchEvent(customEvent);

element.addEventListener('myEvent', function(event) {
  console.log(event.detail.message);
});
```

## Wisdom of the Ancients

### Code Organization Rituals

```javascript
// Use strict mode
'use strict';

// Consistent naming conventions
const userName = 'john_doe';        // camelCase for variables
const MAX_RETRIES = 3;              // UPPER_CASE for constants
class UserManager {}                // PascalCase for classes

// Avoid global variables
(function() {
  // Your code here
})();

// Use modules
export const utils = {
  formatDate: (date) => date.toISOString(),
  validateEmail: (email) => /\S+@\S+\.\S+/.test(email)
};
```

### Performance Enhancement Spells

```javascript
// Use const and let instead of var
const immutableValue = 'constant';
let mutableValue = 'variable';

// Avoid creating functions in loops
// Bad
for (let i = 0; i < items.length; i++) {
  items[i].addEventListener('click', function() {
    console.log(i);
  });
}

// Good
function handleItemClick(index) {
  return function() {
    console.log(index);
  };
}
for (let i = 0; i < items.length; i++) {
  items[i].addEventListener('click', handleItemClick(i));
}

// Use array methods instead of loops when appropriate
const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((acc, n) => acc + n, 0);

// Debounce expensive operations
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

const debouncedSearch = debounce(searchFunction, 300);
```

### Error Prevention Protective Wards

```javascript
// Use optional chaining
const street = user?.address?.street;
const result = api?.getData?.();

// Use nullish coalescing
const name = user.name ?? 'Anonymous';

// Validate function parameters
function divide(a, b) {
  if (typeof a !== 'number' || typeof b !== 'number') {
    throw new TypeError('Both arguments must be numbers');
  }
  if (b === 0) {
    throw new Error('Division by zero is not allowed');
  }
  return a / b;
}

// Use try-catch for async operations
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    return await response.json();
  } catch (error) {
    console.error('Failed to fetch data:', error);
    return null;
  }
}
```

## Common Curses & Their Remedies

### Curse 1: Hoisting Confusion
Variable and function declarations are hoisted, but not their assignments.

**Remedy:**
```javascript
// Use let and const instead of var
// Declare functions before using them
console.log(myVar); // undefined (not ReferenceError)
var myVar = 5;

console.log(myLet); // ReferenceError
let myLet = 5;
```

### Curse 2: 'this' Context Problem Hex
Arrow functions don't have their own 'this' context.

**Remedy:**
```javascript
class MyClass {
  constructor() {
    this.value = 42;
  }

  // Use arrow function to preserve 'this'
  handleClick = () => {
    console.log(this.value); // 42
  }

  // Or bind in constructor
  // this.handleClick = this.handleClick.bind(this);
}
```

### Curse 3: Asynchronous Code Execution Malfunction
Understanding the event loop and promise resolution.

**Remedy:**
```javascript
// Use async/await for cleaner asynchronous code
async function processData() {
  try {
    const data = await fetchData();
    const processed = await processData(data);
    return processed;
  } catch (error) {
    console.error('Error processing data:', error);
  }
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://developer.mozilla.org/en-US/docs/Web/JavaScript" %}

{% embed url="https://javascript.info/" %}

{% embed url="https://tc39.es/ecma262/" %}
```
```
```
```

