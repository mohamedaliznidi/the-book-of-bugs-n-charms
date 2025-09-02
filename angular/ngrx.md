---
description: >-
  NgRx provides reactive state management for Angular applications using the Redux pattern. 
  Learn about stores, actions, reducers, effects, and selectors for scalable state management.
---

# NgRx State Management

## Introduction

NgRx is a framework for building reactive applications in Angular. It provides state management based on the Redux pattern, using RxJS to enable powerful side effect management, entity collection management, router bindings, and developer tooling.

## Core Concepts

- **Store**: Single source of truth for application state
- **Actions**: Describe unique events that happen in your application
- **Reducers**: Pure functions that handle state transitions
- **Effects**: Handle side effects like HTTP requests
- **Selectors**: Pure functions for selecting slices of state
- **Facades**: Service layer that encapsulates store interactions

## Installation and Setup

### Installing NgRx

```bash
# Install NgRx Store
ng add @ngrx/store

# Install additional packages
ng add @ngrx/effects
ng add @ngrx/store-devtools
ng add @ngrx/entity
ng add @ngrx/router-store
```

### Basic Store Setup

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { StoreModule } from '@ngrx/store';
import { EffectsModule } from '@ngrx/effects';
import { StoreDevtoolsModule } from '@ngrx/store-devtools';
import { environment } from '../environments/environment';

@NgModule({
  imports: [
    StoreModule.forRoot({}),
    EffectsModule.forRoot([]),
    StoreDevtoolsModule.instrument({
      maxAge: 25,
      logOnly: environment.production
    })
  ]
})
export class AppModule { }
```

## Actions

### Defining Actions

```typescript
// user.actions.ts
import { createAction, props } from '@ngrx/store';

export interface User {
  id: number;
  name: string;
  email: string;
  role: string;
}

// Load Users Actions
export const loadUsers = createAction('[User] Load Users');

export const loadUsersSuccess = createAction(
  '[User] Load Users Success',
  props<{ users: User[] }>()
);

export const loadUsersFailure = createAction(
  '[User] Load Users Failure',
  props<{ error: string }>()
);

// Create User Actions
export const createUser = createAction(
  '[User] Create User',
  props<{ user: Omit<User, 'id'> }>()
);

export const createUserSuccess = createAction(
  '[User] Create User Success',
  props<{ user: User }>()
);

export const createUserFailure = createAction(
  '[User] Create User Failure',
  props<{ error: string }>()
);

// Update User Actions
export const updateUser = createAction(
  '[User] Update User',
  props<{ id: number; changes: Partial<User> }>()
);

export const updateUserSuccess = createAction(
  '[User] Update User Success',
  props<{ user: User }>()
);

export const updateUserFailure = createAction(
  '[User] Update User Failure',
  props<{ error: string }>()
);

// Delete User Actions
export const deleteUser = createAction(
  '[User] Delete User',
  props<{ id: number }>()
);

export const deleteUserSuccess = createAction(
  '[User] Delete User Success',
  props<{ id: number }>()
);

export const deleteUserFailure = createAction(
  '[User] Delete User Failure',
  props<{ error: string }>()
);

// Select User Actions
export const selectUser = createAction(
  '[User] Select User',
  props<{ id: number }>()
);

export const clearSelectedUser = createAction('[User] Clear Selected User');

// Filter Actions
export const setUserFilter = createAction(
  '[User] Set Filter',
  props<{ filter: string }>()
);

export const clearUserFilter = createAction('[User] Clear Filter');
```

## State and Reducers

### Defining State Interface

```typescript
// user.state.ts
import { User } from './user.actions';

export interface UserState {
  users: User[];
  selectedUserId: number | null;
  filter: string;
  loading: boolean;
  error: string | null;
}

export const initialUserState: UserState = {
  users: [],
  selectedUserId: null,
  filter: '',
  loading: false,
  error: null
};
```

### Creating Reducers

```typescript
// user.reducer.ts
import { createReducer, on } from '@ngrx/store';
import { UserState, initialUserState } from './user.state';
import * as UserActions from './user.actions';

export const userReducer = createReducer(
  initialUserState,

  // Load Users
  on(UserActions.loadUsers, (state) => ({
    ...state,
    loading: true,
    error: null
  })),

  on(UserActions.loadUsersSuccess, (state, { users }) => ({
    ...state,
    users,
    loading: false,
    error: null
  })),

  on(UserActions.loadUsersFailure, (state, { error }) => ({
    ...state,
    loading: false,
    error
  })),

  // Create User
  on(UserActions.createUser, (state) => ({
    ...state,
    loading: true,
    error: null
  })),

  on(UserActions.createUserSuccess, (state, { user }) => ({
    ...state,
    users: [...state.users, user],
    loading: false,
    error: null
  })),

  on(UserActions.createUserFailure, (state, { error }) => ({
    ...state,
    loading: false,
    error
  })),

  // Update User
  on(UserActions.updateUser, (state) => ({
    ...state,
    loading: true,
    error: null
  })),

  on(UserActions.updateUserSuccess, (state, { user }) => ({
    ...state,
    users: state.users.map(u => u.id === user.id ? user : u),
    loading: false,
    error: null
  })),

  on(UserActions.updateUserFailure, (state, { error }) => ({
    ...state,
    loading: false,
    error
  })),

  // Delete User
  on(UserActions.deleteUser, (state) => ({
    ...state,
    loading: true,
    error: null
  })),

  on(UserActions.deleteUserSuccess, (state, { id }) => ({
    ...state,
    users: state.users.filter(user => user.id !== id),
    selectedUserId: state.selectedUserId === id ? null : state.selectedUserId,
    loading: false,
    error: null
  })),

  on(UserActions.deleteUserFailure, (state, { error }) => ({
    ...state,
    loading: false,
    error
  })),

  // Select User
  on(UserActions.selectUser, (state, { id }) => ({
    ...state,
    selectedUserId: id
  })),

  on(UserActions.clearSelectedUser, (state) => ({
    ...state,
    selectedUserId: null
  })),

  // Filter
  on(UserActions.setUserFilter, (state, { filter }) => ({
    ...state,
    filter
  })),

  on(UserActions.clearUserFilter, (state) => ({
    ...state,
    filter: ''
  }))
);
```

## Selectors

### Creating Selectors

```typescript
// user.selectors.ts
import { createFeatureSelector, createSelector } from '@ngrx/store';
import { UserState } from './user.state';

// Feature selector
export const selectUserState = createFeatureSelector<UserState>('users');

// Basic selectors
export const selectAllUsers = createSelector(
  selectUserState,
  (state) => state.users
);

export const selectUsersLoading = createSelector(
  selectUserState,
  (state) => state.loading
);

export const selectUsersError = createSelector(
  selectUserState,
  (state) => state.error
);

export const selectSelectedUserId = createSelector(
  selectUserState,
  (state) => state.selectedUserId
);

export const selectUserFilter = createSelector(
  selectUserState,
  (state) => state.filter
);

// Computed selectors
export const selectSelectedUser = createSelector(
  selectAllUsers,
  selectSelectedUserId,
  (users, selectedId) => users.find(user => user.id === selectedId) || null
);

export const selectFilteredUsers = createSelector(
  selectAllUsers,
  selectUserFilter,
  (users, filter) => {
    if (!filter) return users;
    
    return users.filter(user =>
      user.name.toLowerCase().includes(filter.toLowerCase()) ||
      user.email.toLowerCase().includes(filter.toLowerCase()) ||
      user.role.toLowerCase().includes(filter.toLowerCase())
    );
  }
);

export const selectUsersByRole = createSelector(
  selectAllUsers,
  (users) => {
    return users.reduce((acc, user) => {
      if (!acc[user.role]) {
        acc[user.role] = [];
      }
      acc[user.role].push(user);
      return acc;
    }, {} as { [role: string]: User[] });
  }
);

export const selectUsersCount = createSelector(
  selectAllUsers,
  (users) => users.length
);

export const selectFilteredUsersCount = createSelector(
  selectFilteredUsers,
  (users) => users.length
);

// Parameterized selector
export const selectUserById = (id: number) => createSelector(
  selectAllUsers,
  (users) => users.find(user => user.id === id)
);
```

## Effects

### Creating Effects

```typescript
// user.effects.ts
import { Injectable } from '@angular/core';
import { Actions, createEffect, ofType } from '@ngrx/effects';
import { of } from 'rxjs';
import { map, mergeMap, catchError, switchMap } from 'rxjs/operators';
import { UserService } from '../services/user.service';
import * as UserActions from './user.actions';

@Injectable()
export class UserEffects {
  constructor(
    private actions$: Actions,
    private userService: UserService
  ) {}

  loadUsers$ = createEffect(() =>
    this.actions$.pipe(
      ofType(UserActions.loadUsers),
      switchMap(() =>
        this.userService.getUsers().pipe(
          map(users => UserActions.loadUsersSuccess({ users })),
          catchError(error => of(UserActions.loadUsersFailure({ 
            error: error.message 
          })))
        )
      )
    )
  );

  createUser$ = createEffect(() =>
    this.actions$.pipe(
      ofType(UserActions.createUser),
      mergeMap(action =>
        this.userService.createUser(action.user).pipe(
          map(user => UserActions.createUserSuccess({ user })),
          catchError(error => of(UserActions.createUserFailure({ 
            error: error.message 
          })))
        )
      )
    )
  );

  updateUser$ = createEffect(() =>
    this.actions$.pipe(
      ofType(UserActions.updateUser),
      mergeMap(action =>
        this.userService.updateUser(action.id, action.changes).pipe(
          map(user => UserActions.updateUserSuccess({ user })),
          catchError(error => of(UserActions.updateUserFailure({ 
            error: error.message 
          })))
        )
      )
    )
  );

  deleteUser$ = createEffect(() =>
    this.actions$.pipe(
      ofType(UserActions.deleteUser),
      mergeMap(action =>
        this.userService.deleteUser(action.id).pipe(
          map(() => UserActions.deleteUserSuccess({ id: action.id })),
          catchError(error => of(UserActions.deleteUserFailure({ 
            error: error.message 
          })))
        )
      )
    )
  );
}
```
