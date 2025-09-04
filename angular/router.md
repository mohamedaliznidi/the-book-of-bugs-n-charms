---
description: >-
  Angular Router Portal Navigation enables mystical travel between views and component
  enchantments in single-page applications. Master routing configuration spells,
  navigation magic, route guardian rituals, and lazy loading sorcery.
---

# Angular Router Portal Navigation

## The Ancient Knowledge

The Angular Router is a powerful mystical navigation library that enables you to travel from one view to the next as users perform application tasks. It interprets a browser URL as an instruction to navigate to a client-generated view and can pass optional parameters along to the supporting view component to help it decide what specific content to manifest.

## Key Mystical Concepts

### Router Portal Components
- **Router**: The main service that enables mystical navigation
- **Route**: Defines how the router should navigate to a component based on a URL pattern spell
- **Routes**: An array of Route objects that define the routing configuration enchantments
- **RouterOutlet**: A directive that acts as a mystical placeholder for routed components
- **RouterLink**: A directive for creating navigation link spells
- **ActivatedRoute**: Provides mystical information about the active route

## Basic Portal Setup

### Installing Router Magic

```bash
# Router is included by default when creating a new Angular project with routing magic
ng new my-app --routing

# Or add routing magic to existing project
ng generate module app-routing --flat --module=app
```

### Basic Routing Configuration

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';
import { ContactComponent } from './contact/contact.component';
import { PageNotFoundComponent } from './page-not-found/page-not-found.component';

const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'contact', component: ContactComponent },
  { path: '**', component: PageNotFoundComponent } // Wildcard route - must be last
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule, AppRoutingModule],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

```html
<!-- app.component.html -->
<nav class="navbar">
  <a routerLink="/home" routerLinkActive="active">Home</a>
  <a routerLink="/about" routerLinkActive="active">About</a>
  <a routerLink="/contact" routerLinkActive="active">Contact</a>
</nav>

<main>
  <router-outlet></router-outlet>
</main>
```

```css
/* app.component.css */
.navbar {
  display: flex;
  gap: 16px;
  padding: 16px;
  background-color: #f5f5f5;
  border-bottom: 1px solid #ddd;
}

.navbar a {
  text-decoration: none;
  padding: 8px 16px;
  border-radius: 4px;
  color: #333;
  transition: background-color 0.3s;
}

.navbar a:hover {
  background-color: #e0e0e0;
}

.navbar a.active {
  background-color: #2196f3;
  color: white;
}
```

## Route Mystical Parameters

### Path Parameter Spells

```typescript
// Routes with parameters
const routes: Routes = [
  { path: 'user/:id', component: UserDetailComponent },
  { path: 'user/:id/edit', component: UserEditComponent },
  { path: 'product/:category/:id', component: ProductDetailComponent }
];

// user-detail.component.ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, ParamMap } from '@angular/router';
import { switchMap } from 'rxjs/operators';

@Component({
  selector: 'app-user-detail',
  template: `
    <div class="user-detail" *ngIf="user">
      <h2>{{ user.name }}</h2>
      <p>Email: {{ user.email }}</p>
      <p>Role: {{ user.role }}</p>
      
      <div class="actions">
        <button (click)="editUser()">Edit User</button>
        <button (click)="goBack()">Go Back</button>
      </div>
    </div>
    
    <div *ngIf="!user" class="loading">
      Loading user...
    </div>
  `
})
export class UserDetailComponent implements OnInit {
  user: User | null = null;

  constructor(
    private route: ActivatedRoute,
    private router: Router,
    private userService: UserService
  ) {}

  ngOnInit(): void {
    // Method 1: Using snapshot (for one-time access)
    const id = this.route.snapshot.paramMap.get('id');
    if (id) {
      this.loadUser(+id);
    }

    // Method 2: Using observable (for reactive updates)
    this.route.paramMap.pipe(
      switchMap((params: ParamMap) => {
        const userId = params.get('id');
        return userId ? this.userService.getUserById(+userId) : of(null);
      })
    ).subscribe(user => {
      this.user = user;
    });
  }

  private loadUser(id: number): void {
    this.userService.getUserById(id).subscribe(user => {
      this.user = user;
    });
  }

  editUser(): void {
    if (this.user) {
      this.router.navigate(['/user', this.user.id, 'edit']);
    }
  }

  goBack(): void {
    this.router.navigate(['/users']);
  }
}
```

### Query Parameters

```typescript
// Navigation with query parameters
@Component({
  selector: 'app-user-list',
  template: `
    <div class="filters">
      <input [(ngModel)]="searchTerm" placeholder="Search users">
      <select [(ngModel)]="roleFilter">
        <option value="">All Roles</option>
        <option value="admin">Admin</option>
        <option value="user">User</option>
      </select>
      <button (click)="applyFilters()">Apply Filters</button>
    </div>
    
    <div class="users">
      <div *ngFor="let user of filteredUsers" class="user-card">
        <h3>{{ user.name }}</h3>
        <p>{{ user.email }}</p>
        <button (click)="viewUser(user.id)">View Details</button>
      </div>
    </div>
    
    <div class="pagination">
      <button (click)="previousPage()" [disabled]="currentPage === 1">Previous</button>
      <span>Page {{ currentPage }} of {{ totalPages }}</span>
      <button (click)="nextPage()" [disabled]="currentPage === totalPages">Next</button>
    </div>
  `
})
export class UserListComponent implements OnInit {
  users: User[] = [];
  filteredUsers: User[] = [];
  searchTerm = '';
  roleFilter = '';
  currentPage = 1;
  pageSize = 10;
  totalPages = 1;

  constructor(
    private route: ActivatedRoute,
    private router: Router,
    private userService: UserService
  ) {}

  ngOnInit(): void {
    // Read query parameters on component initialization
    this.route.queryParams.subscribe(params => {
      this.searchTerm = params['search'] || '';
      this.roleFilter = params['role'] || '';
      this.currentPage = +params['page'] || 1;
      this.pageSize = +params['size'] || 10;
      
      this.loadUsers();
    });
  }

  applyFilters(): void {
    // Navigate with query parameters
    this.router.navigate([], {
      relativeTo: this.route,
      queryParams: {
        search: this.searchTerm || null,
        role: this.roleFilter || null,
        page: 1 // Reset to first page when filtering
      },
      queryParamsHandling: 'merge' // Merge with existing query params
    });
  }

  viewUser(id: number): void {
    this.router.navigate(['/user', id], {
      queryParams: { returnUrl: this.router.url }
    });
  }

  previousPage(): void {
    if (this.currentPage > 1) {
      this.navigateToPage(this.currentPage - 1);
    }
  }

  nextPage(): void {
    if (this.currentPage < this.totalPages) {
      this.navigateToPage(this.currentPage + 1);
    }
  }

  private navigateToPage(page: number): void {
    this.router.navigate([], {
      relativeTo: this.route,
      queryParams: { page },
      queryParamsHandling: 'merge'
    });
  }

  private loadUsers(): void {
    this.userService.getUsers({
      search: this.searchTerm,
      role: this.roleFilter,
      page: this.currentPage,
      size: this.pageSize
    }).subscribe(response => {
      this.users = response.users;
      this.filteredUsers = response.users;
      this.totalPages = Math.ceil(response.total / this.pageSize);
    });
  }
}
```

### Fragment and Preserve Query Params

```typescript
// Navigation with fragments and query param preservation
@Component({
  template: `
    <nav>
      <a [routerLink]="['/docs']" fragment="introduction">Introduction</a>
      <a [routerLink]="['/docs']" fragment="getting-started">Getting Started</a>
      <a [routerLink]="['/docs']" fragment="advanced">Advanced</a>
    </nav>
  `
})
export class NavigationComponent {
  constructor(private router: Router) {}

  navigateWithFragment(): void {
    this.router.navigate(['/docs'], { 
      fragment: 'section-1',
      queryParamsHandling: 'preserve' // Preserve existing query params
    });
  }

  navigateWithState(): void {
    this.router.navigate(['/user', 123], {
      state: { 
        returnUrl: '/users',
        previousSearch: 'john doe'
      }
    });
  }
}

// Accessing navigation state
@Component({})
export class UserDetailComponent implements OnInit {
  constructor(private router: Router) {}

  ngOnInit(): void {
    // Access navigation state
    const navigation = this.router.getCurrentNavigation();
    if (navigation?.extras.state) {
      const state = navigation.extras.state;
      console.log('Return URL:', state['returnUrl']);
      console.log('Previous Search:', state['previousSearch']);
    }
  }
}

## Nested Portal Routes

### Child Route Configuration Spells

```typescript
// app-routing.module.ts
const routes: Routes = [
  {
    path: 'dashboard',
    component: DashboardComponent,
    children: [
      { path: '', redirectTo: 'overview', pathMatch: 'full' },
      { path: 'overview', component: OverviewComponent },
      { path: 'analytics', component: AnalyticsComponent },
      { path: 'reports', component: ReportsComponent },
      {
        path: 'users',
        component: UsersComponent,
        children: [
          { path: '', component: UserListComponent },
          { path: ':id', component: UserDetailComponent },
          { path: ':id/edit', component: UserEditComponent }
        ]
      }
    ]
  }
];

// dashboard.component.ts
@Component({
  selector: 'app-dashboard',
  template: `
    <div class="dashboard">
      <aside class="sidebar">
        <nav class="nav-menu">
          <a routerLink="overview" routerLinkActive="active">Overview</a>
          <a routerLink="analytics" routerLinkActive="active">Analytics</a>
          <a routerLink="reports" routerLinkActive="active">Reports</a>
          <a routerLink="users" routerLinkActive="active">Users</a>
        </nav>
      </aside>

      <main class="content">
        <router-outlet></router-outlet>
      </main>
    </div>
  `,
  styles: [`
    .dashboard {
      display: flex;
      height: 100vh;
    }
    .sidebar {
      width: 250px;
      background-color: #f5f5f5;
      padding: 20px;
    }
    .nav-menu {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    .nav-menu a {
      padding: 12px 16px;
      text-decoration: none;
      color: #333;
      border-radius: 4px;
      transition: background-color 0.3s;
    }
    .nav-menu a:hover {
      background-color: #e0e0e0;
    }
    .nav-menu a.active {
      background-color: #2196f3;
      color: white;
    }
    .content {
      flex: 1;
      padding: 20px;
      overflow-y: auto;
    }
  `]
})
export class DashboardComponent { }

// users.component.ts (nested component with its own router-outlet)
@Component({
  selector: 'app-users',
  template: `
    <div class="users-section">
      <header class="section-header">
        <h2>User Management</h2>
        <nav class="sub-nav">
          <a routerLink="./" routerLinkActive="active" [routerLinkActiveOptions]="{exact: true}">
            All Users
          </a>
          <a routerLink="./new" routerLinkActive="active">Add User</a>
        </nav>
      </header>

      <div class="section-content">
        <router-outlet></router-outlet>
      </div>
    </div>
  `,
  styles: [`
    .section-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 20px;
      padding-bottom: 16px;
      border-bottom: 1px solid #ddd;
    }
    .sub-nav {
      display: flex;
      gap: 16px;
    }
    .sub-nav a {
      padding: 8px 16px;
      text-decoration: none;
      color: #666;
      border: 1px solid #ddd;
      border-radius: 4px;
      transition: all 0.3s;
    }
    .sub-nav a:hover {
      background-color: #f0f0f0;
    }
    .sub-nav a.active {
      background-color: #2196f3;
      color: white;
      border-color: #2196f3;
    }
  `]
})
export class UsersComponent { }
```

### Relative Navigation

```typescript
// Navigating relative to current route
@Component({
  selector: 'app-user-detail',
  template: `
    <div class="user-detail">
      <h2>{{ user?.name }}</h2>

      <div class="actions">
        <!-- Relative navigation -->
        <button (click)="editUser()">Edit</button>
        <button (click)="goToSibling()">Go to Sibling</button>
        <button (click)="goToParent()">Back to List</button>
        <button (click)="goToRoot()">Go to Dashboard</button>
      </div>
    </div>
  `
})
export class UserDetailComponent {
  user: User | null = null;

  constructor(
    private router: Router,
    private route: ActivatedRoute
  ) {}

  editUser(): void {
    // Navigate to sibling route: /dashboard/users/123/edit
    this.router.navigate(['edit'], { relativeTo: this.route });
  }

  goToSibling(): void {
    // Navigate to sibling route: /dashboard/users/456
    this.router.navigate(['..', '456'], { relativeTo: this.route });
  }

  goToParent(): void {
    // Navigate to parent route: /dashboard/users
    this.router.navigate(['..'], { relativeTo: this.route });
  }

  goToRoot(): void {
    // Navigate to root of current route tree: /dashboard
    this.router.navigate(['../..'], { relativeTo: this.route });
  }
}
```

## Route Guardian Spells

### CanActivate Guardian Enchantment

```typescript
// auth.guard.ts
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';
import { Observable } from 'rxjs';
import { AuthService } from './auth.service';

@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(
    private authService: AuthService,
    private router: Router
  ) {}

  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean> | Promise<boolean> | boolean {

    if (this.authService.isAuthenticated()) {
      return true;
    }

    // Store the attempted URL for redirecting after login
    this.authService.setRedirectUrl(state.url);

    // Navigate to login page
    this.router.navigate(['/login']);
    return false;
  }
}

// role.guard.ts
@Injectable({
  providedIn: 'root'
})
export class RoleGuard implements CanActivate {
  constructor(
    private authService: AuthService,
    private router: Router
  ) {}

  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): boolean {

    const requiredRoles = route.data['roles'] as string[];
    const userRoles = this.authService.getUserRoles();

    if (requiredRoles && requiredRoles.some(role => userRoles.includes(role))) {
      return true;
    }

    // Navigate to access denied page
    this.router.navigate(['/access-denied']);
    return false;
  }
}

// Using guards in routes
const routes: Routes = [
  {
    path: 'dashboard',
    component: DashboardComponent,
    canActivate: [AuthGuard],
    children: [
      {
        path: 'admin',
        component: AdminComponent,
        canActivate: [RoleGuard],
        data: { roles: ['admin', 'super-admin'] }
      }
    ]
  }
];
```

### CanDeactivate Guard

```typescript
// can-deactivate.guard.ts
import { Injectable } from '@angular/core';
import { CanDeactivate } from '@angular/router';
import { Observable } from 'rxjs';

export interface CanComponentDeactivate {
  canDeactivate(): Observable<boolean> | Promise<boolean> | boolean;
}

@Injectable({
  providedIn: 'root'
})
export class CanDeactivateGuard implements CanDeactivate<CanComponentDeactivate> {
  canDeactivate(
    component: CanComponentDeactivate
  ): Observable<boolean> | Promise<boolean> | boolean {
    return component.canDeactivate ? component.canDeactivate() : true;
  }
}

// user-edit.component.ts
@Component({
  selector: 'app-user-edit',
  template: `
    <form [formGroup]="userForm" (ngSubmit)="onSubmit()">
      <div class="form-group">
        <label for="name">Name:</label>
        <input id="name" formControlName="name" type="text">
      </div>

      <div class="form-group">
        <label for="email">Email:</label>
        <input id="email" formControlName="email" type="email">
      </div>

      <div class="form-actions">
        <button type="submit" [disabled]="!userForm.valid">Save</button>
        <button type="button" (click)="cancel()">Cancel</button>
      </div>
    </form>
  `
})
export class UserEditComponent implements CanComponentDeactivate {
  userForm: FormGroup;
  private originalFormValue: any;

  constructor(
    private fb: FormBuilder,
    private router: Router
  ) {
    this.userForm = this.fb.group({
      name: ['', Validators.required],
      email: ['', [Validators.required, Validators.email]]
    });

    // Store original form value
    this.originalFormValue = this.userForm.value;
  }

  canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
    // Check if form has unsaved changes
    if (this.hasUnsavedChanges()) {
      return confirm('You have unsaved changes. Do you really want to leave?');
    }
    return true;
  }

  private hasUnsavedChanges(): boolean {
    return JSON.stringify(this.userForm.value) !== JSON.stringify(this.originalFormValue);
  }

  onSubmit(): void {
    if (this.userForm.valid) {
      // Save user
      this.originalFormValue = this.userForm.value;
      this.router.navigate(['/users']);
    }
  }

  cancel(): void {
    this.router.navigate(['/users']);
  }
}

// Route configuration with CanDeactivate guard
const routes: Routes = [
  {
    path: 'user/:id/edit',
    component: UserEditComponent,
    canDeactivate: [CanDeactivateGuard]
  }
];

## Lazy Loading Sorcery

### Feature Module with Lazy Loading Magic

```typescript
// feature.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { FeatureRoutingModule } from './feature-routing.module';
import { FeatureComponent } from './feature.component';
import { FeatureListComponent } from './feature-list/feature-list.component';
import { FeatureDetailComponent } from './feature-detail/feature-detail.component';

@NgModule({
  declarations: [
    FeatureComponent,
    FeatureListComponent,
    FeatureDetailComponent
  ],
  imports: [
    CommonModule,
    FeatureRoutingModule
  ]
})
export class FeatureModule { }

// feature-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { FeatureComponent } from './feature.component';
import { FeatureListComponent } from './feature-list/feature-list.component';
import { FeatureDetailComponent } from './feature-detail/feature-detail.component';

const routes: Routes = [
  {
    path: '',
    component: FeatureComponent,
    children: [
      { path: '', redirectTo: 'list', pathMatch: 'full' },
      { path: 'list', component: FeatureListComponent },
      { path: ':id', component: FeatureDetailComponent }
    ]
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class FeatureRoutingModule { }

// app-routing.module.ts (main routing)
const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  {
    path: 'products',
    loadChildren: () => import('./products/products.module').then(m => m.ProductsModule)
  },
  {
    path: 'orders',
    loadChildren: () => import('./orders/orders.module').then(m => m.OrdersModule),
    canLoad: [AuthGuard] // Guard for lazy-loaded modules
  },
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule),
    canLoad: [AuthGuard, RoleGuard],
    data: { roles: ['admin'] }
  }
];
```

### Preloading Strategies

```typescript
// app-routing.module.ts
import { PreloadAllModules, RouterModule } from '@angular/router';

@NgModule({
  imports: [RouterModule.forRoot(routes, {
    // Preload all lazy-loaded modules
    preloadingStrategy: PreloadAllModules,

    // Enable router tracing for debugging
    enableTracing: false, // Set to true for debugging

    // Scroll behavior
    scrollPositionRestoration: 'top',

    // URL matching strategy
    urlUpdateStrategy: 'eager'
  })],
  exports: [RouterModule]
})
export class AppRoutingModule { }

// custom-preloading.strategy.ts
import { Injectable } from '@angular/core';
import { PreloadingStrategy, Route } from '@angular/router';
import { Observable, of } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class CustomPreloadingStrategy implements PreloadingStrategy {
  preload(route: Route, load: () => Observable<any>): Observable<any> {
    // Only preload routes marked with preload: true
    if (route.data && route.data['preload']) {
      console.log('Preloading module:', route.path);
      return load();
    }
    return of(null);
  }
}

// Usage with custom preloading
const routes: Routes = [
  {
    path: 'products',
    loadChildren: () => import('./products/products.module').then(m => m.ProductsModule),
    data: { preload: true } // This module will be preloaded
  },
  {
    path: 'reports',
    loadChildren: () => import('./reports/reports.module').then(m => m.ReportsModule),
    data: { preload: false } // This module will not be preloaded
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes, {
    preloadingStrategy: CustomPreloadingStrategy
  })],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

## Route Mystical Resolvers

### Data Resolver Enchantments

```typescript
// user-resolver.service.ts
import { Injectable } from '@angular/core';
import { Resolve, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Observable, of } from 'rxjs';
import { catchError } from 'rxjs/operators';
import { UserService } from './user.service';

@Injectable({
  providedIn: 'root'
})
export class UserResolver implements Resolve<User | null> {
  constructor(private userService: UserService) {}

  resolve(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<User | null> {
    const id = route.paramMap.get('id');

    if (id) {
      return this.userService.getUserById(+id).pipe(
        catchError(error => {
          console.error('Error loading user:', error);
          return of(null);
        })
      );
    }

    return of(null);
  }
}

// users-resolver.service.ts
@Injectable({
  providedIn: 'root'
})
export class UsersResolver implements Resolve<{ users: User[]; total: number }> {
  constructor(private userService: UserService) {}

  resolve(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<{ users: User[]; total: number }> {

    const page = +(route.queryParamMap.get('page') || 1);
    const search = route.queryParamMap.get('search') || '';
    const role = route.queryParamMap.get('role') || '';

    return this.userService.getUsers({ page, search, role }).pipe(
      catchError(error => {
        console.error('Error loading users:', error);
        return of({ users: [], total: 0 });
      })
    );
  }
}

// Route configuration with resolvers
const routes: Routes = [
  {
    path: 'users',
    component: UserListComponent,
    resolve: {
      usersData: UsersResolver
    }
  },
  {
    path: 'user/:id',
    component: UserDetailComponent,
    resolve: {
      user: UserResolver
    }
  }
];

// Using resolved data in components
@Component({
  selector: 'app-user-detail',
  template: `
    <div class="user-detail" *ngIf="user; else notFound">
      <h2>{{ user.name }}</h2>
      <p>{{ user.email }}</p>
      <p>{{ user.role }}</p>
    </div>

    <ng-template #notFound>
      <div class="not-found">
        <h2>User Not Found</h2>
        <p>The requested user could not be found.</p>
        <button (click)="goBack()">Go Back</button>
      </div>
    </ng-template>
  `
})
export class UserDetailComponent implements OnInit {
  user: User | null = null;

  constructor(
    private route: ActivatedRoute,
    private router: Router
  ) {}

  ngOnInit(): void {
    // Access resolved data
    this.user = this.route.snapshot.data['user'];

    // Or subscribe to data changes
    this.route.data.subscribe(data => {
      this.user = data['user'];
    });
  }

  goBack(): void {
    this.router.navigate(['/users']);
  }
}

@Component({
  selector: 'app-user-list',
  template: `
    <div class="user-list">
      <h2>Users ({{ total }} total)</h2>

      <div class="user-grid">
        <div *ngFor="let user of users" class="user-card">
          <h3>{{ user.name }}</h3>
          <p>{{ user.email }}</p>
          <button (click)="viewUser(user.id)">View Details</button>
        </div>
      </div>
    </div>
  `
})
export class UserListComponent implements OnInit {
  users: User[] = [];
  total = 0;

  constructor(
    private route: ActivatedRoute,
    private router: Router
  ) {}

  ngOnInit(): void {
    // Access resolved data
    const usersData = this.route.snapshot.data['usersData'];
    this.users = usersData.users;
    this.total = usersData.total;
  }

  viewUser(id: number): void {
    this.router.navigate(['/user', id]);
  }
}
```

## Advanced Router Mystical Features

### Router Event Divination

```typescript
// router-events.service.ts
import { Injectable } from '@angular/core';
import { Router, NavigationStart, NavigationEnd, NavigationCancel, NavigationError } from '@angular/router';
import { BehaviorSubject } from 'rxjs';
import { filter } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class RouterEventsService {
  private loadingSubject = new BehaviorSubject<boolean>(false);
  loading$ = this.loadingSubject.asObservable();

  constructor(private router: Router) {
    this.setupRouterEvents();
  }

  private setupRouterEvents(): void {
    this.router.events.subscribe(event => {
      switch (true) {
        case event instanceof NavigationStart:
          console.log('Navigation started to:', event.url);
          this.loadingSubject.next(true);
          break;

        case event instanceof NavigationEnd:
          console.log('Navigation ended:', event.url);
          this.loadingSubject.next(false);
          break;

        case event instanceof NavigationCancel:
          console.log('Navigation cancelled:', event.url);
          this.loadingSubject.next(false);
          break;

        case event instanceof NavigationError:
          console.error('Navigation error:', event.error);
          this.loadingSubject.next(false);
          break;
      }
    });
  }

  // Get specific router events
  getNavigationEndEvents() {
    return this.router.events.pipe(
      filter(event => event instanceof NavigationEnd)
    );
  }
}

// app.component.ts
@Component({
  selector: 'app-root',
  template: `
    <div class="app">
      <!-- Loading indicator -->
      <div *ngIf="routerEventsService.loading$ | async" class="loading-bar">
        <div class="loading-progress"></div>
      </div>

      <nav class="navbar">
        <!-- Navigation items -->
      </nav>

      <main class="content">
        <router-outlet></router-outlet>
      </main>
    </div>
  `,
  styles: [`
    .loading-bar {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 3px;
      background-color: #f0f0f0;
      z-index: 9999;
    }
    .loading-progress {
      height: 100%;
      background-color: #2196f3;
      animation: loading 2s ease-in-out infinite;
    }
    @keyframes loading {
      0% { width: 0%; }
      50% { width: 70%; }
      100% { width: 100%; }
    }
  `]
})
export class AppComponent {
  constructor(public routerEventsService: RouterEventsService) {}
}

### Custom Route Matcher

```typescript
// custom-route-matcher.ts
import { UrlMatcher, UrlSegment, Route } from '@angular/router';

// Custom matcher for article URLs like /article/2023/12/my-article-title
export function articleMatcher(segments: UrlSegment[], group: any, route: Route) {
  if (segments.length === 4 && segments[0].path === 'article') {
    const year = segments[1].path;
    const month = segments[2].path;
    const slug = segments[3].path;

    // Validate year and month format
    if (/^\d{4}$/.test(year) && /^\d{2}$/.test(month)) {
      return {
        consumed: segments,
        posParams: {
          year: segments[1],
          month: segments[2],
          slug: segments[3]
        }
      };
    }
  }
  return null;
}

// Route configuration with custom matcher
const routes: Routes = [
  {
    matcher: articleMatcher,
    component: ArticleComponent
  },
  // Fallback route
  { path: '**', component: PageNotFoundComponent }
];

// article.component.ts
@Component({
  selector: 'app-article',
  template: `
    <article class="article" *ngIf="article">
      <header>
        <h1>{{ article.title }}</h1>
        <div class="meta">
          <span>Published: {{ article.publishedDate | date }}</span>
          <span>Category: {{ article.category }}</span>
        </div>
      </header>
      <div class="content" [innerHTML]="article.content"></div>
    </article>
  `
})
export class ArticleComponent implements OnInit {
  article: Article | null = null;

  constructor(
    private route: ActivatedRoute,
    private articleService: ArticleService
  ) {}

  ngOnInit(): void {
    const year = this.route.snapshot.paramMap.get('year');
    const month = this.route.snapshot.paramMap.get('month');
    const slug = this.route.snapshot.paramMap.get('slug');

    if (year && month && slug) {
      this.articleService.getArticleBySlug(year, month, slug)
        .subscribe(article => {
          this.article = article;
        });
    }
  }
}
```

## Testing Router Magic

### Testing Components with Router Spells

```typescript
// user-detail.component.spec.ts
import { ComponentFixture, TestBed } from '@angular/core/testing';
import { Router } from '@angular/router';
import { ActivatedRoute } from '@angular/router';
import { of } from 'rxjs';
import { UserDetailComponent } from './user-detail.component';
import { UserService } from '../services/user.service';

describe('UserDetailComponent', () => {
  let component: UserDetailComponent;
  let fixture: ComponentFixture<UserDetailComponent>;
  let mockRouter: jasmine.SpyObj<Router>;
  let mockActivatedRoute: any;
  let mockUserService: jasmine.SpyObj<UserService>;

  const mockUser = {
    id: 1,
    name: 'John Doe',
    email: 'john@example.com',
    role: 'admin'
  };

  beforeEach(async () => {
    mockRouter = jasmine.createSpyObj('Router', ['navigate']);
    mockUserService = jasmine.createSpyObj('UserService', ['getUserById']);

    mockActivatedRoute = {
      snapshot: {
        paramMap: {
          get: jasmine.createSpy('get').and.returnValue('1')
        }
      },
      paramMap: of({
        get: (key: string) => key === 'id' ? '1' : null
      })
    };

    await TestBed.configureTestingModule({
      declarations: [UserDetailComponent],
      providers: [
        { provide: Router, useValue: mockRouter },
        { provide: ActivatedRoute, useValue: mockActivatedRoute },
        { provide: UserService, useValue: mockUserService }
      ]
    }).compileComponents();

    fixture = TestBed.createComponent(UserDetailComponent);
    component = fixture.componentInstance;
  });

  it('should create', () => {
    expect(component).toBeTruthy();
  });

  it('should load user on init', () => {
    mockUserService.getUserById.and.returnValue(of(mockUser));

    component.ngOnInit();

    expect(mockUserService.getUserById).toHaveBeenCalledWith(1);
    expect(component.user).toEqual(mockUser);
  });

  it('should navigate to edit page', () => {
    component.user = mockUser;

    component.editUser();

    expect(mockRouter.navigate).toHaveBeenCalledWith(['/user', 1, 'edit']);
  });

  it('should navigate back to users list', () => {
    component.goBack();

    expect(mockRouter.navigate).toHaveBeenCalledWith(['/users']);
  });
});

// Testing route guards
describe('AuthGuard', () => {
  let guard: AuthGuard;
  let mockAuthService: jasmine.SpyObj<AuthService>;
  let mockRouter: jasmine.SpyObj<Router>;

  beforeEach(() => {
    mockAuthService = jasmine.createSpyObj('AuthService', ['isAuthenticated', 'setRedirectUrl']);
    mockRouter = jasmine.createSpyObj('Router', ['navigate']);

    TestBed.configureTestingModule({
      providers: [
        AuthGuard,
        { provide: AuthService, useValue: mockAuthService },
        { provide: Router, useValue: mockRouter }
      ]
    });

    guard = TestBed.inject(AuthGuard);
  });

  it('should allow access when authenticated', () => {
    mockAuthService.isAuthenticated.and.returnValue(true);

    const result = guard.canActivate({} as any, { url: '/dashboard' } as any);

    expect(result).toBe(true);
    expect(mockRouter.navigate).not.toHaveBeenCalled();
  });

  it('should redirect to login when not authenticated', () => {
    mockAuthService.isAuthenticated.and.returnValue(false);

    const result = guard.canActivate({} as any, { url: '/dashboard' } as any);

    expect(result).toBe(false);
    expect(mockAuthService.setRedirectUrl).toHaveBeenCalledWith('/dashboard');
    expect(mockRouter.navigate).toHaveBeenCalledWith(['/login']);
  });
});
```

### Integration Testing with RouterTestingModule

```typescript
// app.component.spec.ts
import { TestBed } from '@angular/core/testing';
import { RouterTestingModule } from '@angular/router/testing';
import { Router } from '@angular/router';
import { Location } from '@angular/common';
import { Component } from '@angular/core';
import { AppComponent } from './app.component';

@Component({ template: 'Home Component' })
class MockHomeComponent { }

@Component({ template: 'About Component' })
class MockAboutComponent { }

describe('AppComponent Routing', () => {
  let router: Router;
  let location: Location;
  let fixture: any;

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [AppComponent, MockHomeComponent, MockAboutComponent],
      imports: [
        RouterTestingModule.withRoutes([
          { path: '', redirectTo: '/home', pathMatch: 'full' },
          { path: 'home', component: MockHomeComponent },
          { path: 'about', component: MockAboutComponent }
        ])
      ]
    }).compileComponents();

    fixture = TestBed.createComponent(AppComponent);
    router = TestBed.inject(Router);
    location = TestBed.inject(Location);
    fixture.detectChanges();
  });

  it('should navigate to home by default', async () => {
    await router.navigate(['']);
    expect(location.path()).toBe('/home');
  });

  it('should navigate to about page', async () => {
    await router.navigate(['/about']);
    expect(location.path()).toBe('/about');
  });

  it('should render correct component for route', async () => {
    await router.navigate(['/home']);
    fixture.detectChanges();

    const compiled = fixture.nativeElement;
    expect(compiled.textContent).toContain('Home Component');
  });
});
```

## Wisdom of the Router Ancients

### Route Organization Mystical Principles

```typescript
// ✅ Good: Organize routes hierarchically
const routes: Routes = [
  // Public routes
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'contact', component: ContactComponent },

  // Authentication routes
  { path: 'login', component: LoginComponent },
  { path: 'register', component: RegisterComponent },
  { path: 'forgot-password', component: ForgotPasswordComponent },

  // Protected routes
  {
    path: 'dashboard',
    component: DashboardComponent,
    canActivate: [AuthGuard],
    children: [
      { path: '', redirectTo: 'overview', pathMatch: 'full' },
      { path: 'overview', component: OverviewComponent },
      { path: 'profile', component: ProfileComponent }
    ]
  },

  // Feature modules (lazy loaded)
  {
    path: 'products',
    loadChildren: () => import('./products/products.module').then(m => m.ProductsModule),
    canLoad: [AuthGuard]
  },

  // Error routes
  { path: '404', component: PageNotFoundComponent },
  { path: '403', component: AccessDeniedComponent },

  // Wildcard route - must be last
  { path: '**', redirectTo: '/404' }
];

// ✅ Good: Use feature routing modules
// products-routing.module.ts
const routes: Routes = [
  {
    path: '',
    component: ProductsComponent,
    children: [
      { path: '', component: ProductListComponent },
      { path: 'new', component: ProductCreateComponent, canActivate: [RoleGuard] },
      { path: ':id', component: ProductDetailComponent, resolve: { product: ProductResolver } },
      { path: ':id/edit', component: ProductEditComponent, canActivate: [RoleGuard], canDeactivate: [CanDeactivateGuard] }
    ]
  }
];
```

### Performance Optimization

```typescript
// ✅ Use lazy loading for feature modules
const routes: Routes = [
  {
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule)
  }
];

// ✅ Implement custom preloading strategy
@Injectable()
export class SelectivePreloadingStrategy implements PreloadingStrategy {
  preload(route: Route, load: () => Observable<any>): Observable<any> {
    return route.data && route.data['preload'] ? load() : of(null);
  }
}

// ✅ Use OnPush change detection with router
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent {
  user$ = this.route.data.pipe(map(data => data['user']));

  constructor(private route: ActivatedRoute) {}
}
```

### Security Best Practices

```typescript
// ✅ Implement proper route guards
@Injectable()
export class AuthGuard implements CanActivate, CanLoad {
  canActivate(): boolean {
    return this.checkAuth();
  }

  canLoad(): boolean {
    return this.checkAuth();
  }

  private checkAuth(): boolean {
    if (this.authService.isAuthenticated()) {
      return true;
    }
    this.router.navigate(['/login']);
    return false;
  }
}

// ✅ Validate route parameters
@Component({})
export class UserDetailComponent implements OnInit {
  ngOnInit(): void {
    const id = this.route.snapshot.paramMap.get('id');

    // Validate parameter
    if (!id || isNaN(+id)) {
      this.router.navigate(['/404']);
      return;
    }

    this.loadUser(+id);
  }
}
```

## Common Curses & Their Remedies

### Curse 1: Memory Drain from Route Mystical Subscriptions
Not unsubscribing from route parameter observables causes mystical memory leaks.

**Counter-Spell:**
```typescript
// ❌ Bad: No unsubscription
export class BadComponent implements OnInit {
  ngOnInit(): void {
    this.route.params.subscribe(params => {
      // This subscription will never be cleaned up
    });
  }
}

// ✅ Good: Proper cleanup
export class GoodComponent implements OnInit, OnDestroy {
  private destroy$ = new Subject<void>();

  ngOnInit(): void {
    this.route.params.pipe(
      takeUntil(this.destroy$)
    ).subscribe(params => {
      // Subscription will be cleaned up
    });
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

### Issue 2: Incorrect Route Order
Placing wildcard routes before specific routes.

**Solution:**
```typescript
// ❌ Bad: Wildcard route too early
const routes: Routes = [
  { path: '**', component: PageNotFoundComponent }, // This catches everything!
  { path: 'users', component: UsersComponent }, // Never reached
];

// ✅ Good: Wildcard route last
const routes: Routes = [
  { path: 'users', component: UsersComponent },
  { path: '**', component: PageNotFoundComponent } // Catches unmatched routes
];
```

### Issue 3: Not Handling Route Errors
Failing to handle navigation errors and resolver failures.

**Solution:**
```typescript
// ✅ Good: Handle navigation errors
async navigateToUser(id: number): Promise<void> {
  try {
    await this.router.navigate(['/user', id]);
  } catch (error) {
    console.error('Navigation failed:', error);
    this.notificationService.error('Failed to navigate to user');
  }
}

// ✅ Good: Handle resolver errors
@Injectable()
export class UserResolver implements Resolve<User | null> {
  resolve(route: ActivatedRouteSnapshot): Observable<User | null> {
    const id = route.paramMap.get('id');

    return this.userService.getUserById(+id!).pipe(
      catchError(error => {
        console.error('Failed to load user:', error);
        this.router.navigate(['/users']);
        return of(null);
      })
    );
  }
}
```

## Sacred Texts & Mystical Sources

{% embed url="https://angular.io/guide/router" %}

{% embed url="https://angular.io/guide/router-tutorial" %}

{% embed url="https://angular.io/api/router" %}

{% embed url="https://angular.io/guide/lazy-loading-ngmodules" %}
```
```
```
