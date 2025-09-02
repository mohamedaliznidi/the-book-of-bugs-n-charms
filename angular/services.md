---
description: >-
  Angular Services provide a way to share data and functionality across components. 
  Learn about dependency injection, HTTP services, and state management patterns.
---

# Angular Services

## Introduction

Services in Angular are singleton objects that get instantiated only once during the lifetime of an application. They contain methods that maintain data throughout the life of an application, i.e., data is available all the time. Services are used to organize and share business logic, models, data, or functions with different components of an Angular application.

## Why Use Services?

### Separation of Concerns
- **Components**: Handle UI logic and user interactions
- **Services**: Handle business logic, data access, and shared functionality

### Code Reusability
- Share common functionality across multiple components
- Avoid code duplication
- Maintain consistency across the application

### Testability
- Services can be easily mocked and tested in isolation
- Better unit testing capabilities

### Data Sharing
- Share data between components that don't have a parent-child relationship
- Maintain application state

## Creating Services

### Basic Service Creation

```bash
# Generate a new service
ng generate service user

# Short form
ng g s user

# Generate service in specific folder
ng g s services/user

# Generate service with specific options
ng g s user --skip-tests
```

### Basic Service Example

```typescript
// user.service.ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root' // Makes service available application-wide
})
export class UserService {
  private users: User[] = [
    { id: 1, name: 'John Doe', email: 'john@example.com', role: 'admin' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com', role: 'user' },
    { id: 3, name: 'Bob Johnson', email: 'bob@example.com', role: 'user' }
  ];

  private currentUser: User | null = null;

  // Get all users
  getUsers(): User[] {
    return [...this.users]; // Return copy to prevent mutation
  }

  // Get user by ID
  getUserById(id: number): User | undefined {
    return this.users.find(user => user.id === id);
  }

  // Add new user
  addUser(user: Omit<User, 'id'>): User {
    const newUser: User = {
      ...user,
      id: Math.max(...this.users.map(u => u.id)) + 1
    };
    this.users.push(newUser);
    return newUser;
  }

  // Update user
  updateUser(id: number, updates: Partial<User>): User | null {
    const userIndex = this.users.findIndex(user => user.id === id);
    if (userIndex !== -1) {
      this.users[userIndex] = { ...this.users[userIndex], ...updates };
      return this.users[userIndex];
    }
    return null;
  }

  // Delete user
  deleteUser(id: number): boolean {
    const userIndex = this.users.findIndex(user => user.id === id);
    if (userIndex !== -1) {
      this.users.splice(userIndex, 1);
      return true;
    }
    return false;
  }

  // Current user management
  setCurrentUser(user: User): void {
    this.currentUser = user;
  }

  getCurrentUser(): User | null {
    return this.currentUser;
  }

  isLoggedIn(): boolean {
    return this.currentUser !== null;
  }

  logout(): void {
    this.currentUser = null;
  }
}

// user.interface.ts
export interface User {
  id: number;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'moderator';
}
```

### Using Services in Components

```typescript
// user-list.component.ts
import { Component, OnInit } from '@angular/core';
import { UserService } from '../services/user.service';
import { User } from '../interfaces/user.interface';

@Component({
  selector: 'app-user-list',
  template: `
    <div class="user-list">
      <h2>Users</h2>
      
      <div class="user-actions">
        <button (click)="showAddForm = !showAddForm">
          {{ showAddForm ? 'Cancel' : 'Add User' }}
        </button>
      </div>

      <!-- Add User Form -->
      <div *ngIf="showAddForm" class="add-user-form">
        <h3>Add New User</h3>
        <form (ngSubmit)="addUser()" #userForm="ngForm">
          <input [(ngModel)]="newUser.name" name="name" placeholder="Name" required>
          <input [(ngModel)]="newUser.email" name="email" type="email" placeholder="Email" required>
          <select [(ngModel)]="newUser.role" name="role" required>
            <option value="user">User</option>
            <option value="admin">Admin</option>
            <option value="moderator">Moderator</option>
          </select>
          <button type="submit" [disabled]="!userForm.valid">Add User</button>
        </form>
      </div>

      <!-- User List -->
      <div class="users">
        <div *ngFor="let user of users" class="user-card">
          <div class="user-info">
            <h4>{{ user.name }}</h4>
            <p>{{ user.email }}</p>
            <span class="role">{{ user.role }}</span>
          </div>
          <div class="user-actions">
            <button (click)="editUser(user)">Edit</button>
            <button (click)="deleteUser(user.id)" class="danger">Delete</button>
            <button (click)="setCurrentUser(user)">Login as</button>
          </div>
        </div>
      </div>

      <!-- Current User Info -->
      <div *ngIf="currentUser" class="current-user">
        <h3>Current User</h3>
        <p>{{ currentUser.name }} ({{ currentUser.role }})</p>
        <button (click)="logout()">Logout</button>
      </div>
    </div>
  `,
  styles: [`
    .user-list { padding: 20px; }
    .user-card {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 16px;
      border: 1px solid #ddd;
      border-radius: 8px;
      margin: 8px 0;
    }
    .add-user-form {
      background: #f5f5f5;
      padding: 16px;
      border-radius: 8px;
      margin: 16px 0;
    }
    .add-user-form input, .add-user-form select {
      margin: 4px;
      padding: 8px;
      border: 1px solid #ddd;
      border-radius: 4px;
    }
    .current-user {
      background: #e3f2fd;
      padding: 16px;
      border-radius: 8px;
      margin-top: 16px;
    }
    .role {
      background: #2196f3;
      color: white;
      padding: 4px 8px;
      border-radius: 12px;
      font-size: 12px;
    }
    .danger { background-color: #f44336; color: white; }
  `]
})
export class UserListComponent implements OnInit {
  users: User[] = [];
  currentUser: User | null = null;
  showAddForm = false;
  newUser: Omit<User, 'id'> = { name: '', email: '', role: 'user' };

  constructor(private userService: UserService) {}

  ngOnInit(): void {
    this.loadUsers();
    this.currentUser = this.userService.getCurrentUser();
  }

  loadUsers(): void {
    this.users = this.userService.getUsers();
  }

  addUser(): void {
    if (this.newUser.name && this.newUser.email) {
      this.userService.addUser(this.newUser);
      this.loadUsers();
      this.newUser = { name: '', email: '', role: 'user' };
      this.showAddForm = false;
    }
  }

  editUser(user: User): void {
    const newName = prompt('Enter new name:', user.name);
    if (newName && newName !== user.name) {
      this.userService.updateUser(user.id, { name: newName });
      this.loadUsers();
    }
  }

  deleteUser(id: number): void {
    if (confirm('Are you sure you want to delete this user?')) {
      this.userService.deleteUser(id);
      this.loadUsers();
    }
  }

  setCurrentUser(user: User): void {
    this.userService.setCurrentUser(user);
    this.currentUser = this.userService.getCurrentUser();
  }

  logout(): void {
    this.userService.logout();
    this.currentUser = null;
  }
}
```

## Dependency Injection

### Understanding Dependency Injection

Dependency Injection (DI) is a design pattern where dependencies are provided to a class rather than the class creating them itself. Angular has a built-in DI system that manages the creation and lifetime of service instances.

### Provider Configuration

```typescript
// Different ways to provide services

// 1. Root level (recommended for most services)
@Injectable({
  providedIn: 'root'
})
export class GlobalService { }

// 2. Module level
@NgModule({
  providers: [UserService]
})
export class AppModule { }

// 3. Component level (creates new instance for each component)
@Component({
  selector: 'app-example',
  providers: [LocalService]
})
export class ExampleComponent { }

// 4. Using factory providers
@NgModule({
  providers: [
    {
      provide: ConfigService,
      useFactory: (http: HttpClient) => {
        return new ConfigService(http, environment.apiUrl);
      },
      deps: [HttpClient]
    }
  ]
})
export class AppModule { }

// 5. Using value providers
@NgModule({
  providers: [
    { provide: 'API_URL', useValue: 'https://api.example.com' }
  ]
})
export class AppModule { }
```

### Injection Tokens

```typescript
// Creating injection tokens for non-class dependencies
import { InjectionToken } from '@angular/core';

// Define injection token
export const API_CONFIG = new InjectionToken<ApiConfig>('api.config');

export interface ApiConfig {
  baseUrl: string;
  timeout: number;
  retries: number;
}

// Provide the token
@NgModule({
  providers: [
    {
      provide: API_CONFIG,
      useValue: {
        baseUrl: 'https://api.example.com',
        timeout: 10000,
        retries: 3
      }
    }
  ]
})
export class AppModule { }

// Inject the token
@Injectable({
  providedIn: 'root'
})
export class ApiService {
  constructor(@Inject(API_CONFIG) private config: ApiConfig) {
    console.log('API Base URL:', this.config.baseUrl);
  }
}
```

### Optional and Multiple Providers

```typescript
import { Injectable, Optional, Inject } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class LoggingService {
  constructor(
    @Optional() @Inject('LOGGER_CONFIG') private config?: any
  ) {
    // config will be null if not provided
    if (this.config) {
      console.log('Logger configured with:', this.config);
    }
  }
}

// Multiple providers
@NgModule({
  providers: [
    { provide: 'PLUGINS', useClass: PluginA, multi: true },
    { provide: 'PLUGINS', useClass: PluginB, multi: true },
    { provide: 'PLUGINS', useClass: PluginC, multi: true }
  ]
})
export class AppModule { }

@Injectable()
export class PluginManager {
  constructor(@Inject('PLUGINS') private plugins: any[]) {
    // plugins will be an array of all provided plugins
    console.log('Loaded plugins:', this.plugins.length);
  }
}

## HTTP Services

### Basic HTTP Service

```typescript
// api.service.ts
import { Injectable } from '@angular/core';
import { HttpClient, HttpHeaders, HttpParams, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError, BehaviorSubject } from 'rxjs';
import { catchError, map, retry, timeout } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private readonly baseUrl = 'https://api.example.com';
  private readonly timeout = 10000;

  constructor(private http: HttpClient) {}

  // GET request
  get<T>(endpoint: string, params?: any): Observable<T> {
    const httpParams = this.buildParams(params);
    const options = {
      headers: this.getHeaders(),
      params: httpParams
    };

    return this.http.get<T>(`${this.baseUrl}/${endpoint}`, options)
      .pipe(
        timeout(this.timeout),
        retry(2),
        catchError(this.handleError)
      );
  }

  // POST request
  post<T>(endpoint: string, data: any): Observable<T> {
    const options = {
      headers: this.getHeaders()
    };

    return this.http.post<T>(`${this.baseUrl}/${endpoint}`, data, options)
      .pipe(
        timeout(this.timeout),
        catchError(this.handleError)
      );
  }

  // PUT request
  put<T>(endpoint: string, data: any): Observable<T> {
    const options = {
      headers: this.getHeaders()
    };

    return this.http.put<T>(`${this.baseUrl}/${endpoint}`, data, options)
      .pipe(
        timeout(this.timeout),
        catchError(this.handleError)
      );
  }

  // DELETE request
  delete<T>(endpoint: string): Observable<T> {
    const options = {
      headers: this.getHeaders()
    };

    return this.http.delete<T>(`${this.baseUrl}/${endpoint}`, options)
      .pipe(
        timeout(this.timeout),
        catchError(this.handleError)
      );
  }

  // File upload
  uploadFile(endpoint: string, file: File, additionalData?: any): Observable<any> {
    const formData = new FormData();
    formData.append('file', file);

    if (additionalData) {
      Object.keys(additionalData).forEach(key => {
        formData.append(key, additionalData[key]);
      });
    }

    const headers = new HttpHeaders();
    // Don't set Content-Type header for FormData, let browser set it

    return this.http.post(`${this.baseUrl}/${endpoint}`, formData, { headers })
      .pipe(
        timeout(30000), // Longer timeout for file uploads
        catchError(this.handleError)
      );
  }

  private getHeaders(): HttpHeaders {
    let headers = new HttpHeaders({
      'Content-Type': 'application/json'
    });

    // Add authorization token if available
    const token = localStorage.getItem('authToken');
    if (token) {
      headers = headers.set('Authorization', `Bearer ${token}`);
    }

    return headers;
  }

  private buildParams(params: any): HttpParams {
    let httpParams = new HttpParams();

    if (params) {
      Object.keys(params).forEach(key => {
        if (params[key] !== null && params[key] !== undefined) {
          httpParams = httpParams.set(key, params[key].toString());
        }
      });
    }

    return httpParams;
  }

  private handleError(error: HttpErrorResponse): Observable<never> {
    let errorMessage = 'An unknown error occurred';

    if (error.error instanceof ErrorEvent) {
      // Client-side error
      errorMessage = `Client Error: ${error.error.message}`;
    } else {
      // Server-side error
      switch (error.status) {
        case 400:
          errorMessage = 'Bad Request: ' + (error.error?.message || 'Invalid request');
          break;
        case 401:
          errorMessage = 'Unauthorized: Please log in';
          break;
        case 403:
          errorMessage = 'Forbidden: Access denied';
          break;
        case 404:
          errorMessage = 'Not Found: Resource does not exist';
          break;
        case 500:
          errorMessage = 'Internal Server Error: Please try again later';
          break;
        default:
          errorMessage = `Server Error: ${error.status} - ${error.error?.message || error.message}`;
      }
    }

    console.error('HTTP Error:', error);
    return throwError(() => new Error(errorMessage));
  }
}
```

### Specialized HTTP Services

```typescript
// user-api.service.ts
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';
import { ApiService } from './api.service';

export interface User {
  id: number;
  name: string;
  email: string;
  role: string;
  createdAt: string;
  updatedAt: string;
}

export interface CreateUserRequest {
  name: string;
  email: string;
  role: string;
}

export interface UpdateUserRequest {
  name?: string;
  email?: string;
  role?: string;
}

export interface UsersResponse {
  users: User[];
  total: number;
  page: number;
  limit: number;
}

@Injectable({
  providedIn: 'root'
})
export class UserApiService {
  private readonly endpoint = 'users';

  constructor(private apiService: ApiService) {}

  // Get all users with pagination
  getUsers(page = 1, limit = 10, search?: string): Observable<UsersResponse> {
    const params = { page, limit };
    if (search) {
      params['search'] = search;
    }

    return this.apiService.get<UsersResponse>(this.endpoint, params);
  }

  // Get user by ID
  getUserById(id: number): Observable<User> {
    return this.apiService.get<{ user: User }>(`${this.endpoint}/${id}`)
      .pipe(
        map(response => response.user)
      );
  }

  // Create new user
  createUser(userData: CreateUserRequest): Observable<User> {
    return this.apiService.post<{ user: User }>(this.endpoint, userData)
      .pipe(
        map(response => response.user)
      );
  }

  // Update user
  updateUser(id: number, userData: UpdateUserRequest): Observable<User> {
    return this.apiService.put<{ user: User }>(`${this.endpoint}/${id}`, userData)
      .pipe(
        map(response => response.user)
      );
  }

  // Delete user
  deleteUser(id: number): Observable<void> {
    return this.apiService.delete<void>(`${this.endpoint}/${id}`);
  }

  // Upload user avatar
  uploadAvatar(userId: number, file: File): Observable<{ avatarUrl: string }> {
    return this.apiService.uploadFile(`${this.endpoint}/${userId}/avatar`, file);
  }

  // Search users
  searchUsers(query: string): Observable<User[]> {
    return this.apiService.get<{ users: User[] }>(`${this.endpoint}/search`, { q: query })
      .pipe(
        map(response => response.users)
      );
  }

  // Get users by role
  getUsersByRole(role: string): Observable<User[]> {
    return this.apiService.get<{ users: User[] }>(this.endpoint, { role })
      .pipe(
        map(response => response.users)
      );
  }
}
```

### HTTP Interceptors

```typescript
// auth.interceptor.ts
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    // Get auth token from storage
    const authToken = localStorage.getItem('authToken');

    if (authToken) {
      // Clone the request and add authorization header
      const authReq = req.clone({
        headers: req.headers.set('Authorization', `Bearer ${authToken}`)
      });

      return next.handle(authReq);
    }

    return next.handle(req);
  }
}

// error.interceptor.ts
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';
import { Router } from '@angular/router';

@Injectable()
export class ErrorInterceptor implements HttpInterceptor {
  constructor(private router: Router) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    return next.handle(req).pipe(
      catchError((error: HttpErrorResponse) => {
        if (error.status === 401) {
          // Unauthorized - redirect to login
          localStorage.removeItem('authToken');
          this.router.navigate(['/login']);
        } else if (error.status === 403) {
          // Forbidden - redirect to access denied page
          this.router.navigate(['/access-denied']);
        }

        return throwError(() => error);
      })
    );
  }
}

// loading.interceptor.ts
import { Injectable } from '@angular/core';
import { HttpInterceptor, HttpRequest, HttpHandler, HttpEvent } from '@angular/common/http';
import { Observable } from 'rxjs';
import { finalize } from 'rxjs/operators';
import { LoadingService } from './loading.service';

@Injectable()
export class LoadingInterceptor implements HttpInterceptor {
  constructor(private loadingService: LoadingService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    // Show loading indicator
    this.loadingService.show();

    return next.handle(req).pipe(
      finalize(() => {
        // Hide loading indicator when request completes
        this.loadingService.hide();
      })
    );
  }
}

// Register interceptors in app.module.ts
@NgModule({
  providers: [
    { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true },
    { provide: HTTP_INTERCEPTORS, useClass: ErrorInterceptor, multi: true },
    { provide: HTTP_INTERCEPTORS, useClass: LoadingInterceptor, multi: true }
  ]
})
export class AppModule { }

## State Management Services

### Simple State Management

```typescript
// state.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable } from 'rxjs';
import { map, distinctUntilChanged } from 'rxjs/operators';

export interface AppState {
  user: User | null;
  isLoading: boolean;
  notifications: Notification[];
  theme: 'light' | 'dark';
  sidebarOpen: boolean;
}

const initialState: AppState = {
  user: null,
  isLoading: false,
  notifications: [],
  theme: 'light',
  sidebarOpen: false
};

@Injectable({
  providedIn: 'root'
})
export class StateService {
  private state$ = new BehaviorSubject<AppState>(initialState);

  // Get current state
  getState(): AppState {
    return this.state$.value;
  }

  // Get state as observable
  getState$(): Observable<AppState> {
    return this.state$.asObservable();
  }

  // Select specific part of state
  select<K extends keyof AppState>(key: K): Observable<AppState[K]> {
    return this.state$.pipe(
      map(state => state[key]),
      distinctUntilChanged()
    );
  }

  // Update state
  setState(partialState: Partial<AppState>): void {
    const currentState = this.getState();
    const newState = { ...currentState, ...partialState };
    this.state$.next(newState);
  }

  // Reset state
  resetState(): void {
    this.state$.next(initialState);
  }

  // Specific state updaters
  setUser(user: User | null): void {
    this.setState({ user });
  }

  setLoading(isLoading: boolean): void {
    this.setState({ isLoading });
  }

  addNotification(notification: Notification): void {
    const notifications = [...this.getState().notifications, notification];
    this.setState({ notifications });
  }

  removeNotification(id: string): void {
    const notifications = this.getState().notifications.filter(n => n.id !== id);
    this.setState({ notifications });
  }

  toggleSidebar(): void {
    this.setState({ sidebarOpen: !this.getState().sidebarOpen });
  }

  setTheme(theme: 'light' | 'dark'): void {
    this.setState({ theme });
  }
}

// Usage in component
@Component({
  selector: 'app-header',
  template: `
    <header class="header" [class.dark]="isDarkTheme$ | async">
      <button (click)="toggleSidebar()">Menu</button>
      <div *ngIf="user$ | async as user">
        Welcome, {{ user.name }}!
      </div>
      <div *ngIf="isLoading$ | async" class="loading">Loading...</div>
      <button (click)="toggleTheme()">Toggle Theme</button>
    </header>
  `
})
export class HeaderComponent {
  user$ = this.stateService.select('user');
  isLoading$ = this.stateService.select('isLoading');
  isDarkTheme$ = this.stateService.select('theme').pipe(
    map(theme => theme === 'dark')
  );

  constructor(private stateService: StateService) {}

  toggleSidebar(): void {
    this.stateService.toggleSidebar();
  }

  toggleTheme(): void {
    const currentTheme = this.stateService.getState().theme;
    const newTheme = currentTheme === 'light' ? 'dark' : 'light';
    this.stateService.setTheme(newTheme);
  }
}
```

### Advanced State Management with Actions

```typescript
// actions.ts
export enum ActionType {
  LOAD_USERS = '[User] Load Users',
  LOAD_USERS_SUCCESS = '[User] Load Users Success',
  LOAD_USERS_FAILURE = '[User] Load Users Failure',
  ADD_USER = '[User] Add User',
  UPDATE_USER = '[User] Update User',
  DELETE_USER = '[User] Delete User'
}

export interface Action {
  type: ActionType;
  payload?: any;
}

// user-store.service.ts
import { Injectable } from '@angular/core';
import { BehaviorSubject, Observable, of } from 'rxjs';
import { map, catchError, tap } from 'rxjs/operators';

interface UserState {
  users: User[];
  loading: boolean;
  error: string | null;
  selectedUser: User | null;
}

const initialUserState: UserState = {
  users: [],
  loading: false,
  error: null,
  selectedUser: null
};

@Injectable({
  providedIn: 'root'
})
export class UserStoreService {
  private state$ = new BehaviorSubject<UserState>(initialUserState);

  // Selectors
  users$ = this.state$.pipe(map(state => state.users));
  loading$ = this.state$.pipe(map(state => state.loading));
  error$ = this.state$.pipe(map(state => state.error));
  selectedUser$ = this.state$.pipe(map(state => state.selectedUser));

  constructor(private userApiService: UserApiService) {}

  // Actions
  loadUsers(): Observable<User[]> {
    this.updateState({ loading: true, error: null });

    return this.userApiService.getUsers().pipe(
      map(response => response.users),
      tap(users => {
        this.updateState({ users, loading: false });
      }),
      catchError(error => {
        this.updateState({ loading: false, error: error.message });
        return of([]);
      })
    );
  }

  addUser(userData: CreateUserRequest): Observable<User> {
    this.updateState({ loading: true, error: null });

    return this.userApiService.createUser(userData).pipe(
      tap(newUser => {
        const currentUsers = this.state$.value.users;
        this.updateState({
          users: [...currentUsers, newUser],
          loading: false
        });
      }),
      catchError(error => {
        this.updateState({ loading: false, error: error.message });
        throw error;
      })
    );
  }

  updateUser(id: number, userData: UpdateUserRequest): Observable<User> {
    this.updateState({ loading: true, error: null });

    return this.userApiService.updateUser(id, userData).pipe(
      tap(updatedUser => {
        const currentUsers = this.state$.value.users;
        const updatedUsers = currentUsers.map(user =>
          user.id === id ? updatedUser : user
        );
        this.updateState({
          users: updatedUsers,
          loading: false,
          selectedUser: this.state$.value.selectedUser?.id === id ? updatedUser : this.state$.value.selectedUser
        });
      }),
      catchError(error => {
        this.updateState({ loading: false, error: error.message });
        throw error;
      })
    );
  }

  deleteUser(id: number): Observable<void> {
    this.updateState({ loading: true, error: null });

    return this.userApiService.deleteUser(id).pipe(
      tap(() => {
        const currentUsers = this.state$.value.users;
        const filteredUsers = currentUsers.filter(user => user.id !== id);
        this.updateState({
          users: filteredUsers,
          loading: false,
          selectedUser: this.state$.value.selectedUser?.id === id ? null : this.state$.value.selectedUser
        });
      }),
      catchError(error => {
        this.updateState({ loading: false, error: error.message });
        throw error;
      })
    );
  }

  selectUser(user: User | null): void {
    this.updateState({ selectedUser: user });
  }

  clearError(): void {
    this.updateState({ error: null });
  }

  private updateState(partialState: Partial<UserState>): void {
    const currentState = this.state$.value;
    const newState = { ...currentState, ...partialState };
    this.state$.next(newState);
  }
}
```

### Caching Service

```typescript
// cache.service.ts
import { Injectable } from '@angular/core';
import { Observable, of, BehaviorSubject } from 'rxjs';
import { tap, shareReplay, map } from 'rxjs/operators';

interface CacheEntry<T> {
  data: T;
  timestamp: number;
  expiry: number;
}

@Injectable({
  providedIn: 'root'
})
export class CacheService {
  private cache = new Map<string, CacheEntry<any>>();
  private readonly DEFAULT_TTL = 5 * 60 * 1000; // 5 minutes

  // Set cache entry
  set<T>(key: string, data: T, ttl: number = this.DEFAULT_TTL): void {
    const entry: CacheEntry<T> = {
      data,
      timestamp: Date.now(),
      expiry: Date.now() + ttl
    };
    this.cache.set(key, entry);
  }

  // Get cache entry
  get<T>(key: string): T | null {
    const entry = this.cache.get(key);

    if (!entry) {
      return null;
    }

    // Check if expired
    if (Date.now() > entry.expiry) {
      this.cache.delete(key);
      return null;
    }

    return entry.data;
  }

  // Check if key exists and is valid
  has(key: string): boolean {
    return this.get(key) !== null;
  }

  // Delete cache entry
  delete(key: string): boolean {
    return this.cache.delete(key);
  }

  // Clear all cache
  clear(): void {
    this.cache.clear();
  }

  // Get or set pattern
  getOrSet<T>(
    key: string,
    factory: () => Observable<T>,
    ttl: number = this.DEFAULT_TTL
  ): Observable<T> {
    const cached = this.get<T>(key);

    if (cached !== null) {
      return of(cached);
    }

    return factory().pipe(
      tap(data => this.set(key, data, ttl)),
      shareReplay(1)
    );
  }

  // Cache with observable
  cacheObservable<T>(
    key: string,
    source$: Observable<T>,
    ttl: number = this.DEFAULT_TTL
  ): Observable<T> {
    return this.getOrSet(key, () => source$, ttl);
  }

  // Get cache statistics
  getStats(): { size: number; keys: string[] } {
    return {
      size: this.cache.size,
      keys: Array.from(this.cache.keys())
    };
  }

  // Clean expired entries
  cleanExpired(): number {
    let cleaned = 0;
    const now = Date.now();

    for (const [key, entry] of this.cache.entries()) {
      if (now > entry.expiry) {
        this.cache.delete(key);
        cleaned++;
      }
    }

    return cleaned;
  }
}

// Enhanced API service with caching
@Injectable({
  providedIn: 'root'
})
export class CachedUserService {
  constructor(
    private userApiService: UserApiService,
    private cacheService: CacheService
  ) {}

  getUsers(page = 1, limit = 10, useCache = true): Observable<UsersResponse> {
    const cacheKey = `users_${page}_${limit}`;

    if (useCache) {
      return this.cacheService.getOrSet(
        cacheKey,
        () => this.userApiService.getUsers(page, limit),
        2 * 60 * 1000 // 2 minutes cache
      );
    }

    return this.userApiService.getUsers(page, limit);
  }

  getUserById(id: number, useCache = true): Observable<User> {
    const cacheKey = `user_${id}`;

    if (useCache) {
      return this.cacheService.getOrSet(
        cacheKey,
        () => this.userApiService.getUserById(id),
        5 * 60 * 1000 // 5 minutes cache
      );
    }

    return this.userApiService.getUserById(id);
  }

  createUser(userData: CreateUserRequest): Observable<User> {
    return this.userApiService.createUser(userData).pipe(
      tap(() => {
        // Invalidate users list cache
        this.invalidateUsersCache();
      })
    );
  }

  updateUser(id: number, userData: UpdateUserRequest): Observable<User> {
    return this.userApiService.updateUser(id, userData).pipe(
      tap(() => {
        // Invalidate specific user and users list cache
        this.cacheService.delete(`user_${id}`);
        this.invalidateUsersCache();
      })
    );
  }

  deleteUser(id: number): Observable<void> {
    return this.userApiService.deleteUser(id).pipe(
      tap(() => {
        // Invalidate specific user and users list cache
        this.cacheService.delete(`user_${id}`);
        this.invalidateUsersCache();
      })
    );
  }

  private invalidateUsersCache(): void {
    const stats = this.cacheService.getStats();
    stats.keys
      .filter(key => key.startsWith('users_'))
      .forEach(key => this.cacheService.delete(key));
  }

  clearCache(): void {
    this.cacheService.clear();
  }
}

## Observable Patterns in Services

### Subject-based Communication

```typescript
// notification.service.ts
import { Injectable } from '@angular/core';
import { Subject, BehaviorSubject, ReplaySubject, Observable } from 'rxjs';
import { filter, map } from 'rxjs/operators';

export interface Notification {
  id: string;
  type: 'success' | 'error' | 'warning' | 'info';
  message: string;
  duration?: number;
  timestamp: Date;
}

@Injectable({
  providedIn: 'root'
})
export class NotificationService {
  // Subject for new notifications
  private notificationSubject = new Subject<Notification>();

  // BehaviorSubject for current notifications list
  private notificationsSubject = new BehaviorSubject<Notification[]>([]);

  // ReplaySubject for notification history (last 50)
  private historySubject = new ReplaySubject<Notification>(50);

  // Public observables
  notification$ = this.notificationSubject.asObservable();
  notifications$ = this.notificationsSubject.asObservable();
  history$ = this.historySubject.asObservable();

  // Filtered observables
  errors$ = this.notification$.pipe(
    filter(notification => notification.type === 'error')
  );

  successes$ = this.notification$.pipe(
    filter(notification => notification.type === 'success')
  );

  constructor() {
    // Auto-remove notifications after duration
    this.notification$.subscribe(notification => {
      if (notification.duration && notification.duration > 0) {
        setTimeout(() => {
          this.remove(notification.id);
        }, notification.duration);
      }
    });
  }

  // Show notification
  show(
    message: string,
    type: Notification['type'] = 'info',
    duration = 5000
  ): string {
    const notification: Notification = {
      id: this.generateId(),
      type,
      message,
      duration,
      timestamp: new Date()
    };

    // Add to current notifications
    const current = this.notificationsSubject.value;
    this.notificationsSubject.next([...current, notification]);

    // Emit new notification
    this.notificationSubject.next(notification);

    // Add to history
    this.historySubject.next(notification);

    return notification.id;
  }

  // Convenience methods
  success(message: string, duration = 3000): string {
    return this.show(message, 'success', duration);
  }

  error(message: string, duration = 0): string {
    return this.show(message, 'error', duration); // No auto-dismiss for errors
  }

  warning(message: string, duration = 4000): string {
    return this.show(message, 'warning', duration);
  }

  info(message: string, duration = 5000): string {
    return this.show(message, 'info', duration);
  }

  // Remove notification
  remove(id: string): void {
    const current = this.notificationsSubject.value;
    const filtered = current.filter(n => n.id !== id);
    this.notificationsSubject.next(filtered);
  }

  // Clear all notifications
  clear(): void {
    this.notificationsSubject.next([]);
  }

  // Get current count
  getCount(): number {
    return this.notificationsSubject.value.length;
  }

  // Get notifications by type
  getByType(type: Notification['type']): Observable<Notification[]> {
    return this.notifications$.pipe(
      map(notifications => notifications.filter(n => n.type === type))
    );
  }

  private generateId(): string {
    return Math.random().toString(36).substr(2, 9);
  }
}

// Usage in component
@Component({
  selector: 'app-notification-demo',
  template: `
    <div class="demo">
      <button (click)="showSuccess()">Success</button>
      <button (click)="showError()">Error</button>
      <button (click)="showWarning()">Warning</button>
      <button (click)="showInfo()">Info</button>
      <button (click)="clearAll()">Clear All</button>

      <div class="notifications">
        <div *ngFor="let notification of notifications$ | async"
             class="notification"
             [class]="'notification-' + notification.type">
          {{ notification.message }}
          <button (click)="remove(notification.id)">×</button>
        </div>
      </div>
    </div>
  `
})
export class NotificationDemoComponent {
  notifications$ = this.notificationService.notifications$;

  constructor(private notificationService: NotificationService) {}

  showSuccess(): void {
    this.notificationService.success('Operation completed successfully!');
  }

  showError(): void {
    this.notificationService.error('An error occurred!');
  }

  showWarning(): void {
    this.notificationService.warning('Please review your input.');
  }

  showInfo(): void {
    this.notificationService.info('Here is some information.');
  }

  remove(id: string): void {
    this.notificationService.remove(id);
  }

  clearAll(): void {
    this.notificationService.clear();
  }
}
```

### WebSocket Service

```typescript
// websocket.service.ts
import { Injectable } from '@angular/core';
import { Observable, Subject, BehaviorSubject, timer } from 'rxjs';
import { filter, map, retryWhen, delay, take } from 'rxjs/operators';

export interface WebSocketMessage {
  type: string;
  payload: any;
  timestamp: number;
}

@Injectable({
  providedIn: 'root'
})
export class WebSocketService {
  private socket: WebSocket | null = null;
  private messageSubject = new Subject<WebSocketMessage>();
  private connectionSubject = new BehaviorSubject<boolean>(false);
  private reconnectAttempts = 0;
  private maxReconnectAttempts = 5;
  private reconnectInterval = 5000;

  // Public observables
  messages$ = this.messageSubject.asObservable();
  connected$ = this.connectionSubject.asObservable();

  constructor() {}

  // Connect to WebSocket
  connect(url: string): Observable<boolean> {
    if (this.socket && this.socket.readyState === WebSocket.OPEN) {
      return this.connected$;
    }

    this.socket = new WebSocket(url);

    this.socket.onopen = () => {
      console.log('WebSocket connected');
      this.connectionSubject.next(true);
      this.reconnectAttempts = 0;
    };

    this.socket.onmessage = (event) => {
      try {
        const message: WebSocketMessage = JSON.parse(event.data);
        this.messageSubject.next(message);
      } catch (error) {
        console.error('Error parsing WebSocket message:', error);
      }
    };

    this.socket.onclose = () => {
      console.log('WebSocket disconnected');
      this.connectionSubject.next(false);
      this.attemptReconnect(url);
    };

    this.socket.onerror = (error) => {
      console.error('WebSocket error:', error);
    };

    return this.connected$;
  }

  // Send message
  send(message: Omit<WebSocketMessage, 'timestamp'>): void {
    if (this.socket && this.socket.readyState === WebSocket.OPEN) {
      const fullMessage: WebSocketMessage = {
        ...message,
        timestamp: Date.now()
      };
      this.socket.send(JSON.stringify(fullMessage));
    } else {
      console.warn('WebSocket is not connected');
    }
  }

  // Listen for specific message types
  listen<T>(messageType: string): Observable<T> {
    return this.messages$.pipe(
      filter(message => message.type === messageType),
      map(message => message.payload as T)
    );
  }

  // Disconnect
  disconnect(): void {
    if (this.socket) {
      this.socket.close();
      this.socket = null;
    }
  }

  private attemptReconnect(url: string): void {
    if (this.reconnectAttempts < this.maxReconnectAttempts) {
      this.reconnectAttempts++;
      console.log(`Attempting to reconnect... (${this.reconnectAttempts}/${this.maxReconnectAttempts})`);

      timer(this.reconnectInterval).subscribe(() => {
        this.connect(url);
      });
    } else {
      console.error('Max reconnection attempts reached');
    }
  }
}

// Chat service using WebSocket
@Injectable({
  providedIn: 'root'
})
export class ChatService {
  private readonly WS_URL = 'ws://localhost:8080/chat';

  constructor(private webSocketService: WebSocketService) {
    this.webSocketService.connect(this.WS_URL);
  }

  // Send chat message
  sendMessage(message: string, userId: string): void {
    this.webSocketService.send({
      type: 'chat_message',
      payload: {
        message,
        userId,
        timestamp: Date.now()
      }
    });
  }

  // Listen for chat messages
  getChatMessages(): Observable<any> {
    return this.webSocketService.listen('chat_message');
  }

  // Listen for user joined/left events
  getUserEvents(): Observable<any> {
    return this.webSocketService.listen('user_event');
  }

  // Get connection status
  getConnectionStatus(): Observable<boolean> {
    return this.webSocketService.connected$;
  }
}
```

## Testing Services

### Unit Testing Services

```typescript
// user.service.spec.ts
import { TestBed } from '@angular/core/testing';
import { HttpClientTestingModule, HttpTestingController } from '@angular/common/http/testing';
import { UserApiService, User, CreateUserRequest } from './user-api.service';
import { ApiService } from './api.service';

describe('UserApiService', () => {
  let service: UserApiService;
  let httpMock: HttpTestingController;
  let apiService: jasmine.SpyObj<ApiService>;

  const mockUsers: User[] = [
    { id: 1, name: 'John Doe', email: 'john@example.com', role: 'admin', createdAt: '2023-01-01', updatedAt: '2023-01-01' },
    { id: 2, name: 'Jane Smith', email: 'jane@example.com', role: 'user', createdAt: '2023-01-02', updatedAt: '2023-01-02' }
  ];

  beforeEach(() => {
    const apiServiceSpy = jasmine.createSpyObj('ApiService', ['get', 'post', 'put', 'delete']);

    TestBed.configureTestingModule({
      imports: [HttpClientTestingModule],
      providers: [
        UserApiService,
        { provide: ApiService, useValue: apiServiceSpy }
      ]
    });

    service = TestBed.inject(UserApiService);
    apiService = TestBed.inject(ApiService) as jasmine.SpyObj<ApiService>;
  });

  it('should be created', () => {
    expect(service).toBeTruthy();
  });

  describe('getUsers', () => {
    it('should return users', (done) => {
      const mockResponse = { users: mockUsers, total: 2, page: 1, limit: 10 };
      apiService.get.and.returnValue(of(mockResponse));

      service.getUsers().subscribe(response => {
        expect(response).toEqual(mockResponse);
        expect(apiService.get).toHaveBeenCalledWith('users', { page: 1, limit: 10 });
        done();
      });
    });

    it('should include search parameter when provided', (done) => {
      const mockResponse = { users: [mockUsers[0]], total: 1, page: 1, limit: 10 };
      apiService.get.and.returnValue(of(mockResponse));

      service.getUsers(1, 10, 'John').subscribe(() => {
        expect(apiService.get).toHaveBeenCalledWith('users', { page: 1, limit: 10, search: 'John' });
        done();
      });
    });
  });

  describe('getUserById', () => {
    it('should return a single user', (done) => {
      const mockResponse = { user: mockUsers[0] };
      apiService.get.and.returnValue(of(mockResponse));

      service.getUserById(1).subscribe(user => {
        expect(user).toEqual(mockUsers[0]);
        expect(apiService.get).toHaveBeenCalledWith('users/1');
        done();
      });
    });
  });

  describe('createUser', () => {
    it('should create a new user', (done) => {
      const newUserData: CreateUserRequest = { name: 'New User', email: 'new@example.com', role: 'user' };
      const mockResponse = { user: { ...newUserData, id: 3, createdAt: '2023-01-03', updatedAt: '2023-01-03' } };
      apiService.post.and.returnValue(of(mockResponse));

      service.createUser(newUserData).subscribe(user => {
        expect(user).toEqual(mockResponse.user);
        expect(apiService.post).toHaveBeenCalledWith('users', newUserData);
        done();
      });
    });
  });

  describe('deleteUser', () => {
    it('should delete a user', (done) => {
      apiService.delete.and.returnValue(of(undefined));

      service.deleteUser(1).subscribe(() => {
        expect(apiService.delete).toHaveBeenCalledWith('users/1');
        done();
      });
    });
  });
});

// Testing services with dependencies
describe('UserStoreService', () => {
  let service: UserStoreService;
  let userApiService: jasmine.SpyObj<UserApiService>;

  beforeEach(() => {
    const userApiServiceSpy = jasmine.createSpyObj('UserApiService', ['getUsers', 'createUser', 'updateUser', 'deleteUser']);

    TestBed.configureTestingModule({
      providers: [
        UserStoreService,
        { provide: UserApiService, useValue: userApiServiceSpy }
      ]
    });

    service = TestBed.inject(UserStoreService);
    userApiService = TestBed.inject(UserApiService) as jasmine.SpyObj<UserApiService>;
  });

  it('should load users and update state', (done) => {
    const mockResponse = { users: mockUsers, total: 2, page: 1, limit: 10 };
    userApiService.getUsers.and.returnValue(of(mockResponse));

    service.loadUsers().subscribe(() => {
      service.users$.subscribe(users => {
        expect(users).toEqual(mockUsers);
        done();
      });
    });
  });

  it('should handle errors when loading users', (done) => {
    const errorMessage = 'Failed to load users';
    userApiService.getUsers.and.returnValue(throwError(() => new Error(errorMessage)));

    service.loadUsers().subscribe(() => {
      service.error$.subscribe(error => {
        expect(error).toBe(errorMessage);
        done();
      });
    });
  });
});

## Best Practices

### Service Design Principles

```typescript
// ✅ Good: Single Responsibility Principle
@Injectable({
  providedIn: 'root'
})
export class UserService {
  // Only handles user-related operations
  getUsers(): Observable<User[]> { /* ... */ }
  createUser(user: CreateUserRequest): Observable<User> { /* ... */ }
  updateUser(id: number, user: UpdateUserRequest): Observable<User> { /* ... */ }
  deleteUser(id: number): Observable<void> { /* ... */ }
}

@Injectable({
  providedIn: 'root'
})
export class AuthService {
  // Only handles authentication
  login(credentials: LoginRequest): Observable<AuthResponse> { /* ... */ }
  logout(): Observable<void> { /* ... */ }
  refreshToken(): Observable<string> { /* ... */ }
  isAuthenticated(): boolean { /* ... */ }
}

// ✅ Good: Immutable State Updates
@Injectable({
  providedIn: 'root'
})
export class TodoService {
  private todosSubject = new BehaviorSubject<Todo[]>([]);
  todos$ = this.todosSubject.asObservable();

  addTodo(todo: Todo): void {
    const currentTodos = this.todosSubject.value;
    // Create new array instead of mutating existing one
    this.todosSubject.next([...currentTodos, todo]);
  }

  updateTodo(id: number, updates: Partial<Todo>): void {
    const currentTodos = this.todosSubject.value;
    const updatedTodos = currentTodos.map(todo =>
      todo.id === id ? { ...todo, ...updates } : todo
    );
    this.todosSubject.next(updatedTodos);
  }
}

// ✅ Good: Error Handling
@Injectable({
  providedIn: 'root'
})
export class DataService {
  constructor(private http: HttpClient) {}

  getData(): Observable<any> {
    return this.http.get('/api/data').pipe(
      retry(2),
      timeout(10000),
      catchError(this.handleError)
    );
  }

  private handleError(error: HttpErrorResponse): Observable<never> {
    let errorMessage = 'Unknown error occurred';

    if (error.error instanceof ErrorEvent) {
      errorMessage = `Client Error: ${error.error.message}`;
    } else {
      errorMessage = `Server Error: ${error.status} - ${error.message}`;
    }

    console.error(errorMessage);
    return throwError(() => new Error(errorMessage));
  }
}

// ✅ Good: Resource Cleanup
@Injectable({
  providedIn: 'root'
})
export class SubscriptionService implements OnDestroy {
  private destroy$ = new Subject<void>();

  constructor(private dataService: DataService) {
    this.dataService.getData()
      .pipe(takeUntil(this.destroy$))
      .subscribe(data => {
        // Handle data
      });
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

### Performance Optimization

```typescript
// ✅ Use shareReplay for expensive operations
@Injectable({
  providedIn: 'root'
})
export class ConfigService {
  private config$ = this.http.get<Config>('/api/config').pipe(
    shareReplay(1) // Cache the result and share with all subscribers
  );

  getConfig(): Observable<Config> {
    return this.config$;
  }
}

// ✅ Implement proper caching
@Injectable({
  providedIn: 'root'
})
export class CachedDataService {
  private cache = new Map<string, Observable<any>>();

  getData(id: string): Observable<any> {
    if (!this.cache.has(id)) {
      const data$ = this.http.get(`/api/data/${id}`).pipe(
        shareReplay(1),
        // Remove from cache after 5 minutes
        finalize(() => {
          setTimeout(() => this.cache.delete(id), 5 * 60 * 1000);
        })
      );
      this.cache.set(id, data$);
    }

    return this.cache.get(id)!;
  }
}

// ✅ Use OnPush-friendly patterns
@Injectable({
  providedIn: 'root'
})
export class StateService {
  private state$ = new BehaviorSubject<AppState>(initialState);

  // Return new state object for OnPush detection
  updateState(updates: Partial<AppState>): void {
    const currentState = this.state$.value;
    const newState = { ...currentState, ...updates };
    this.state$.next(newState);
  }

  // Provide specific selectors
  selectUser(): Observable<User | null> {
    return this.state$.pipe(
      map(state => state.user),
      distinctUntilChanged()
    );
  }
}
```

### Service Communication Patterns

```typescript
// ✅ Event Bus Pattern
@Injectable({
  providedIn: 'root'
})
export class EventBusService {
  private eventSubject = new Subject<{ type: string; payload?: any }>();

  emit(type: string, payload?: any): void {
    this.eventSubject.next({ type, payload });
  }

  on(type: string): Observable<any> {
    return this.eventSubject.pipe(
      filter(event => event.type === type),
      map(event => event.payload)
    );
  }
}

// ✅ Facade Pattern
@Injectable({
  providedIn: 'root'
})
export class UserFacadeService {
  constructor(
    private userApiService: UserApiService,
    private userStoreService: UserStoreService,
    private notificationService: NotificationService
  ) {}

  // Simplified API for components
  async loadUsers(): Promise<void> {
    try {
      await this.userStoreService.loadUsers().toPromise();
      this.notificationService.success('Users loaded successfully');
    } catch (error) {
      this.notificationService.error('Failed to load users');
      throw error;
    }
  }

  async createUser(userData: CreateUserRequest): Promise<User> {
    try {
      const user = await this.userStoreService.addUser(userData).toPromise();
      this.notificationService.success(`User ${user.name} created successfully`);
      return user;
    } catch (error) {
      this.notificationService.error('Failed to create user');
      throw error;
    }
  }
}
```

## Common Pitfalls

### Issue 1: Memory Leaks from Subscriptions
Not properly unsubscribing from observables in services.

**Solution:**
```typescript
// ❌ Bad: No cleanup
@Injectable()
export class BadService {
  constructor(private dataService: DataService) {
    this.dataService.getData().subscribe(data => {
      // This subscription will never be cleaned up
    });
  }
}

// ✅ Good: Proper cleanup
@Injectable()
export class GoodService implements OnDestroy {
  private destroy$ = new Subject<void>();

  constructor(private dataService: DataService) {
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
}
```

### Issue 2: Mutating State Directly
Directly mutating state objects can cause change detection issues.

**Solution:**
```typescript
// ❌ Bad: Direct mutation
@Injectable()
export class BadStateService {
  private state = { users: [], loading: false };

  addUser(user: User): void {
    this.state.users.push(user); // Mutates existing array
  }
}

// ✅ Good: Immutable updates
@Injectable()
export class GoodStateService {
  private stateSubject = new BehaviorSubject({ users: [], loading: false });

  addUser(user: User): void {
    const currentState = this.stateSubject.value;
    this.stateSubject.next({
      ...currentState,
      users: [...currentState.users, user] // Creates new array
    });
  }
}
```

### Issue 3: Not Handling HTTP Errors
Failing to properly handle HTTP errors can crash the application.

**Solution:**
```typescript
// ❌ Bad: No error handling
@Injectable()
export class BadApiService {
  getData(): Observable<any> {
    return this.http.get('/api/data');
    // Errors will propagate and potentially crash the app
  }
}

// ✅ Good: Proper error handling
@Injectable()
export class GoodApiService {
  getData(): Observable<any> {
    return this.http.get('/api/data').pipe(
      catchError(error => {
        console.error('API Error:', error);
        return throwError(() => new Error('Failed to fetch data'));
      })
    );
  }
}
```

### Issue 4: Creating Multiple Service Instances
Accidentally creating multiple instances of singleton services.

**Solution:**
```typescript
// ❌ Bad: Providing service in multiple places
@Component({
  providers: [UserService] // Creates new instance for this component
})
export class BadComponent { }

// ✅ Good: Use providedIn: 'root' for singletons
@Injectable({
  providedIn: 'root' // Single instance across the app
})
export class UserService { }

// Or provide in module only
@NgModule({
  providers: [UserService] // Single instance in this module
})
export class AppModule { }
```

### Issue 5: Blocking Operations in Services
Performing synchronous operations that block the UI.

**Solution:**
```typescript
// ❌ Bad: Synchronous heavy operation
@Injectable()
export class BadService {
  processData(data: any[]): any[] {
    // Heavy synchronous processing
    return data.map(item => {
      // Complex calculations that block UI
      for (let i = 0; i < 1000000; i++) {
        // Heavy computation
      }
      return processedItem;
    });
  }
}

// ✅ Good: Asynchronous processing
@Injectable()
export class GoodService {
  processData(data: any[]): Observable<any[]> {
    return new Observable(observer => {
      // Use setTimeout to avoid blocking UI
      setTimeout(() => {
        const processed = data.map(item => {
          // Process item
          return processedItem;
        });
        observer.next(processed);
        observer.complete();
      }, 0);
    });
  }

  // Or use Web Workers for heavy computations
  processDataWithWorker(data: any[]): Observable<any[]> {
    return new Observable(observer => {
      const worker = new Worker('./data-processor.worker', { type: 'module' });
      worker.postMessage(data);

      worker.onmessage = ({ data }) => {
        observer.next(data);
        observer.complete();
        worker.terminate();
      };

      worker.onerror = (error) => {
        observer.error(error);
        worker.terminate();
      };
    });
  }
}
```

## References

{% embed url="https://angular.io/guide/architecture-services" %}

{% embed url="https://angular.io/guide/dependency-injection" %}

{% embed url="https://angular.io/guide/http" %}

{% embed url="https://angular.io/guide/observables" %}

{% embed url="https://angular.io/guide/testing" %}
```
```
```
```
