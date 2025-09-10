---
description: >-
  Lodash is a modern JavaScript utility library delivering modularity, performance,
  and extras for working with arrays, objects, strings, and functions.

theme: "magic"
---

# The Lodash Utility Sorcery Grimoire

## The Ancient Knowledge

Lodash is a modern JavaScript utility library that provides a comprehensive set of mystical functions for common programming tasks. It offers consistent, modular, performant, and feature-rich utilities for manipulating arrays, objects, strings, functions, and more through powerful data transformation spells. Lodash helps write cleaner, more readable code while handling edge cases and browser compatibility issues with protective enchantments.

## When to Cast These Spells

1. **Data Manipulation Rituals**: Transform, filter, and process arrays and objects efficiently through data alchemy
2. **Functional Programming Sorcery**: Use higher-order functions and composition patterns with mystical function enchantments
3. **Type Checking Divination**: Safely check data types and handle edge cases through protective validation spells
4. **Performance Optimization Magic**: Leverage optimized algorithms for common operations with efficiency enchantments
5. **Cross-browser Compatibility Shields**: Handle browser differences transparently through compatibility protection spells
6. **Code Simplification Alchemy**: Replace verbose native JavaScript with concise utility function incantations

## Summoning the Utility Spirits

### CDN Invocation

```html
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
<script>
    // Lodash is available as _ global variable
    const result = _.map([1, 2, 3], n => n * 2);
    console.log(result); // [2, 4, 6]
</script>
```

### NPM Ritual

```bash
npm install lodash
```

```javascript
// Import entire library
import _ from 'lodash';
const result = _.map([1, 2, 3], n => n * 2);

// Import specific functions (recommended for tree-shaking)
import { map, filter, reduce } from 'lodash';
const result = map([1, 2, 3], n => n * 2);

// Import individual modules
import map from 'lodash/map';
import filter from 'lodash/filter';
```

### ES6 Modules (Individual Functions)

```javascript
// Best for bundle size optimization
import map from 'lodash/map';
import filter from 'lodash/filter';
import groupBy from 'lodash/groupBy';
import debounce from 'lodash/debounce';
```

## Array Method Enchantments

### Basic Array Operation Spells

```javascript
import { chunk, compact, concat, difference, drop, take, flatten } from 'lodash';

// Chunk - Split array into chunks
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];
const chunked = chunk(numbers, 3);
console.log(chunked); // [[1, 2, 3], [4, 5, 6], [7, 8]]

// Compact - Remove falsy values
const mixed = [0, 1, false, 2, '', 3, null, undefined, 4, NaN];
const compacted = compact(mixed);
console.log(compacted); // [1, 2, 3, 4]

// Concat - Concatenate arrays and values
const arr1 = [1, 2];
const arr2 = [3, 4];
const concatenated = concat(arr1, arr2, 5, [6, 7]);
console.log(concatenated); // [1, 2, 3, 4, 5, 6, 7]

// Difference - Find elements in first array not in others
const array1 = [1, 2, 3, 4, 5];
const array2 = [3, 4];
const array3 = [4, 5, 6];
const diff = difference(array1, array2, array3);
console.log(diff); // [1, 2]

// Drop and Take - Remove/take elements from beginning
const letters = ['a', 'b', 'c', 'd', 'e'];
const dropped = drop(letters, 2);     // ['c', 'd', 'e']
const taken = take(letters, 3);       // ['a', 'b', 'c']

// Flatten - Flatten nested arrays
const nested = [1, [2, 3], [4, [5, 6]]];
const flattened = flatten(nested);           // [1, 2, 3, 4, [5, 6]]
const deepFlattened = flattenDeep(nested);   // [1, 2, 3, 4, 5, 6]
```

### Array Searching and Filtering Divination

```javascript
import { find, findIndex, filter, reject, partition, some, every } from 'lodash';

const users = [
    { id: 1, name: 'John', age: 25, active: true },
    { id: 2, name: 'Jane', age: 30, active: false },
    { id: 3, name: 'Bob', age: 35, active: true },
    { id: 4, name: 'Alice', age: 28, active: false }
];

// Find - Get first matching element
const activeUser = find(users, { active: true });
console.log(activeUser); // { id: 1, name: 'John', age: 25, active: true }

const youngUser = find(users, user => user.age < 30);
console.log(youngUser); // { id: 1, name: 'John', age: 25, active: true }

// FindIndex - Get index of first matching element
const johnIndex = findIndex(users, { name: 'John' });
console.log(johnIndex); // 0

// Filter - Get all matching elements
const activeUsers = filter(users, { active: true });
const youngUsers = filter(users, user => user.age < 30);

// Reject - Get all non-matching elements (opposite of filter)
const inactiveUsers = reject(users, { active: true });

// Partition - Split array into two groups
const [active, inactive] = partition(users, { active: true });
console.log('Active:', active.length, 'Inactive:', inactive.length);

// Some - Check if any element matches
const hasActiveUsers = some(users, { active: true }); // true
const hasMinors = some(users, user => user.age < 18); // false

// Every - Check if all elements match
const allAdults = every(users, user => user.age >= 18); // true
const allActive = every(users, { active: true }); // false
```

### Array Transformation Alchemy

```javascript
import { map, reduce, groupBy, sortBy, orderBy, uniq, uniqBy } from 'lodash';

const products = [
    { id: 1, name: 'Laptop', price: 999, category: 'Electronics', rating: 4.5 },
    { id: 2, name: 'Phone', price: 699, category: 'Electronics', rating: 4.2 },
    { id: 3, name: 'Book', price: 29, category: 'Education', rating: 4.8 },
    { id: 4, name: 'Headphones', price: 199, category: 'Electronics', rating: 4.0 },
    { id: 5, name: 'Notebook', price: 15, category: 'Education', rating: 4.3 }
];

// Map - Transform each element
const productNames = map(products, 'name');
const discountedPrices = map(products, product => product.price * 0.9);

// Reduce - Aggregate values
const totalValue = reduce(products, (sum, product) => sum + product.price, 0);
const categoryCount = reduce(products, (acc, product) => {
    acc[product.category] = (acc[product.category] || 0) + 1;
    return acc;
}, {});

// GroupBy - Group elements by criteria
const byCategory = groupBy(products, 'category');
const byPriceRange = groupBy(products, product => {
    if (product.price < 50) return 'cheap';
    if (product.price < 500) return 'medium';
    return 'expensive';
});

// SortBy - Sort by one or more criteria
const sortedByPrice = sortBy(products, 'price');
const sortedByRating = sortBy(products, product => -product.rating); // Descending

// OrderBy - Sort with explicit order
const ordered = orderBy(products, ['category', 'price'], ['asc', 'desc']);

// Uniq - Remove duplicates
const categories = uniq(map(products, 'category'));
const uniqueProducts = uniqBy(products, 'category'); // First product from each category
```

## Object Method Sorcery

### Object Manipulation Enchantments

```javascript
import { 
    get, set, has, omit, pick, merge, cloneDeep, 
    keys, values, entries, mapKeys, mapValues 
} from 'lodash';

const user = {
    id: 1,
    profile: {
        name: 'John Doe',
        email: 'john@example.com',
        address: {
            street: '123 Main St',
            city: 'New York',
            country: 'USA'
        }
    },
    preferences: {
        theme: 'dark',
        notifications: true
    }
};

// Get - Safely access nested properties
const userName = get(user, 'profile.name');
const street = get(user, 'profile.address.street');
const phone = get(user, 'profile.phone', 'Not provided'); // With default

// Set - Safely set nested properties
const updatedUser = cloneDeep(user);
set(updatedUser, 'profile.phone', '+1-555-0123');
set(updatedUser, 'profile.social.twitter', '@johndoe');

// Has - Check if property exists
const hasEmail = has(user, 'profile.email'); // true
const hasPhone = has(user, 'profile.phone'); // false

// Pick - Select specific properties
const publicProfile = pick(user, ['id', 'profile.name', 'profile.email']);
const userPrefs = pick(user, ['preferences.theme', 'preferences.notifications']);

// Omit - Exclude specific properties
const userWithoutId = omit(user, ['id']);
const profileWithoutEmail = omit(user.profile, ['email']);

// Merge - Deep merge objects
const defaultSettings = {
    theme: 'light',
    notifications: true,
    privacy: {
        showEmail: false,
        showPhone: false
    }
};

const userSettings = {
    theme: 'dark',
    privacy: {
        showEmail: true
    }
};

const mergedSettings = merge({}, defaultSettings, userSettings);
// Result: { theme: 'dark', notifications: true, privacy: { showEmail: true, showPhone: false } }
```

### Object Transformation Alchemy

```javascript
import { mapKeys, mapValues, invert, transform, toPairs, fromPairs } from 'lodash';

const data = {
    firstName: 'John',
    lastName: 'Doe',
    emailAddress: 'john@example.com',
    phoneNumber: '+1-555-0123'
};

// MapKeys - Transform object keys
const camelCased = mapKeys(data, (value, key) => camelCase(key));
const uppercased = mapKeys(data, (value, key) => key.toUpperCase());

// MapValues - Transform object values
const lengths = mapValues(data, value => value.length);
const uppercasedValues = mapValues(data, value => value.toUpperCase());

// Invert - Swap keys and values
const inverted = invert(data);
// { 'John': 'firstName', 'Doe': 'lastName', ... }

// Transform - Custom object transformation
const transformed = transform(data, (result, value, key) => {
    if (typeof value === 'string' && value.includes('@')) {
        result.emails = result.emails || [];
        result.emails.push(value);
    } else {
        result[key] = value;
    }
}, {});

// ToPairs and FromPairs - Convert between object and array of pairs
const pairs = toPairs(data);
// [['firstName', 'John'], ['lastName', 'Doe'], ...]

const backToObject = fromPairs(pairs);
// { firstName: 'John', lastName: 'Doe', ... }
```

## String Method Incantations

### String Manipulation Spells

```javascript
import { 
    camelCase, kebabCase, snakeCase, startCase, upperFirst, lowerFirst,
    capitalize, deburr, escape, unescape, pad, padStart, padEnd,
    repeat, trim, trimStart, trimEnd, truncate
} from 'lodash';

const text = '  hello world example  ';
const longText = 'This is a very long text that needs to be truncated for display purposes';

// Case conversion
console.log(camelCase(text));    // 'helloWorldExample'
console.log(kebabCase(text));    // 'hello-world-example'
console.log(snakeCase(text));    // 'hello_world_example'
console.log(startCase(text));    // 'Hello World Example'

// Capitalization
console.log(capitalize(text));   // '  hello world example  ' (only first char)
console.log(upperFirst(text));   // '  Hello world example  '
console.log(lowerFirst('Hello')); // 'hello'

// Trimming and padding
console.log(trim(text));         // 'hello world example'
console.log(trimStart(text));    // 'hello world example  '
console.log(trimEnd(text));      // '  hello world example'

console.log(pad('hello', 10));       // '  hello   '
console.log(padStart('hello', 10));  // '     hello'
console.log(padEnd('hello', 10));    // 'hello     '

// Repeat and truncate
console.log(repeat('*', 5));     // '*****'
console.log(truncate(longText, { length: 30 })); 
// 'This is a very long text th...'

console.log(truncate(longText, { 
    length: 30, 
    separator: ' ',
    omission: '...' 
})); 
// 'This is a very long text...'

// Special characters
console.log(deburr('déjà vu'));  // 'deja vu'
console.log(escape('<script>')); // '&lt;script&gt;'
```

## Function Utility Mysticism

### Function Control Enchantments

```javascript
import { debounce, throttle, once, memoize, delay, defer } from 'lodash';

// Debounce - Execute function after delay, reset timer on each call
const searchInput = document.getElementById('search');
const debouncedSearch = debounce((query) => {
    console.log('Searching for:', query);
    // Perform search API call
}, 300);

searchInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
});

// Throttle - Execute function at most once per interval
const throttledScroll = throttle(() => {
    console.log('Scroll event handled');
    // Update scroll position, lazy load images, etc.
}, 100);

window.addEventListener('scroll', throttledScroll);

// Once - Execute function only once
const initializeApp = once(() => {
    console.log('App initialized');
    // Expensive initialization code
});

// Memoize - Cache function results
const expensiveCalculation = memoize((n) => {
    console.log('Calculating for:', n);
    // Simulate expensive operation
    let result = 0;
    for (let i = 0; i < n * 1000000; i++) {
        result += Math.random();
    }
    return result;
});

console.log(expensiveCalculation(100)); // Calculates
console.log(expensiveCalculation(100)); // Returns cached result

// Custom memoize resolver
const memoizedApiCall = memoize(
    async (endpoint, params) => {
        const response = await fetch(`${endpoint}?${new URLSearchParams(params)}`);
        return response.json();
    },
    (endpoint, params) => `${endpoint}_${JSON.stringify(params)}` // Custom cache key
);

// Delay and defer
delay(() => {
    console.log('Executed after 1 second');
}, 1000);

defer(() => {
    console.log('Executed on next tick');
});
```

### Function Composition Alchemy

```javascript
import { flow, flowRight, partial, partialRight, curry, bind } from 'lodash';

// Flow - Left-to-right function composition
const processText = flow([
    text => text.toLowerCase(),
    text => text.replace(/[^a-z0-9\s]/g, ''),
    text => text.trim(),
    text => text.split(' '),
    words => words.filter(word => word.length > 2),
    words => words.join('-')
]);

console.log(processText('Hello, World! This is a Test.'));
// 'hello-world-this-test'

// FlowRight - Right-to-left function composition
const processNumber = flowRight([
    n => n.toFixed(2),
    n => n * 1.1,
    n => Math.max(0, n),
    n => n - 10
]);

console.log(processNumber(15)); // '5.50'

// Partial application
const multiply = (a, b, c) => a * b * c;
const double = partial(multiply, 2);
const quadruple = partial(multiply, 2, 2);

console.log(double(5, 3));    // 30 (2 * 5 * 3)
console.log(quadruple(5));    // 20 (2 * 2 * 5)

// Partial right
const subtract = (a, b, c) => a - b - c;
const subtractFromTen = partialRight(subtract, 5, 2);
console.log(subtractFromTen(10)); // 3 (10 - 5 - 2)

// Curry - Transform function to accept arguments one at a time
const curriedMultiply = curry((a, b, c) => a * b * c);
const multiplyByTwo = curriedMultiply(2);
const multiplyByTwoAndThree = multiplyByTwo(3);

console.log(multiplyByTwoAndThree(4)); // 24
console.log(curriedMultiply(2)(3)(4)); // 24
console.log(curriedMultiply(2, 3, 4)); // 24

## Type Checking and Validation Divination

### Basic Type Checking Oracles

```javascript
import {
    isArray, isObject, isString, isNumber, isBoolean, isFunction,
    isDate, isRegExp, isNull, isUndefined, isNil, isEmpty,
    isPlainObject, isArrayLike, isInteger, isFinite, isNaN
} from 'lodash';

const testValues = [
    [],
    {},
    'hello',
    42,
    true,
    function() {},
    new Date(),
    /regex/,
    null,
    undefined,
    '',
    0,
    NaN,
    Infinity
];

// Basic type checks
console.log('Arrays:', testValues.filter(isArray));
console.log('Objects:', testValues.filter(isObject));
console.log('Strings:', testValues.filter(isString));
console.log('Numbers:', testValues.filter(isNumber));
console.log('Booleans:', testValues.filter(isBoolean));
console.log('Functions:', testValues.filter(isFunction));

// Special checks
console.log('Dates:', testValues.filter(isDate));
console.log('RegExp:', testValues.filter(isRegExp));
console.log('Null values:', testValues.filter(isNull));
console.log('Undefined values:', testValues.filter(isUndefined));
console.log('Nil (null or undefined):', testValues.filter(isNil));
console.log('Empty values:', testValues.filter(isEmpty));

// Advanced checks
console.log('Plain objects:', testValues.filter(isPlainObject));
console.log('Array-like:', testValues.filter(isArrayLike));
console.log('Integers:', testValues.filter(isInteger));
console.log('Finite numbers:', testValues.filter(isFinite));
console.log('NaN values:', testValues.filter(isNaN));
```

### Advanced Type Checking Prophecies

```javascript
import {
    isEqual, isEqualWith, isMatch, isMatchWith,
    conformsTo, isValidDate, isError, isElement
} from 'lodash';

// Deep equality comparison
const obj1 = { a: 1, b: { c: 2 } };
const obj2 = { a: 1, b: { c: 2 } };
const obj3 = { a: 1, b: { c: 3 } };

console.log(obj1 === obj2);        // false (reference equality)
console.log(isEqual(obj1, obj2));  // true (deep equality)
console.log(isEqual(obj1, obj3));  // false

// Custom equality comparison
const customEqual = isEqualWith(obj1, obj2, (objValue, othValue, key) => {
    if (key === 'timestamp') {
        // Ignore timestamp differences
        return true;
    }
    // Use default comparison for other properties
    return undefined;
});

// Partial matching
const user = { id: 1, name: 'John', age: 25, active: true };
console.log(isMatch(user, { name: 'John', active: true })); // true
console.log(isMatch(user, { name: 'Jane' })); // false

// Schema validation
const userSchema = {
    id: isNumber,
    name: isString,
    email: (value) => isString(value) && value.includes('@'),
    age: (value) => isNumber(value) && value >= 0 && value <= 120,
    active: isBoolean
};

const validUser = { id: 1, name: 'John', email: 'john@example.com', age: 25, active: true };
const invalidUser = { id: '1', name: 'John', email: 'invalid-email', age: -5, active: 'yes' };

console.log(conformsTo(validUser, userSchema));   // true
console.log(conformsTo(invalidUser, userSchema)); // false
```

## Collection Method Sorcery (Arrays and Objects)

### Iteration and Transformation Rituals

```javascript
import {
    forEach, forEachRight, map, filter, reduce, reduceRight,
    find, findLast, some, every, includes, invoke, sample, sampleSize
} from 'lodash';

const users = [
    { id: 1, name: 'John', department: 'Engineering', salary: 75000 },
    { id: 2, name: 'Jane', department: 'Marketing', salary: 65000 },
    { id: 3, name: 'Bob', department: 'Engineering', salary: 80000 },
    { id: 4, name: 'Alice', department: 'Sales', salary: 70000 }
];

// ForEach - Iterate over collection
forEach(users, (user, index) => {
    console.log(`${index}: ${user.name} - ${user.department}`);
});

// Works with objects too
const departments = { eng: 'Engineering', mkt: 'Marketing', sales: 'Sales' };
forEach(departments, (fullName, code) => {
    console.log(`${code}: ${fullName}`);
});

// Map - Transform collection
const userNames = map(users, 'name');
const salaryBonuses = map(users, user => user.salary * 0.1);

// Filter - Select matching items
const engineers = filter(users, { department: 'Engineering' });
const highEarners = filter(users, user => user.salary > 70000);

// Reduce - Aggregate values
const totalSalary = reduce(users, (sum, user) => sum + user.salary, 0);
const departmentCounts = reduce(users, (counts, user) => {
    counts[user.department] = (counts[user.department] || 0) + 1;
    return counts;
}, {});

// Find operations
const firstEngineer = find(users, { department: 'Engineering' });
const lastHighEarner = findLast(users, user => user.salary > 70000);

// Boolean checks
const hasEngineers = some(users, { department: 'Engineering' });
const allHighEarners = every(users, user => user.salary > 60000);
const includesJohn = includes(map(users, 'name'), 'John');

// Random sampling
const randomUser = sample(users);
const randomUsers = sampleSize(users, 2);
```

### Grouping and Sorting Enchantments

```javascript
import {
    groupBy, keyBy, countBy, partition, sortBy, orderBy,
    maxBy, minBy, meanBy, sumBy, size
} from 'lodash';

const products = [
    { id: 1, name: 'Laptop', category: 'Electronics', price: 999, rating: 4.5, inStock: true },
    { id: 2, name: 'Phone', category: 'Electronics', price: 699, rating: 4.2, inStock: false },
    { id: 3, name: 'Book', category: 'Education', price: 29, rating: 4.8, inStock: true },
    { id: 4, name: 'Headphones', category: 'Electronics', price: 199, rating: 4.0, inStock: true },
    { id: 5, name: 'Notebook', category: 'Education', price: 15, rating: 4.3, inStock: false }
];

// GroupBy - Group by property or function
const byCategory = groupBy(products, 'category');
const byPriceRange = groupBy(products, product => {
    if (product.price < 50) return 'budget';
    if (product.price < 500) return 'mid-range';
    return 'premium';
});

// KeyBy - Create object with keys from property
const productsById = keyBy(products, 'id');
const productsByName = keyBy(products, 'name');

// CountBy - Count occurrences
const categoryCount = countBy(products, 'category');
const stockCount = countBy(products, 'inStock');

// Partition - Split into two groups
const [inStock, outOfStock] = partition(products, 'inStock');
const [affordable, expensive] = partition(products, product => product.price < 100);

// Sorting
const sortedByPrice = sortBy(products, 'price');
const sortedByRating = sortBy(products, product => -product.rating); // Descending

// Multi-criteria sorting
const ordered = orderBy(products,
    ['category', 'price', 'rating'],
    ['asc', 'desc', 'desc']
);

// Aggregation functions
const mostExpensive = maxBy(products, 'price');
const cheapest = minBy(products, 'price');
const averagePrice = meanBy(products, 'price');
const totalValue = sumBy(products, 'price');
const productCount = size(products);

console.log('Most expensive:', mostExpensive.name, '$' + mostExpensive.price);
console.log('Average price:', '$' + averagePrice.toFixed(2));
console.log('Total inventory value:', '$' + totalValue);
```

## Practical Utility Manifestations

### Data Processing Pipeline Rituals

```javascript
import {
    flow, map, filter, groupBy, sortBy, take,
    sumBy, meanBy, maxBy, pick, omit
} from 'lodash';

// Sample sales data
const salesData = [
    { id: 1, product: 'Laptop', category: 'Electronics', amount: 999, date: '2023-01-15', salesperson: 'John' },
    { id: 2, product: 'Phone', category: 'Electronics', amount: 699, date: '2023-01-16', salesperson: 'Jane' },
    { id: 3, product: 'Book', category: 'Education', amount: 29, date: '2023-01-17', salesperson: 'John' },
    { id: 4, product: 'Headphones', category: 'Electronics', amount: 199, date: '2023-01-18', salesperson: 'Bob' },
    { id: 5, product: 'Course', category: 'Education', amount: 299, date: '2023-01-19', salesperson: 'Jane' },
    { id: 6, product: 'Tablet', category: 'Electronics', amount: 499, date: '2023-01-20', salesperson: 'John' }
];

// Create a data processing pipeline
const processSalesData = flow([
    // Filter out low-value sales
    data => filter(data, sale => sale.amount > 50),

    // Add computed fields
    data => map(data, sale => ({
        ...sale,
        month: sale.date.substring(0, 7),
        quarter: Math.ceil(parseInt(sale.date.substring(5, 7)) / 3)
    })),

    // Group by salesperson
    data => groupBy(data, 'salesperson'),

    // Transform to summary format
    grouped => map(grouped, (sales, salesperson) => ({
        salesperson,
        totalSales: sumBy(sales, 'amount'),
        averageSale: meanBy(sales, 'amount'),
        salesCount: sales.length,
        topSale: maxBy(sales, 'amount'),
        categories: [...new Set(map(sales, 'category'))]
    })),

    // Sort by total sales
    summaries => sortBy(summaries, summary => -summary.totalSales),

    // Take top performers
    summaries => take(summaries, 3)
]);

const topPerformers = processSalesData(salesData);
console.log('Top Sales Performers:', topPerformers);
```

### Form Validation Utility Enchantments

```javascript
import {
    isString, isEmail, isNumber, isBoolean, isEmpty,
    isLength, matches, conformsTo, mapValues, pickBy
} from 'lodash';

class FormValidator {
    constructor() {
        this.rules = {};
        this.messages = {};
    }

    // Define validation rules
    addRule(field, validators, message) {
        this.rules[field] = Array.isArray(validators) ? validators : [validators];
        this.messages[field] = message;
        return this;
    }

    // Built-in validators
    static validators = {
        required: (value) => !isEmpty(value),
        string: (value) => isString(value),
        number: (value) => isNumber(value),
        boolean: (value) => isBoolean(value),
        email: (value) => isString(value) && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value),
        minLength: (min) => (value) => isString(value) && value.length >= min,
        maxLength: (max) => (value) => isString(value) && value.length <= max,
        pattern: (regex) => (value) => isString(value) && regex.test(value),
        min: (min) => (value) => isNumber(value) && value >= min,
        max: (max) => (value) => isNumber(value) && value <= max,
        oneOf: (options) => (value) => options.includes(value)
    };

    // Validate form data
    validate(data) {
        const errors = {};

        // Check each field
        Object.keys(this.rules).forEach(field => {
            const value = data[field];
            const fieldRules = this.rules[field];

            for (const rule of fieldRules) {
                if (!rule(value)) {
                    errors[field] = this.messages[field] || `Invalid ${field}`;
                    break;
                }
            }
        });

        return {
            isValid: Object.keys(errors).length === 0,
            errors
        };
    }

    // Validate single field
    validateField(field, value) {
        const fieldRules = this.rules[field];
        if (!fieldRules) return { isValid: true };

        for (const rule of fieldRules) {
            if (!rule(value)) {
                return {
                    isValid: false,
                    error: this.messages[field] || `Invalid ${field}`
                };
            }
        }

        return { isValid: true };
    }
}

// Usage example
const userValidator = new FormValidator()
    .addRule('name', [
        FormValidator.validators.required,
        FormValidator.validators.string,
        FormValidator.validators.minLength(2)
    ], 'Name is required and must be at least 2 characters')

    .addRule('email', [
        FormValidator.validators.required,
        FormValidator.validators.email
    ], 'Valid email is required')

    .addRule('age', [
        FormValidator.validators.required,
        FormValidator.validators.number,
        FormValidator.validators.min(18),
        FormValidator.validators.max(120)
    ], 'Age must be between 18 and 120')

    .addRule('role', [
        FormValidator.validators.required,
        FormValidator.validators.oneOf(['admin', 'user', 'moderator'])
    ], 'Role must be admin, user, or moderator');

// Test validation
const userData = {
    name: 'John Doe',
    email: 'john@example.com',
    age: 25,
    role: 'user'
};

const validation = userValidator.validate(userData);
console.log('Validation result:', validation);
```

### API Response Transformer Alchemy

```javascript
import {
    map, pick, omit, camelCase, snakeCase,
    mapKeys, mapValues, isObject, isArray, transform
} from 'lodash';

class APITransformer {
    // Transform API response from snake_case to camelCase
    static toCamelCase(obj) {
        if (isArray(obj)) {
            return map(obj, item => this.toCamelCase(item));
        }

        if (isObject(obj) && obj.constructor === Object) {
            return transform(obj, (result, value, key) => {
                const camelKey = camelCase(key);
                result[camelKey] = this.toCamelCase(value);
            }, {});
        }

        return obj;
    }

    // Transform request data from camelCase to snake_case
    static toSnakeCase(obj) {
        if (isArray(obj)) {
            return map(obj, item => this.toSnakeCase(item));
        }

        if (isObject(obj) && obj.constructor === Object) {
            return transform(obj, (result, value, key) => {
                const snakeKey = snakeCase(key);
                result[snakeKey] = this.toSnakeCase(value);
            }, {});
        }

        return obj;
    }

    // Extract and transform user data
    static transformUser(apiUser) {
        return flow([
            // Pick relevant fields
            user => pick(user, [
                'id', 'firstName', 'lastName', 'email', 'createdAt',
                'profile.avatar', 'profile.bio', 'settings.theme'
            ]),

            // Transform to camelCase
            user => this.toCamelCase(user),

            // Add computed fields
            user => ({
                ...user,
                fullName: `${user.firstName} ${user.lastName}`,
                initials: `${user.firstName[0]}${user.lastName[0]}`.toUpperCase(),
                memberSince: new Date(user.createdAt).getFullYear()
            }),

            // Remove sensitive data
            user => omit(user, ['internalId', 'passwordHash'])
        ])(apiUser);
    }

    // Transform product list
    static transformProducts(apiProducts) {
        return flow([
            // Transform each product
            products => map(products, product => ({
                ...this.toCamelCase(product),
                // Add computed fields
                discountedPrice: product.price * (1 - (product.discount || 0) / 100),
                isOnSale: (product.discount || 0) > 0,
                priceRange: this.getPriceRange(product.price)
            })),

            // Sort by popularity
            products => sortBy(products, product => -product.popularity),

            // Group by category
            products => groupBy(products, 'category')
        ])(apiProducts);
    }

    static getPriceRange(price) {
        if (price < 25) return 'budget';
        if (price < 100) return 'affordable';
        if (price < 500) return 'premium';
        return 'luxury';
    }
}

// Usage example
const apiResponse = {
    user_id: 123,
    first_name: 'John',
    last_name: 'Doe',
    email_address: 'john@example.com',
    created_at: '2023-01-15T10:30:00Z',
    user_profile: {
        avatar_url: 'https://example.com/avatar.jpg',
        bio_text: 'Software developer'
    },
    user_settings: {
        theme_preference: 'dark'
    }
};

const transformedUser = APITransformer.transformUser(apiResponse);
console.log('Transformed user:', transformedUser);
```

## Performance Optimization Enchantments

### Efficient Data Processing Spells

```javascript
import {
    memoize, debounce, throttle, once,
    chain, lazy, take, value
} from 'lodash';

// Memoization for expensive calculations
const expensiveOperation = memoize((data) => {
    console.log('Performing expensive operation...');
    return data.reduce((sum, item) => sum + item.value * item.multiplier, 0);
});

// Chain operations for better performance
const processLargeDataset = (data) => {
    return chain(data)
        .filter(item => item.active)
        .groupBy('category')
        .mapValues(group => ({
            count: group.length,
            total: sumBy(group, 'value'),
            average: meanBy(group, 'value')
        }))
        .value();
};

// Lazy evaluation for large datasets
const lazyProcessing = (largeArray) => {
    return lazy(largeArray)
        .filter(item => item.score > 50)
        .map(item => ({ ...item, grade: item.score > 90 ? 'A' : 'B' }))
        .take(100) // Only process first 100 matching items
        .value();
};

// Debounced search function
const createDebouncedSearch = (searchFn, delay = 300) => {
    return debounce((query) => {
        if (query.length < 2) return;
        searchFn(query);
    }, delay);
};

// Throttled scroll handler
const createThrottledHandler = (handler, limit = 100) => {
    return throttle(handler, limit, { leading: true, trailing: true });
};
```

## Wisdom of the Utility Ancients

### Code Organization Sacred Rituals

```javascript
// Group related utilities
const StringUtils = {
    toCamelCase: camelCase,
    toKebabCase: kebabCase,
    toSnakeCase: snakeCase,
    capitalize: capitalize,
    truncate: (text, length = 50) => truncate(text, { length })
};

const ArrayUtils = {
    chunk: chunk,
    compact: compact,
    unique: uniq,
    uniqueBy: uniqBy,
    groupBy: groupBy,
    sortBy: sortBy
};

const ObjectUtils = {
    pick: pick,
    omit: omit,
    merge: merge,
    cloneDeep: cloneDeep,
    get: get,
    set: set,
    has: has
};

// Create reusable validation functions
const createValidator = (schema) => {
    return (data) => {
        const errors = {};

        Object.keys(schema).forEach(key => {
            const validator = schema[key];
            const value = get(data, key);

            if (!validator(value)) {
                errors[key] = `Invalid ${key}`;
            }
        });

        return {
            isValid: isEmpty(errors),
            errors
        };
    };
};

// Use function composition for complex operations
const processUserData = flow([
    data => pick(data, ['name', 'email', 'age', 'preferences']),
    data => mapValues(data, (value, key) => {
        if (key === 'name') return startCase(value);
        if (key === 'email') return toLower(value);
        return value;
    }),
    data => ({
        ...data,
        id: uniqueId('user_'),
        createdAt: new Date().toISOString()
    })
]);
```

### Error Handling Protective Spells

```javascript
import { attempt, isError } from 'lodash';

// Safe function execution
const safeExecute = (fn, ...args) => {
    const result = attempt(fn, ...args);

    if (isError(result)) {
        console.error('Function execution failed:', result.message);
        return null;
    }

    return result;
};

// Safe property access with fallback
const safeGet = (obj, path, fallback = null) => {
    const result = get(obj, path);
    return result !== undefined ? result : fallback;
};

// Safe array operations
const safeMap = (array, iteratee) => {
    if (!isArray(array)) {
        console.warn('Expected array, got:', typeof array);
        return [];
    }

    return map(array, (item, index) => {
        const result = attempt(iteratee, item, index);
        return isError(result) ? null : result;
    }).filter(item => item !== null);
};

// Usage examples
const userData = { profile: { name: 'John' } };
const userName = safeGet(userData, 'profile.name', 'Anonymous');
const userAge = safeGet(userData, 'profile.age', 0);

const numbers = [1, 2, 3, '4', 5];
const doubled = safeMap(numbers, n => {
    if (!isNumber(n)) throw new Error('Not a number');
    return n * 2;
});
```

## Common Utility Curses & Their Remedies

### Curse 1: Overusing Lodash Dependency
Not all operations need Lodash - modern JavaScript has many built-in alternatives.

**Solution:**
```javascript
// Instead of always using Lodash
const names = _.map(users, 'name');

// Consider native alternatives when appropriate
const names = users.map(user => user.name);

// Use Lodash for complex operations
const grouped = _.groupBy(users, 'department'); // No native equivalent
```

### Curse 2: Performance Issues with Large Dataset Burden
Some Lodash methods can be slow with very large datasets.

**Solution:**
```javascript
// Use lazy evaluation for large datasets
const result = _(largeArray)
    .filter(item => item.active)
    .map(item => item.value)
    .take(100)
    .value();

// Consider native methods for simple operations
const filtered = largeArray.filter(item => item.active); // Often faster than _.filter
```

### Curse 3: Mutating Original Data Corruption
Some Lodash methods mutate the original data.

**Solution:**
```javascript
// Always clone when needed
const original = { a: 1, b: { c: 2 } };
const modified = _.merge(_.cloneDeep(original), { b: { d: 3 } });

// Or use immutable methods
const updated = _.set(_.cloneDeep(original), 'b.d', 3);
```

### Curse 4: Bundle Size Bloat Hex
Importing the entire Lodash library increases bundle size.

**Solution:**
```javascript
// Import only what you need
import map from 'lodash/map';
import filter from 'lodash/filter';

// Or use babel-plugin-lodash for automatic optimization
import { map, filter } from 'lodash'; // Automatically optimized
```

## Sacred Texts & Mystical Sources

{% embed url="https://lodash.com/" %}

{% embed url="https://lodash.com/docs/" %}

{% embed url="https://github.com/lodash/lodash" %}
```
