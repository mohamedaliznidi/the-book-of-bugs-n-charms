---
description: >-
  Advanced TypeScript patterns including utility types, conditional types,
  template literals, complex type inference, and sophisticated type
  manipulation techniques for professional TypeScript development.
---

# Advanced TypeScript Patterns

## Introduction

Advanced TypeScript patterns enable sophisticated type manipulation, better code organization, and enhanced developer experience. These patterns include utility types, conditional types, template literals, and complex type inference that help build robust, type-safe applications.

## Use Cases

1. **API Type Generation**: Automatically generate types from API schemas
2. **Form Validation**: Create type-safe form validation with dynamic schemas
3. **State Management**: Build strongly-typed state management solutions
4. **Library Development**: Create flexible, type-safe library APIs

## Utility Types

### 1. Built-in Utility Types

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
  isActive: boolean;
}

// Partial - makes all properties optional
type PartialUser = Partial<User>;
// { id?: number; name?: string; email?: string; age?: number; isActive?: boolean; }

// Required - makes all properties required
type RequiredUser = Required<PartialUser>;
// { id: number; name: string; email: string; age: number; isActive: boolean; }

// Pick - select specific properties
type UserSummary = Pick<User, 'id' | 'name' | 'email'>;
// { id: number; name: string; email: string; }

// Omit - exclude specific properties
type UserWithoutId = Omit<User, 'id'>;
// { name: string; email: string; age: number; isActive: boolean; }

// Record - create object type with specific keys and values
type UserRoles = Record<'admin' | 'user' | 'guest', User[]>;
// { admin: User[]; user: User[]; guest: User[]; }

// Exclude - exclude types from union
type StringOrNumber = string | number | boolean;
type OnlyStringOrNumber = Exclude<StringOrNumber, boolean>;
// string | number

// Extract - extract types from union
type OnlyString = Extract<StringOrNumber, string>;
// string

// NonNullable - remove null and undefined
type NonNullableString = NonNullable<string | null | undefined>;
// string
```

### 2. Custom Utility Types

```typescript
// Deep Partial - makes all nested properties optional
type DeepPartial<T> = {
  [P in keyof T]?: T[P] extends object ? DeepPartial<T[P]> : T[P];
};

interface NestedUser {
  id: number;
  profile: {
    name: string;
    settings: {
      theme: string;
      notifications: boolean;
    };
  };
}

type DeepPartialUser = DeepPartial<NestedUser>;
// All properties and nested properties are optional

// Mutable - remove readonly modifiers
type Mutable<T> = {
  -readonly [P in keyof T]: T[P];
};

interface ReadonlyUser {
  readonly id: number;
  readonly name: string;
}

type MutableUser = Mutable<ReadonlyUser>;
// { id: number; name: string; }

// Optional Keys - get keys that are optional
type OptionalKeys<T> = {
  [K in keyof T]-?: {} extends Pick<T, K> ? K : never;
}[keyof T];

type UserOptionalKeys = OptionalKeys<PartialUser>;
// 'id' | 'name' | 'email' | 'age' | 'isActive'

// Required Keys - get keys that are required
type RequiredKeys<T> = {
  [K in keyof T]-?: {} extends Pick<T, K> ? never : K;
}[keyof T];

type UserRequiredKeys = RequiredKeys<User>;
// 'id' | 'name' | 'email' | 'age' | 'isActive'
```

## Conditional Types

### 1. Basic Conditional Types

```typescript
// Basic conditional type syntax
type IsString<T> = T extends string ? true : false;

type Test1 = IsString<string>; // true
type Test2 = IsString<number>; // false

// More complex conditional types
type ApiResponse<T> = T extends string
  ? { message: T }
  : T extends number
  ? { code: T }
  : T extends object
  ? { data: T }
  : never;

type StringResponse = ApiResponse<string>; // { message: string }
type NumberResponse = ApiResponse<number>; // { code: number }
type ObjectResponse = ApiResponse<User>; // { data: User }

// Conditional types with infer
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;

function getUser(): User {
  return {} as User;
}

type UserReturnType = ReturnType<typeof getUser>; // User

// Array element type extraction
type ArrayElement<T> = T extends (infer U)[] ? U : never;

type StringArrayElement = ArrayElement<string[]>; // string
type UserArrayElement = ArrayElement<User[]>; // User
```

### 2. Advanced Conditional Patterns

```typescript
// Flatten nested arrays
type Flatten<T> = T extends (infer U)[]
  ? U extends (infer V)[]
    ? Flatten<V>
    : U
  : T;

type NestedArray = string[][][];
type FlattenedArray = Flatten<NestedArray>; // string

// Promise unwrapping
type Awaited<T> = T extends Promise<infer U>
  ? U extends Promise<any>
    ? Awaited<U>
    : U
  : T;

type PromiseString = Awaited<Promise<string>>; // string
type NestedPromise = Awaited<Promise<Promise<number>>>; // number

// Function parameter extraction
type Parameters<T extends (...args: any) => any> = T extends (
  ...args: infer P
) => any
  ? P
  : never;

function createUser(name: string, age: number, email: string): User {
  return {} as User;
}

type CreateUserParams = Parameters<typeof createUser>; // [string, number, string]

// Object key-value mapping
type KeyValuePairs<T> = {
  [K in keyof T]: { key: K; value: T[K] };
}[keyof T];

type UserKeyValuePairs = KeyValuePairs<User>;
// { key: 'id'; value: number } | { key: 'name'; value: string } | ...
```

## Template Literal Types

### 1. Basic Template Literals

```typescript
// Basic template literal types
type Greeting = `Hello, ${string}!`;

const greeting1: Greeting = "Hello, World!"; // ✅
const greeting2: Greeting = "Hi there!"; // ❌ Type error

// Combining with unions
type Color = 'red' | 'green' | 'blue';
type Shade = 'light' | 'dark';

type ColorVariant = `${Shade}-${Color}`;
// 'light-red' | 'light-green' | 'light-blue' | 'dark-red' | 'dark-green' | 'dark-blue'

// Event naming pattern
type EventName<T extends string> = `on${Capitalize<T>}`;

type ClickEvent = EventName<'click'>; // 'onClick'
type ChangeEvent = EventName<'change'>; // 'onChange'
```

### 2. Advanced Template Literal Patterns

```typescript
// API endpoint generation
type HttpMethod = 'GET' | 'POST' | 'PUT' | 'DELETE';
type Resource = 'users' | 'posts' | 'comments';

type ApiEndpoint<M extends HttpMethod, R extends Resource> = 
  `${Lowercase<M>} /${R}`;

type GetUsers = ApiEndpoint<'GET', 'users'>; // 'get /users'
type PostUsers = ApiEndpoint<'POST', 'users'>; // 'post /users'

// CSS property generation
type CSSUnit = 'px' | 'rem' | 'em' | '%';
type CSSValue<T extends number, U extends CSSUnit> = `${T}${U}`;

type PixelValue = CSSValue<16, 'px'>; // '16px'
type RemValue = CSSValue<1, 'rem'>; // '1rem'

// Path parameter extraction
type ExtractPathParams<T extends string> = 
  T extends `${string}:${infer Param}/${infer Rest}`
    ? Param | ExtractPathParams<Rest>
    : T extends `${string}:${infer Param}`
    ? Param
    : never;

type UserPath = '/users/:userId/posts/:postId';
type PathParams = ExtractPathParams<UserPath>; // 'userId' | 'postId'

// SQL query builder types
type SQLOperator = 'SELECT' | 'FROM' | 'WHERE' | 'ORDER BY';
type SQLQuery<T extends string> = `${SQLOperator} ${T}`;

type UserQuery = SQLQuery<'* FROM users WHERE active = true'>;
// 'SELECT * FROM users WHERE active = true'
```

## Complex Type Inference

### 1. Function Overloading with Types

```typescript
// Function overloading with conditional return types
interface DatabaseConfig {
  host: string;
  port: number;
  database: string;
}

interface ApiConfig {
  baseUrl: string;
  apiKey: string;
}

function createConfig<T extends 'database' | 'api'>(
  type: T
): T extends 'database' ? DatabaseConfig : ApiConfig;

function createConfig(type: 'database' | 'api') {
  if (type === 'database') {
    return {
      host: 'localhost',
      port: 5432,
      database: 'myapp'
    } as DatabaseConfig;
  }
  return {
    baseUrl: 'https://api.example.com',
    apiKey: 'secret'
  } as ApiConfig;
}

const dbConfig = createConfig('database'); // DatabaseConfig
const apiConfig = createConfig('api'); // ApiConfig
```

### 2. Generic Constraints and Inference

```typescript
// Advanced generic constraints
interface Identifiable {
  id: string | number;
}

interface Timestamped {
  createdAt: Date;
  updatedAt: Date;
}

// Multiple constraint intersection
function updateEntity<T extends Identifiable & Timestamped>(
  entity: T,
  updates: Partial<Omit<T, 'id' | 'createdAt'>>
): T {
  return {
    ...entity,
    ...updates,
    updatedAt: new Date()
  };
}

interface BlogPost extends Identifiable, Timestamped {
  title: string;
  content: string;
  authorId: string;
}

const post: BlogPost = {
  id: '1',
  title: 'Hello World',
  content: 'This is my first post',
  authorId: 'user1',
  createdAt: new Date(),
  updatedAt: new Date()
};

const updatedPost = updateEntity(post, { title: 'Updated Title' });
// Type is BlogPost, can't update id or createdAt

// Mapped type with key remapping
type Getters<T> = {
  [K in keyof T as `get${Capitalize<string & K>}`]: () => T[K];
};

type UserGetters = Getters<User>;
// {
//   getId: () => number;
//   getName: () => string;
//   getEmail: () => string;
//   getAge: () => number;
//   getIsActive: () => boolean;
// }
```

### 3. Type-Safe Event System

```typescript
// Event system with type inference
type EventMap = {
  'user:created': { user: User };
  'user:updated': { user: User; changes: Partial<User> };
  'user:deleted': { userId: string };
  'post:published': { post: BlogPost; author: User };
};

class TypedEventEmitter<T extends Record<string, any>> {
  private listeners: {
    [K in keyof T]?: Array<(data: T[K]) => void>;
  } = {};

  on<K extends keyof T>(event: K, listener: (data: T[K]) => void): void {
    if (!this.listeners[event]) {
      this.listeners[event] = [];
    }
    this.listeners[event]!.push(listener);
  }

  emit<K extends keyof T>(event: K, data: T[K]): void {
    const eventListeners = this.listeners[event];
    if (eventListeners) {
      eventListeners.forEach(listener => listener(data));
    }
  }

  off<K extends keyof T>(event: K, listener: (data: T[K]) => void): void {
    const eventListeners = this.listeners[event];
    if (eventListeners) {
      const index = eventListeners.indexOf(listener);
      if (index > -1) {
        eventListeners.splice(index, 1);
      }
    }
  }
}

const emitter = new TypedEventEmitter<EventMap>();

// Type-safe event handling
emitter.on('user:created', (data) => {
  // data is automatically typed as { user: User }
  console.log(`User created: ${data.user.name}`);
});

emitter.emit('user:created', { user: post.authorId }); // ❌ Type error
emitter.emit('user:created', { user: {} as User }); // ✅ Correct
```

## Advanced Patterns in Practice

### 1. Form Validation Schema

```typescript
// Type-safe form validation
type ValidationRule<T> = {
  required?: boolean;
  min?: T extends string ? number : T extends number ? number : never;
  max?: T extends string ? number : T extends number ? number : never;
  pattern?: T extends string ? RegExp : never;
  custom?: (value: T) => boolean | string;
};

type FormSchema<T> = {
  [K in keyof T]: ValidationRule<T[K]>;
};

type ValidationErrors<T> = {
  [K in keyof T]?: string[];
};

class FormValidator<T extends Record<string, any>> {
  constructor(private schema: FormSchema<T>) {}

  validate(data: Partial<T>): ValidationErrors<T> {
    const errors: ValidationErrors<T> = {};

    for (const key in this.schema) {
      const rule = this.schema[key];
      const value = data[key];
      const fieldErrors: string[] = [];

      if (rule.required && (value === undefined || value === null || value === '')) {
        fieldErrors.push(`${key} is required`);
      }

      if (value !== undefined && rule.min !== undefined) {
        if (typeof value === 'string' && value.length < rule.min) {
          fieldErrors.push(`${key} must be at least ${rule.min} characters`);
        }
        if (typeof value === 'number' && value < rule.min) {
          fieldErrors.push(`${key} must be at least ${rule.min}`);
        }
      }

      if (fieldErrors.length > 0) {
        errors[key] = fieldErrors;
      }
    }

    return errors;
  }
}

// Usage
const userSchema: FormSchema<User> = {
  id: { required: true },
  name: { required: true, min: 2, max: 50 },
  email: { required: true, pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/ },
  age: { required: true, min: 18, max: 120 },
  isActive: { required: false }
};

const validator = new FormValidator(userSchema);
const errors = validator.validate({ name: 'A' }); // Type-safe validation
```

### 2. API Client with Type Inference

```typescript
// Type-safe API client
type ApiEndpoints = {
  'GET /users': { response: User[] };
  'GET /users/:id': { params: { id: string }; response: User };
  'POST /users': { body: Omit<User, 'id'>; response: User };
  'PUT /users/:id': { params: { id: string }; body: Partial<User>; response: User };
  'DELETE /users/:id': { params: { id: string }; response: void };
};

type ExtractMethod<T> = T extends `${infer M} ${string}` ? M : never;
type ExtractPath<T> = T extends `${string} ${infer P}` ? P : never;

class ApiClient<T extends Record<string, any>> {
  constructor(private baseUrl: string) {}

  async request<K extends keyof T>(
    endpoint: K,
    options?: T[K] extends { params: infer P }
      ? { params: P } & (T[K] extends { body: infer B } ? { body: B } : {})
      : T[K] extends { body: infer B }
      ? { body: B }
      : {}
  ): Promise<T[K] extends { response: infer R } ? R : void> {
    const method = (endpoint as string).split(' ')[0];
    let path = (endpoint as string).split(' ')[1];

    // Replace path parameters
    if (options && 'params' in options) {
      for (const [key, value] of Object.entries(options.params as Record<string, any>)) {
        path = path.replace(`:${key}`, String(value));
      }
    }

    const response = await fetch(`${this.baseUrl}${path}`, {
      method,
      headers: {
        'Content-Type': 'application/json',
      },
      body: options && 'body' in options ? JSON.stringify(options.body) : undefined,
    });

    if (method !== 'DELETE') {
      return response.json();
    }
    return undefined as any;
  }
}

// Usage
const api = new ApiClient<ApiEndpoints>('https://api.example.com');

// Type-safe API calls
const users = await api.request('GET /users'); // User[]
const user = await api.request('GET /users/:id', { params: { id: '1' } }); // User
const newUser = await api.request('POST /users', { 
  body: { name: 'John', email: 'john@example.com', age: 30, isActive: true } 
}); // User
```

## Best Practices

- **Type Safety**: Use strict TypeScript configuration for maximum type safety
- **Generic Constraints**: Use constraints to make generic types more specific and useful
- **Utility Types**: Leverage built-in utility types before creating custom ones
- **Template Literals**: Use template literal types for string manipulation and validation
- **Conditional Types**: Use conditional types for flexible API design
- **Type Inference**: Let TypeScript infer types when possible to reduce verbosity

## Common Pitfalls

### Issue 1: Overly Complex Types
Complex types can hurt performance and readability.

**Solution:**
Break complex types into smaller, reusable pieces:
```typescript
// ❌ Wrong - overly complex
type ComplexType<T> = T extends Record<string, any> 
  ? { [K in keyof T]: T[K] extends string 
      ? `processed_${T[K]}` 
      : T[K] extends number 
        ? T[K] * 2 
        : T[K] } 
  : never;

// ✅ Better - break into smaller types
type ProcessString<T> = T extends string ? `processed_${T}` : T;
type ProcessNumber<T> = T extends number ? T : T;
type ProcessValue<T> = ProcessString<T> | ProcessNumber<T>;
type ProcessObject<T> = { [K in keyof T]: ProcessValue<T[K]> };
```

### Issue 2: Circular Type References
Circular references can cause TypeScript to fail.

**Solution:**
Use interfaces for recursive types:
```typescript
// ❌ Wrong - can cause issues
type TreeNode<T> = {
  value: T;
  children: TreeNode<T>[];
};

// ✅ Better - use interface
interface TreeNode<T> {
  value: T;
  children: TreeNode<T>[];
}
```

### Issue 3: Type Assertion Overuse
Excessive type assertions can bypass type safety.

**Solution:**
Use type guards and proper typing:
```typescript
// ❌ Wrong - type assertion
const user = data as User;

// ✅ Better - type guard
function isUser(data: any): data is User {
  return data && typeof data.id === 'number' && typeof data.name === 'string';
}

if (isUser(data)) {
  // data is now properly typed as User
}
```

## References

{% embed url="https://www.typescriptlang.org/docs/handbook/2/conditional-types.html" %}

{% embed url="https://www.typescriptlang.org/docs/handbook/2/template-literal-types.html" %}

{% embed url="https://www.typescriptlang.org/docs/handbook/utility-types.html" %}
