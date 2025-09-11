---
description: >-
  Complete grimoire for Redux Toolkit - master the ancient art of predictable state
  management in React enchantments, featuring modern Redux patterns, RTK Query,
  and powerful debugging spells for complex application state orchestration.

theme: "magic"
---

# Redux Toolkit - The State Orchestration Grimoire

## The Ancient Knowledge

Redux Toolkit (RTK) is the official, opinionated, batteries-included toolset for efficient Redux development. This mystical library simplifies Redux usage by providing powerful utilities that eliminate boilerplate, enforce best practices, and include essential tools like RTK Query for data fetching. It represents the modern evolution of Redux, making state management more accessible while maintaining the predictability and debugging capabilities that made Redux legendary.

## When to Cast This Spell

1. **Complex Application State**: Managing intricate state relationships across large applications
2. **Predictable State Updates**: Need for time-travel debugging and predictable state mutations
3. **Team Collaboration**: Large teams requiring consistent state management patterns
4. **Data Fetching & Caching**: Complex API interactions with sophisticated caching needs
5. **DevTools Integration**: Advanced debugging and state inspection requirements

## Your First Casting

Here's a basic Redux Toolkit setup with a simple counter example:

```jsx
// store/index.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';
import userReducer from './userSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
    user: userReducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: {
        ignoredActions: ['persist/PERSIST'],
      },
    }),
  devTools: process.env.NODE_ENV !== 'production',
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

```jsx
// store/counterSlice.js
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface CounterState {
  value: number;
  step: number;
}

const initialState: CounterState = {
  value: 0,
  step: 1,
};

export const counterSlice = createSlice({
  name: 'counter',
  initialState,
  reducers: {
    increment: (state) => {
      // RTK uses Immer internally, so we can "mutate" the state
      state.value += state.step;
    },
    decrement: (state) => {
      state.value -= state.step;
    },
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.value += action.payload;
    },
    setStep: (state, action: PayloadAction<number>) => {
      state.step = action.payload;
    },
    reset: (state) => {
      state.value = 0;
      state.step = 1;
    },
  },
});

export const { increment, decrement, incrementByAmount, setStep, reset } = counterSlice.actions;
export default counterSlice.reducer;
```

```jsx
// hooks/redux.ts
import { useDispatch, useSelector, TypedUseSelectorHook } from 'react-redux';
import type { RootState, AppDispatch } from '../store';

// Typed hooks for better TypeScript support
export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

```jsx
// components/Counter.jsx
import React from 'react';
import { useAppSelector, useAppDispatch } from '../hooks/redux';
import { increment, decrement, incrementByAmount, setStep, reset } from '../store/counterSlice';

function Counter() {
  const { value, step } = useAppSelector((state) => state.counter);
  const dispatch = useAppDispatch();

  const [customAmount, setCustomAmount] = React.useState(5);

  return (
    <div className="counter-enchantment">
      <div className="counter-display">
        <h2>Mystical Counter: {value}</h2>
        <p>Current Step: {step}</p>
      </div>

      <div className="counter-controls">
        <button onClick={() => dispatch(increment())}>
          Increment (+{step})
        </button>
        
        <button onClick={() => dispatch(decrement())}>
          Decrement (-{step})
        </button>
        
        <div className="custom-controls">
          <input
            type="number"
            value={customAmount}
            onChange={(e) => setCustomAmount(Number(e.target.value))}
          />
          <button onClick={() => dispatch(incrementByAmount(customAmount))}>
            Add {customAmount}
          </button>
        </div>

        <div className="step-controls">
          <label>
            Step Size:
            <input
              type="number"
              value={step}
              onChange={(e) => dispatch(setStep(Number(e.target.value)))}
              min="1"
            />
          </label>
        </div>

        <button onClick={() => dispatch(reset())} className="reset-button">
          Reset Counter
        </button>
      </div>
    </div>
  );
}

export default Counter;
```

```jsx
// App.jsx
import React from 'react';
import { Provider } from 'react-redux';
import { store } from './store';
import Counter from './components/Counter';

function App() {
  return (
    <Provider store={store}>
      <div className="app">
        <h1>Redux Toolkit Enchantment</h1>
        <Counter />
      </div>
    </Provider>
  );
}

export default App;
```

## Advanced Sorcery

### Async Actions with createAsyncThunk

```jsx
// store/userSlice.js
import { createSlice, createAsyncThunk, PayloadAction } from '@reduxjs/toolkit';

interface User {
  id: string;
  name: string;
  email: string;
  avatar?: string;
}

interface UserState {
  currentUser: User | null;
  users: User[];
  loading: boolean;
  error: string | null;
}

const initialState: UserState = {
  currentUser: null,
  users: [],
  loading: false,
  error: null,
};

// Async thunk for fetching user data
export const fetchUser = createAsyncThunk(
  'user/fetchUser',
  async (userId: string, { rejectWithValue }) => {
    try {
      const response = await fetch(`/api/users/${userId}`);
      if (!response.ok) {
        throw new Error('Failed to fetch user');
      }
      const user = await response.json();
      return user;
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

// Async thunk for updating user profile
export const updateUserProfile = createAsyncThunk(
  'user/updateProfile',
  async (userData: Partial<User>, { getState, rejectWithValue }) => {
    try {
      const state = getState() as { user: UserState };
      const currentUser = state.user.currentUser;
      
      if (!currentUser) {
        throw new Error('No user logged in');
      }

      const response = await fetch(`/api/users/${currentUser.id}`, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(userData),
      });

      if (!response.ok) {
        throw new Error('Failed to update profile');
      }

      const updatedUser = await response.json();
      return updatedUser;
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

export const userSlice = createSlice({
  name: 'user',
  initialState,
  reducers: {
    clearError: (state) => {
      state.error = null;
    },
    logout: (state) => {
      state.currentUser = null;
      state.users = [];
      state.error = null;
    },
    setCurrentUser: (state, action: PayloadAction<User>) => {
      state.currentUser = action.payload;
    },
  },
  extraReducers: (builder) => {
    builder
      // Fetch user cases
      .addCase(fetchUser.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUser.fulfilled, (state, action) => {
        state.loading = false;
        state.currentUser = action.payload;
        
        // Add to users array if not already present
        const existingUser = state.users.find(user => user.id === action.payload.id);
        if (!existingUser) {
          state.users.push(action.payload);
        }
      })
      .addCase(fetchUser.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload as string;
      })
      // Update profile cases
      .addCase(updateUserProfile.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(updateUserProfile.fulfilled, (state, action) => {
        state.loading = false;
        state.currentUser = action.payload;
        
        // Update in users array
        const userIndex = state.users.findIndex(user => user.id === action.payload.id);
        if (userIndex !== -1) {
          state.users[userIndex] = action.payload;
        }
      })
      .addCase(updateUserProfile.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload as string;
      });
  },
});

export const { clearError, logout, setCurrentUser } = userSlice.actions;
export default userSlice.reducer;
```

### RTK Query for Data Fetching

```jsx
// api/apiSlice.js
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react';

// Define the API slice
export const apiSlice = createApi({
  reducerPath: 'api',
  baseQuery: fetchBaseQuery({
    baseUrl: '/api',
    prepareHeaders: (headers, { getState }) => {
      // Add auth token if available
      const token = (getState() as RootState).auth.token;
      if (token) {
        headers.set('authorization', `Bearer ${token}`);
      }
      return headers;
    },
  }),
  tagTypes: ['Post', 'User', 'Comment'],
  endpoints: (builder) => ({
    // Posts endpoints
    getPosts: builder.query<Post[], { page?: number; limit?: number }>({
      query: ({ page = 1, limit = 10 } = {}) => `posts?page=${page}&limit=${limit}`,
      providesTags: ['Post'],
    }),
    getPost: builder.query<Post, string>({
      query: (id) => `posts/${id}`,
      providesTags: (result, error, id) => [{ type: 'Post', id }],
    }),
    createPost: builder.mutation<Post, Partial<Post>>({
      query: (newPost) => ({
        url: 'posts',
        method: 'POST',
        body: newPost,
      }),
      invalidatesTags: ['Post'],
    }),
    updatePost: builder.mutation<Post, { id: string; updates: Partial<Post> }>({
      query: ({ id, updates }) => ({
        url: `posts/${id}`,
        method: 'PATCH',
        body: updates,
      }),
      invalidatesTags: (result, error, { id }) => [{ type: 'Post', id }],
    }),
    deletePost: builder.mutation<void, string>({
      query: (id) => ({
        url: `posts/${id}`,
        method: 'DELETE',
      }),
      invalidatesTags: ['Post'],
    }),
    // Users endpoints
    getUsers: builder.query<User[], void>({
      query: () => 'users',
      providesTags: ['User'],
    }),
    getUser: builder.query<User, string>({
      query: (id) => `users/${id}`,
      providesTags: (result, error, id) => [{ type: 'User', id }],
    }),
  }),
});

// Export hooks for usage in components
export const {
  useGetPostsQuery,
  useGetPostQuery,
  useCreatePostMutation,
  useUpdatePostMutation,
  useDeletePostMutation,
  useGetUsersQuery,
  useGetUserQuery,
} = apiSlice;
```

```jsx
// components/PostsList.jsx
import React, { useState } from 'react';
import { useGetPostsQuery, useDeletePostMutation } from '../api/apiSlice';

function PostsList() {
  const [page, setPage] = useState(1);
  const {
    data: posts,
    error,
    isLoading,
    isFetching,
    refetch,
  } = useGetPostsQuery({ page, limit: 5 });

  const [deletePost, { isLoading: isDeleting }] = useDeletePostMutation();

  const handleDelete = async (postId: string) => {
    if (window.confirm('Are you sure you want to delete this post?')) {
      try {
        await deletePost(postId).unwrap();
        // Post will be automatically removed from cache due to invalidatesTags
      } catch (error) {
        console.error('Failed to delete post:', error);
      }
    }
  };

  if (isLoading) {
    return <div className="loading">📚 Loading mystical posts...</div>;
  }

  if (error) {
    return (
      <div className="error">
        ❌ Error loading posts: {error.message}
        <button onClick={refetch}>Try Again</button>
      </div>
    );
  }

  return (
    <div className="posts-list">
      <div className="posts-header">
        <h2>Mystical Posts</h2>
        <button onClick={refetch} disabled={isFetching}>
          {isFetching ? '🔄 Refreshing...' : '🔄 Refresh'}
        </button>
      </div>

      <div className="posts-container">
        {posts?.map((post) => (
          <div key={post.id} className="post-card">
            <h3>{post.title}</h3>
            <p>{post.excerpt}</p>
            <div className="post-meta">
              <span>By {post.author}</span>
              <span>{new Date(post.createdAt).toLocaleDateString()}</span>
            </div>
            <div className="post-actions">
              <button className="edit-button">Edit</button>
              <button
                className="delete-button"
                onClick={() => handleDelete(post.id)}
                disabled={isDeleting}
              >
                {isDeleting ? 'Deleting...' : 'Delete'}
              </button>
            </div>
          </div>
        ))}
      </div>

      <div className="pagination">
        <button
          onClick={() => setPage(page - 1)}
          disabled={page === 1}
        >
          Previous
        </button>
        <span>Page {page}</span>
        <button
          onClick={() => setPage(page + 1)}
          disabled={!posts || posts.length < 5}
        >
          Next
        </button>
      </div>
    </div>
  );
}

export default PostsList;
```

### Advanced Store Configuration

```jsx
// store/index.js
import { configureStore, combineReducers } from '@reduxjs/toolkit';
import { persistStore, persistReducer } from 'redux-persist';
import storage from 'redux-persist/lib/storage';
import { apiSlice } from '../api/apiSlice';
import counterReducer from './counterSlice';
import userReducer from './userSlice';
import authReducer from './authSlice';

// Persist configuration
const persistConfig = {
  key: 'root',
  storage,
  whitelist: ['auth', 'user'], // Only persist auth and user state
  blacklist: ['api'], // Don't persist API cache
};

// Root reducer
const rootReducer = combineReducers({
  counter: counterReducer,
  user: userReducer,
  auth: authReducer,
  [apiSlice.reducerPath]: apiSlice.reducer,
});

// Persisted reducer
const persistedReducer = persistReducer(persistConfig, rootReducer);

// Configure store with middleware
export const store = configureStore({
  reducer: persistedReducer,
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware({
      serializableCheck: {
        ignoredActions: [
          'persist/PERSIST',
          'persist/REHYDRATE',
          'persist/PAUSE',
          'persist/PURGE',
          'persist/REGISTER',
        ],
      },
    }).concat(apiSlice.middleware),
  devTools: {
    name: 'Mystical App Store',
    trace: true,
    traceLimit: 25,
  },
});

export const persistor = persistStore(store);

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### Entity Adapter for Normalized State

```jsx
// store/postsSlice.js
import { createSlice, createEntityAdapter, createAsyncThunk } from '@reduxjs/toolkit';

interface Post {
  id: string;
  title: string;
  content: string;
  author: string;
  tags: string[];
  createdAt: string;
  updatedAt: string;
}

// Create entity adapter
const postsAdapter = createEntityAdapter<Post>({
  // Sort posts by creation date (newest first)
  sortComparer: (a, b) => b.createdAt.localeCompare(a.createdAt),
});

// Get initial state from adapter
const initialState = postsAdapter.getInitialState({
  loading: false,
  error: null,
  selectedPostId: null,
});

// Async thunks
export const fetchPosts = createAsyncThunk('posts/fetchPosts', async () => {
  const response = await fetch('/api/posts');
  return response.json();
});

export const addPost = createAsyncThunk('posts/addPost', async (postData: Omit<Post, 'id'>) => {
  const response = await fetch('/api/posts', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(postData),
  });
  return response.json();
});

const postsSlice = createSlice({
  name: 'posts',
  initialState,
  reducers: {
    selectPost: (state, action) => {
      state.selectedPostId = action.payload;
    },
    clearSelection: (state) => {
      state.selectedPostId = null;
    },
    // Use adapter methods for CRUD operations
    postUpdated: postsAdapter.updateOne,
    postRemoved: postsAdapter.removeOne,
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchPosts.pending, (state) => {
        state.loading = true;
      })
      .addCase(fetchPosts.fulfilled, (state, action) => {
        state.loading = false;
        // Use adapter to set all posts
        postsAdapter.setAll(state, action.payload);
      })
      .addCase(fetchPosts.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      })
      .addCase(addPost.fulfilled, (state, action) => {
        // Use adapter to add new post
        postsAdapter.addOne(state, action.payload);
      });
  },
});

export const { selectPost, clearSelection, postUpdated, postRemoved } = postsSlice.actions;

// Export selectors generated by the adapter
export const {
  selectAll: selectAllPosts,
  selectById: selectPostById,
  selectIds: selectPostIds,
} = postsAdapter.getSelectors((state: RootState) => state.posts);

// Custom selectors
export const selectSelectedPost = (state: RootState) => {
  const selectedId = state.posts.selectedPostId;
  return selectedId ? selectPostById(state, selectedId) : null;
};

export const selectPostsByTag = (state: RootState, tag: string) => {
  return selectAllPosts(state).filter(post => post.tags.includes(tag));
};

export default postsSlice.reducer;
```

### Custom Middleware

```jsx
// middleware/logger.js
import { createListenerMiddleware } from '@reduxjs/toolkit';

// Create listener middleware for side effects
export const listenerMiddleware = createListenerMiddleware();

// Add listeners for specific actions
listenerMiddleware.startListening({
  actionCreator: userSlice.actions.setCurrentUser,
  effect: async (action, listenerApi) => {
    console.log('User logged in:', action.payload);

    // Track user login analytics
    analytics.track('user_login', {
      userId: action.payload.id,
      timestamp: new Date().toISOString(),
    });

    // Fetch user preferences
    listenerApi.dispatch(fetchUserPreferences(action.payload.id));
  },
});

// Custom logging middleware
export const loggerMiddleware = (store) => (next) => (action) => {
  const prevState = store.getState();
  const result = next(action);
  const nextState = store.getState();

  console.group(`Action: ${action.type}`);
  console.log('Previous State:', prevState);
  console.log('Action:', action);
  console.log('Next State:', nextState);
  console.groupEnd();

  return result;
};

// Error handling middleware
export const errorMiddleware = (store) => (next) => (action) => {
  try {
    return next(action);
  } catch (error) {
    console.error('Redux Error:', error);

    // Dispatch error action
    store.dispatch({
      type: 'app/errorOccurred',
      payload: {
        error: error.message,
        action: action.type,
        timestamp: new Date().toISOString(),
      },
    });

    throw error;
  }
};
```

## Performance Optimization Patterns

### Memoized Selectors with Reselect

```jsx
// selectors/index.js
import { createSelector } from '@reduxjs/toolkit';

// Basic selectors
const selectPosts = (state) => state.posts.entities;
const selectUsers = (state) => state.users.entities;
const selectCurrentUserId = (state) => state.auth.currentUserId;

// Memoized selectors
export const selectPostsWithAuthors = createSelector(
  [selectPosts, selectUsers],
  (posts, users) => {
    return Object.values(posts).map(post => ({
      ...post,
      author: users[post.authorId] || { name: 'Unknown' },
    }));
  }
);

export const selectCurrentUserPosts = createSelector(
  [selectPostsWithAuthors, selectCurrentUserId],
  (postsWithAuthors, currentUserId) => {
    return postsWithAuthors.filter(post => post.authorId === currentUserId);
  }
);

export const selectPostsByCategory = createSelector(
  [selectPostsWithAuthors, (state, category) => category],
  (posts, category) => {
    return posts.filter(post => post.category === category);
  }
);

// Complex aggregation selector
export const selectPostStatistics = createSelector(
  [selectAllPosts],
  (posts) => {
    const stats = posts.reduce(
      (acc, post) => {
        acc.totalPosts++;
        acc.totalWords += post.content.split(' ').length;

        post.tags.forEach(tag => {
          acc.tagCounts[tag] = (acc.tagCounts[tag] || 0) + 1;
        });

        return acc;
      },
      { totalPosts: 0, totalWords: 0, tagCounts: {} }
    );

    return {
      ...stats,
      averageWordsPerPost: stats.totalPosts > 0 ? stats.totalWords / stats.totalPosts : 0,
      mostPopularTags: Object.entries(stats.tagCounts)
        .sort(([, a], [, b]) => b - a)
        .slice(0, 5),
    };
  }
);
```

## Wisdom of the Redux Ancients

- **Use RTK by Default**: Modern Redux development should always use Redux Toolkit
- **Normalize Complex State**: Use entity adapters for collections of items
- **Leverage RTK Query**: Excellent for data fetching with built-in caching
- **Memoize Selectors**: Use createSelector for expensive computations
- **Type Everything**: TypeScript integration provides excellent developer experience
- **DevTools Integration**: Essential for debugging complex state interactions

## Common Curses & Their Remedies

### Curse 1: Mutating State Directly (Outside RTK)
Directly mutating state in regular Redux reducers breaks immutability.

**Counter-Spell:**
```jsx
// ❌ Direct mutation in regular reducer
const counterReducer = (state = { value: 0 }, action) => {
  switch (action.type) {
    case 'increment':
      state.value++; // This breaks Redux!
      return state;
  }
};

// ✅ Use RTK createSlice with Immer
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: (state) => {
      state.value++; // Safe with RTK/Immer
    },
  },
});
```

### Curse 2: Not Using Typed Hooks
Using untyped Redux hooks loses TypeScript benefits.

**Banishment Ritual:**
```jsx
// ❌ Untyped hooks
const count = useSelector(state => state.counter.value);
const dispatch = useDispatch();

// ✅ Use typed hooks
const count = useAppSelector(state => state.counter.value);
const dispatch = useAppDispatch();
```

### Curse 3: Overusing Global State
Putting all state in Redux, even local component state.

**Protective Ward:**
```jsx
// ❌ Everything in Redux
const [inputValue, setInputValue] = useReduxState('form.inputValue');

// ✅ Use local state for local concerns
const [inputValue, setInputValue] = useState('');
const globalData = useAppSelector(state => state.data);
```

## Sacred Texts & Mystical Sources

{% embed url="https://redux-toolkit.js.org/" %}

{% embed url="https://rtk-query-docs.netlify.app/" %}

{% embed url="https://redux.js.org/style-guide/style-guide" %}
```
```
