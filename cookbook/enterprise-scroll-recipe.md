---
description: >-
  Build enterprise-scale applications with Angular and NX monorepo architecture. This recipe covers massive team coordination, shared libraries, microservices patterns, advanced state management with NgRx, comprehensive testing strategies, and production deployment workflows for large-scale applications.

theme: "magic"
---

# 🏢 Enterprise Scroll: Angular + NX Monorepo Recipe

*When your codebase needs to scale across teams, departments, and galaxies*

---

## 🎯 Cast This When You Need

**Massive Scale** • **Team Coordination** • **Type Safety Obsession** • **Enterprise Architecture** • **Shared Libraries**

Perfect for: Large teams, enterprise apps, microservices architecture, shared component libraries, strict governance

Terrible for: Solo projects, simple apps, rapid prototyping, when you want minimal overhead

---

## 🏗️ The Cathedral Summoning

```bash
# Create the enterprise kingdom
npx create-nx-workspace@latest my-enterprise --preset=angular-monorepo --style=scss --routing=true --packageManager=npm

cd my-enterprise

# Generate your first application
nx g @nx/angular:app customer-portal --routing --style=scss
nx g @nx/angular:app admin-dashboard --routing --style=scss

# Generate shared libraries
nx g @nx/angular:lib shared-ui --buildable --publishable --importPath=@my-enterprise/shared-ui
nx g @nx/angular:lib shared-data-access --buildable --publishable --importPath=@my-enterprise/shared-data-access
```

**Behold!** Your enterprise monorepo structure emerges:

```
my-enterprise/
├── apps/
│   ├── customer-portal/     # Main customer application
│   ├── admin-dashboard/     # Admin interface
│   └── customer-portal-e2e/ # E2E tests
├── libs/
│   ├── shared-ui/           # Shared components
│   └── shared-data-access/  # Services & models
├── tools/                   # Custom build tools
└── nx.json                  # NX configuration
```

---

## 🛡️ The Enterprise Architecture Fortress

### Core Foundation Setup

```typescript
// libs/shared-data-access/src/lib/models/user.model.ts
export interface User {
  id: string
  email: string
  firstName: string
  lastName: string
  roles: Role[]
  permissions: Permission[]
  organizationId: string
  lastLoginAt?: Date
  isActive: boolean
}

export interface Role {
  id: string
  name: string
  description: string
  permissions: Permission[]
}

export interface Permission {
  id: string
  resource: string
  action: 'create' | 'read' | 'update' | 'delete'
  conditions?: Record<string, any>
}
```

### Enterprise Service Pattern
```typescript
// libs/shared-data-access/src/lib/services/user.service.ts
import { Injectable } from '@angular/core'
import { HttpClient, HttpErrorResponse } from '@angular/common/http'
import { Observable, BehaviorSubject, throwError } from 'rxjs'
import { catchError, map, tap } from 'rxjs/operators'
import { User } from '../models/user.model'

@Injectable({
  providedIn: 'root'
})
export class UserService {
  private readonly apiUrl = '/api/users'
  private currentUserSubject = new BehaviorSubject<User | null>(null)
  
  public currentUser$ = this.currentUserSubject.asObservable()

  constructor(private http: HttpClient) {}

  getCurrentUser(): Observable<User> {
    return this.http.get<User>(`${this.apiUrl}/me`).pipe(
      tap(user => this.currentUserSubject.next(user)),
      catchError(this.handleError)
    )
  }

  getUsersByOrganization(orgId: string, page = 1, limit = 20): Observable<{users: User[], total: number}> {
    return this.http.get<{users: User[], total: number}>(`${this.apiUrl}`, {
      params: { organizationId: orgId, page: page.toString(), limit: limit.toString() }
    }).pipe(
      catchError(this.handleError)
    )
  }

  updateUser(id: string, updates: Partial<User>): Observable<User> {
    return this.http.patch<User>(`${this.apiUrl}/${id}`, updates).pipe(
      tap(updatedUser => {
        if (updatedUser.id === this.currentUserSubject.value?.id) {
          this.currentUserSubject.next(updatedUser)
        }
      }),
      catchError(this.handleError)
    )
  }

  private handleError(error: HttpErrorResponse): Observable<never> {
    let errorMessage = 'An unknown error occurred'
    
    if (error.error instanceof ErrorEvent) {
      errorMessage = `Client Error: ${error.error.message}`
    } else {
      errorMessage = `Server Error: ${error.status} - ${error.message}`
    }
    
    console.error('UserService Error:', errorMessage)
    return throwError(() => new Error(errorMessage))
  }
}
```

### Smart/Dumb Component Architecture
```typescript
// libs/shared-ui/src/lib/components/user-card/user-card.component.ts
import { Component, Input, Output, EventEmitter, ChangeDetectionStrategy } from '@angular/core'
import { User } from '@my-enterprise/shared-data-access'

@Component({
  selector: 'lib-user-card',
  template: `
    <div class="user-card" [class.inactive]="!user.isActive">
      <div class="user-avatar">
        <img [src]="user.avatarUrl || '/assets/default-avatar.png'" 
             [alt]="user.firstName + ' ' + user.lastName">
      </div>
      <div class="user-info">
        <h3>{{user.firstName}} {{user.lastName}}</h3>
        <p class="user-email">{{user.email}}</p>
        <div class="user-roles">
          <span *ngFor="let role of user.roles" class="role-badge">
            {{role.name}}
          </span>
        </div>
      </div>
      <div class="user-actions">
        <button (click)="onEdit()" class="btn btn-primary">Edit</button>
        <button (click)="onToggleStatus()" 
                [class]="user.isActive ? 'btn btn-danger' : 'btn btn-success'">
          {{user.isActive ? 'Deactivate' : 'Activate'}}
        </button>
      </div>
    </div>
  `,
  styleUrls: ['./user-card.component.scss'],
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class UserCardComponent {
  @Input() user!: User
  @Output() edit = new EventEmitter<User>()
  @Output() toggleStatus = new EventEmitter<User>()

  onEdit(): void {
    this.edit.emit(this.user)
  }

  onToggleStatus(): void {
    this.toggleStatus.emit(this.user)
  }
}
```

---

## 🔐 Enterprise Security & Auth

### JWT Auth Interceptor
```typescript
// libs/shared-data-access/src/lib/interceptors/auth.interceptor.ts
import { Injectable } from '@angular/core'
import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http'
import { AuthService } from '../services/auth.service'

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private authService: AuthService) {}

  intercept(req: HttpRequest<any>, next: HttpHandler) {
    const token = this.authService.getToken()
    
    if (token && !req.url.includes('/auth/')) {
      const authReq = req.clone({
        headers: req.headers.set('Authorization', `Bearer ${token}`)
      })
      return next.handle(authReq)
    }
    
    return next.handle(req)
  }
}
```

### Role-Based Route Guards
```typescript
// libs/shared-data-access/src/lib/guards/role.guard.ts
import { Injectable } from '@angular/core'
import { CanActivate, ActivatedRouteSnapshot, Router } from '@angular/router'
import { Observable, of } from 'rxjs'
import { map, catchError } from 'rxjs/operators'
import { UserService } from '../services/user.service'

@Injectable({
  providedIn: 'root'
})
export class RoleGuard implements CanActivate {
  constructor(
    private userService: UserService,
    private router: Router
  ) {}

  canActivate(route: ActivatedRouteSnapshot): Observable<boolean> {
    const requiredRoles = route.data['roles'] as string[]
    
    return this.userService.currentUser$.pipe(
      map(user => {
        if (!user) {
          this.router.navigate(['/auth/login'])
          return false
        }

        const hasRole = requiredRoles.some(role => 
          user.roles.some(userRole => userRole.name === role)
        )

        if (!hasRole) {
          this.router.navigate(['/unauthorized'])
          return false
        }

        return true
      }),
      catchError(() => {
        this.router.navigate(['/auth/login'])
        return of(false)
      })
    )
  }
}
```

---

## 📊 State Management at Scale

### NgRx Enterprise Pattern
```bash
# Install NgRx suite
npm install @ngrx/store @ngrx/effects @ngrx/entity @ngrx/store-devtools
```

```typescript
// libs/shared-data-access/src/lib/+state/user/user.actions.ts
import { createActionGroup, emptyProps, props } from '@ngrx/store'
import { User } from '../../models/user.model'

export const UserActions = createActionGroup({
  source: 'User',
  events: {
    'Load Users': props<{ organizationId: string; page?: number }>(),
    'Load Users Success': props<{ users: User[]; total: number }>(),
    'Load Users Failure': props<{ error: string }>(),
    
    'Select User': props<{ userId: string }>(),
    'Update User': props<{ id: string; updates: Partial<User> }>(),
    'Update User Success': props<{ user: User }>(),
    'Update User Failure': props<{ error: string }>(),
    
    'Clear Selection': emptyProps(),
  },
})
```

```typescript
// libs/shared-data-access/src/lib/+state/user/user.reducer.ts
import { createEntityAdapter, EntityAdapter, EntityState } from '@ngrx/entity'
import { createReducer, on } from '@ngrx/store'
import { User } from '../../models/user.model'
import { UserActions } from './user.actions'

export interface UserState extends EntityState<User> {
  selectedUserId: string | null
  loading: boolean
  error: string | null
  total: number
}

export const userAdapter: EntityAdapter<User> = createEntityAdapter<User>()

export const initialState: UserState = userAdapter.getInitialState({
  selectedUserId: null,
  loading: false,
  error: null,
  total: 0,
})

export const userReducer = createReducer(
  initialState,
  on(UserActions.loadUsers, (state) => ({
    ...state,
    loading: true,
    error: null,
  })),
  on(UserActions.loadUsersSuccess, (state, { users, total }) =>
    userAdapter.setAll(users, {
      ...state,
      loading: false,
      total,
    })
  ),
  on(UserActions.loadUsersFailure, (state, { error }) => ({
    ...state,
    loading: false,
    error,
  })),
  on(UserActions.selectUser, (state, { userId }) => ({
    ...state,
    selectedUserId: userId,
  })),
  on(UserActions.updateUserSuccess, (state, { user }) =>
    userAdapter.updateOne({ id: user.id, changes: user }, state)
  ),
)

export const {
  selectIds,
  selectEntities,
  selectAll,
  selectTotal,
} = userAdapter.getSelectors()
```

---

## 🧪 Testing Enterprise Applications

### Component Testing with Spectator
```bash
npm install -D @ngneat/spectator
```

```typescript
// libs/shared-ui/src/lib/components/user-card/user-card.component.spec.ts
import { createComponentFactory, Spectator } from '@ngneat/spectator'
import { UserCardComponent } from './user-card.component'
import { User } from '@my-enterprise/shared-data-access'

describe('UserCardComponent', () => {
  let spectator: Spectator<UserCardComponent>
  const createComponent = createComponentFactory({
    component: UserCardComponent,
  })

  const mockUser: User = {
    id: '1',
    email: 'john.doe@company.com',
    firstName: 'John',
    lastName: 'Doe',
    roles: [{ id: '1', name: 'Admin', description: 'Administrator', permissions: [] }],
    permissions: [],
    organizationId: 'org-1',
    isActive: true,
  }

  beforeEach(() => {
    spectator = createComponent({
      props: { user: mockUser }
    })
  })

  it('should display user information', () => {
    expect(spectator.query('h3')).toHaveText('John Doe')
    expect(spectator.query('.user-email')).toHaveText('john.doe@company.com')
    expect(spectator.query('.role-badge')).toHaveText('Admin')
  })

  it('should emit edit event when edit button clicked', () => {
    spyOn(spectator.component.edit, 'emit')
    spectator.click('button:contains("Edit")')
    expect(spectator.component.edit.emit).toHaveBeenCalledWith(mockUser)
  })

  it('should show deactivate button for active users', () => {
    expect(spectator.query('button:contains("Deactivate")')).toExist()
  })
})
```

### E2E Testing with Cypress
```typescript
// apps/customer-portal-e2e/src/e2e/user-management.cy.ts
describe('User Management', () => {
  beforeEach(() => {
    cy.login('admin@company.com', 'password123')
    cy.visit('/users')
  })

  it('should display user list', () => {
    cy.get('[data-cy=user-card]').should('have.length.greaterThan', 0)
    cy.get('[data-cy=user-name]').first().should('be.visible')
  })

  it('should allow editing user details', () => {
    cy.get('[data-cy=user-card]').first().find('[data-cy=edit-button]').click()
    cy.get('[data-cy=user-form]').should('be.visible')
    
    cy.get('[data-cy=first-name-input]').clear().type('UpdatedName')
    cy.get('[data-cy=save-button]').click()
    
    cy.get('[data-cy=success-toast]').should('contain', 'User updated successfully')
  })

  it('should filter users by role', () => {
    cy.get('[data-cy=role-filter]').select('Admin')
    cy.get('[data-cy=user-card]').each($card => {
      cy.wrap($card).find('[data-cy=role-badge]').should('contain', 'Admin')
    })
  })
})
```

---

## 🚀 Build & Deployment Mastery

### NX Build Optimization
```json
// nx.json
{
  "extends": "@nx/workspace/presets/npm.json",
  "tasksRunnerOptions": {
    "default": {
      "runner": "nx/tasks-runners/default",
      "options": {
        "cacheableOperations": ["build", "lint", "test", "e2e"],
        "parallel": 3
      }
    }
  },
  "targetDefaults": {
    "build": {
      "dependsOn": ["^build"],
      "cache": true
    }
  }
}
```

### Docker Multi-Stage Build
```dockerfile
# Dockerfile
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
COPY nx.json ./
COPY tsconfig*.json ./
COPY workspace.json ./

RUN npm ci --only=production

COPY . .
RUN npx nx build customer-portal --prod
RUN npx nx build admin-dashboard --prod

# Production stage
FROM nginx:alpine AS production

COPY --from=builder /app/dist/apps/customer-portal /usr/share/nginx/html/customer-portal
COPY --from=builder /app/dist/apps/admin-dashboard /usr/share/nginx/html/admin-dashboard

COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### CI/CD Pipeline with Affected Commands
```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  main:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
          
      - uses: actions/setup-node@v3
        with:
          node-version: 18
          cache: 'npm'
          
      - run: npm ci
      
      - run: npx nx format:check
      - run: npx nx affected --target=lint --parallel=3
      - run: npx nx affected --target=test --parallel=3 --coverage
      - run: npx nx affected --target=build --parallel=3
      - run: npx nx affected --target=e2e --parallel=1
```

---

## 🔧 Enterprise Tooling & DevOps

### Custom Workspace Generators
```typescript
// tools/workspace-plugin/src/generators/feature/generator.ts
import { Tree, formatFiles, installPackagesTask, generateFiles, joinPathFragments } from '@nx/devkit'

interface FeatureGeneratorSchema {
  name: string
  app: string
  directory?: string
}

export default async function (tree: Tree, options: FeatureGeneratorSchema) {
  const projectRoot = `apps/${options.app}/src/app/features/${options.name}`
  
  generateFiles(tree, joinPathFragments(__dirname, 'files'), projectRoot, {
    ...options,
    className: `${options.name.charAt(0).toUpperCase()}${options.name.slice(1)}`,
    template: '',
  })
  
  await formatFiles(tree)
  return () => {
    installPackagesTask(tree)
  }
}
```

### Code Quality Gates
```json
// .eslintrc.json
{
  "extends": ["@nx/eslint-plugin-nx/recommended"],
  "rules": {
    "@nx/enforce-module-boundaries": [
      "error",
      {
        "allow": [],
        "depConstraints": [
          {
            "sourceTag": "scope:shared",
            "onlyDependOnLibsWithTags": ["scope:shared"]
          },
          {
            "sourceTag": "type:app",
            "onlyDependOnLibsWithTags": ["type:feature", "type:ui", "type:data-access", "type:util"]
          },
          {
            "sourceTag": "type:feature",
            "onlyDependOnLibsWithTags": ["type:ui", "type:data-access", "type:util"]
          }
        ]
      }
    ]
  }
}
```

---

## 🐛 Enterprise-Scale Debugging

### Performance Monitoring
```typescript
// libs/shared-data-access/src/lib/services/performance.service.ts
import { Injectable } from '@angular/core'

@Injectable({
  providedIn: 'root'
})
export class PerformanceService {
  trackUserTiming(name: string, startTime: number): void {
    const duration = performance.now() - startTime
    
    if (duration > 1000) {
      console.warn(`Slow operation detected: ${name} took ${duration}ms`)
      
      // Send to monitoring service
      this.sendMetric('slow_operation', {
        operation: name,
        duration,
        timestamp: Date.now(),
      })
    }
  }

  private sendMetric(event: string, data: any): void {
    // Send to your monitoring service (DataDog, New Relic, etc.)
    fetch('/api/metrics', {
      method: 'POST',
      body: JSON.stringify({ event, data }),
    })
  }
}
```

### Memory Leak Detection
```typescript
// libs/shared-ui/src/lib/directives/memory-leak-detector.directive.ts
import { Directive, OnDestroy, OnInit } from '@angular/core'

@Directive({
  selector: '[memoryLeakDetector]'
})
export class MemoryLeakDetectorDirective implements OnInit, OnDestroy {
  private componentName = this.constructor.name
  private createdAt = performance.now()

  ngOnInit(): void {
    console.log(`Component ${this.componentName} initialized`)
  }

  ngOnDestroy(): void {
    const lifetime = performance.now() - this.createdAt
    console.log(`Component ${this.componentName} destroyed after ${lifetime}ms`)
    
    // Detect potential memory leaks
    if (lifetime > 300000) { // 5 minutes
      console.warn(`Potential memory leak: ${this.componentName} lived for ${lifetime}ms`)
    }
  }
}
```

