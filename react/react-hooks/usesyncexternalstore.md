---
description: >-
  Complete grimoire for the useSyncExternalStore enchantment - master the ancient art of
  synchronizing with external data sources in React enchantments, enabling seamless
  integration with third-party libraries, browser APIs, and custom stores.

theme: "magic"
---

# useSyncExternalStore - The External Synchronization Enchantment

## The Ancient Knowledge

The `useSyncExternalStore` enchantment is a React hook that allows you to subscribe to external data sources and keep your components synchronized with their changes. This mystical spell bridges the gap between React's internal state management and external systems like browser APIs, third-party libraries, or custom stores, ensuring consistent and tearing-free updates.

## When to Cast This Spell

1. **Browser API Integration**: Subscribe to window size, online status, or localStorage changes
2. **Third-Party Library Integration**: Connect to external state management libraries or data stores
3. **Custom Store Implementation**: Create your own reactive data stores with React integration
4. **Real-Time Data Sources**: Subscribe to WebSocket connections or server-sent events
5. **Global State Management**: Build lightweight global state solutions without heavy libraries

## Your First Casting

Here's a basic example of using `useSyncExternalStore` with browser APIs:

```jsx
import React, { useSyncExternalStore } from 'react';

// Custom hook for window size
function useWindowSize() {
  const subscribe = (callback) => {
    window.addEventListener('resize', callback);
    return () => window.removeEventListener('resize', callback);
  };

  const getSnapshot = () => ({
    width: window.innerWidth,
    height: window.innerHeight
  });

  const getServerSnapshot = () => ({
    width: 1200, // Default server-side values
    height: 800
  });

  return useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot);
}

// Custom hook for online status
function useOnlineStatus() {
  const subscribe = (callback) => {
    window.addEventListener('online', callback);
    window.addEventListener('offline', callback);
    return () => {
      window.removeEventListener('online', callback);
      window.removeEventListener('offline', callback);
    };
  };

  const getSnapshot = () => navigator.onLine;
  const getServerSnapshot = () => true; // Assume online on server

  return useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot);
}

// Usage component
function ResponsiveApp() {
  const windowSize = useWindowSize();
  const isOnline = useOnlineStatus();

  return (
    <div className="responsive-app">
      <div className="status-bar">
        <div className="window-info">
          📐 Window: {windowSize.width} × {windowSize.height}
        </div>
        <div className={`connection-status ${isOnline ? 'online' : 'offline'}`}>
          {isOnline ? '🟢 Online' : '🔴 Offline'}
        </div>
      </div>
      
      <div className="content">
        {windowSize.width < 768 ? (
          <MobileLayout />
        ) : (
          <DesktopLayout />
        )}
      </div>
    </div>
  );
}
```

## Advanced Sorcery

### Custom Store Implementation

```jsx
import React, { useSyncExternalStore, useCallback } from 'react';

// Create a custom store class
class CustomStore {
  constructor(initialState = {}) {
    this.state = initialState;
    this.listeners = new Set();
  }

  subscribe = (listener) => {
    this.listeners.add(listener);
    return () => this.listeners.delete(listener);
  };

  getSnapshot = () => {
    return this.state;
  };

  setState = (newState) => {
    this.state = { ...this.state, ...newState };
    this.listeners.forEach(listener => listener());
  };

  getState = () => {
    return this.state;
  };
}

// Create store instance
const userStore = new CustomStore({
  user: null,
  preferences: {
    theme: 'light',
    language: 'en'
  },
  notifications: []
});

// Custom hook to use the store
function useStore(selector = (state) => state) {
  const state = useSyncExternalStore(
    userStore.subscribe,
    () => selector(userStore.getSnapshot()),
    () => selector(userStore.getSnapshot()) // Same for server
  );

  const setState = useCallback((newState) => {
    userStore.setState(newState);
  }, []);

  return [state, setState];
}

// Specialized hooks for different parts of the store
function useUser() {
  return useStore(state => state.user);
}

function usePreferences() {
  return useStore(state => state.preferences);
}

function useNotifications() {
  return useStore(state => state.notifications);
}

// Usage components
function UserProfile() {
  const [user, setUser] = useUser();

  const updateUser = (updates) => {
    setUser({ user: { ...user, ...updates } });
  };

  if (!user) {
    return <div>Please log in</div>;
  }

  return (
    <div className="user-profile">
      <h2>Welcome, {user.name}!</h2>
      <div className="user-info">
        <input
          type="text"
          value={user.name}
          onChange={(e) => updateUser({ name: e.target.value })}
        />
        <input
          type="email"
          value={user.email}
          onChange={(e) => updateUser({ email: e.target.value })}
        />
      </div>
    </div>
  );
}

function ThemeSelector() {
  const [preferences, setPreferences] = usePreferences();

  const updateTheme = (theme) => {
    setPreferences({ preferences: { ...preferences, theme } });
  };

  return (
    <div className="theme-selector">
      <h3>Theme Preferences</h3>
      <div className="theme-options">
        {['light', 'dark', 'auto'].map(theme => (
          <button
            key={theme}
            onClick={() => updateTheme(theme)}
            className={preferences.theme === theme ? 'active' : ''}
          >
            {theme.charAt(0).toUpperCase() + theme.slice(1)}
          </button>
        ))}
      </div>
    </div>
  );
}
```

### LocalStorage Integration

```jsx
import React, { useSyncExternalStore, useCallback } from 'react';

// LocalStorage store implementation
class LocalStorageStore {
  constructor(key, defaultValue = null) {
    this.key = key;
    this.defaultValue = defaultValue;
    this.listeners = new Set();
    
    // Listen for storage events from other tabs
    window.addEventListener('storage', this.handleStorageChange);
  }

  handleStorageChange = (e) => {
    if (e.key === this.key) {
      this.listeners.forEach(listener => listener());
    }
  };

  subscribe = (listener) => {
    this.listeners.add(listener);
    return () => this.listeners.delete(listener);
  };

  getSnapshot = () => {
    try {
      const item = localStorage.getItem(this.key);
      return item ? JSON.parse(item) : this.defaultValue;
    } catch (error) {
      console.error('Error reading from localStorage:', error);
      return this.defaultValue;
    }
  };

  getServerSnapshot = () => {
    return this.defaultValue;
  };

  setValue = (value) => {
    try {
      localStorage.setItem(this.key, JSON.stringify(value));
      this.listeners.forEach(listener => listener());
    } catch (error) {
      console.error('Error writing to localStorage:', error);
    }
  };

  removeValue = () => {
    try {
      localStorage.removeItem(this.key);
      this.listeners.forEach(listener => listener());
    } catch (error) {
      console.error('Error removing from localStorage:', error);
    }
  };
}

// Custom hook for localStorage
function useLocalStorage(key, defaultValue = null) {
  const store = React.useMemo(
    () => new LocalStorageStore(key, defaultValue),
    [key, defaultValue]
  );

  const value = useSyncExternalStore(
    store.subscribe,
    store.getSnapshot,
    store.getServerSnapshot
  );

  const setValue = useCallback((newValue) => {
    store.setValue(newValue);
  }, [store]);

  const removeValue = useCallback(() => {
    store.removeValue();
  }, [store]);

  return [value, setValue, removeValue];
}

// Usage component
function SettingsManager() {
  const [settings, setSettings, removeSettings] = useLocalStorage('app-settings', {
    notifications: true,
    autoSave: false,
    theme: 'light'
  });

  const updateSetting = (key, value) => {
    setSettings({ ...settings, [key]: value });
  };

  return (
    <div className="settings-manager">
      <h2>Application Settings</h2>
      
      <div className="setting-group">
        <label>
          <input
            type="checkbox"
            checked={settings.notifications}
            onChange={(e) => updateSetting('notifications', e.target.checked)}
          />
          Enable Notifications
        </label>
      </div>
      
      <div className="setting-group">
        <label>
          <input
            type="checkbox"
            checked={settings.autoSave}
            onChange={(e) => updateSetting('autoSave', e.target.checked)}
          />
          Auto Save
        </label>
      </div>
      
      <div className="setting-group">
        <label>
          Theme:
          <select
            value={settings.theme}
            onChange={(e) => updateSetting('theme', e.target.value)}
          >
            <option value="light">Light</option>
            <option value="dark">Dark</option>
            <option value="auto">Auto</option>
          </select>
        </label>
      </div>
      
      <button onClick={removeSettings} className="reset-button">
        Reset to Defaults
      </button>
    </div>
  );
}
```

### WebSocket Integration

```jsx
import React, { useSyncExternalStore, useCallback, useRef } from 'react';

// WebSocket store implementation
class WebSocketStore {
  constructor(url) {
    this.url = url;
    this.listeners = new Set();
    this.data = null;
    this.status = 'disconnected';
    this.ws = null;
    this.reconnectAttempts = 0;
    this.maxReconnectAttempts = 5;
  }

  connect = () => {
    if (this.ws?.readyState === WebSocket.OPEN) return;

    this.ws = new WebSocket(this.url);
    this.status = 'connecting';
    this.notifyListeners();

    this.ws.onopen = () => {
      this.status = 'connected';
      this.reconnectAttempts = 0;
      this.notifyListeners();
    };

    this.ws.onmessage = (event) => {
      try {
        this.data = JSON.parse(event.data);
        this.notifyListeners();
      } catch (error) {
        console.error('Error parsing WebSocket message:', error);
      }
    };

    this.ws.onclose = () => {
      this.status = 'disconnected';
      this.notifyListeners();
      this.attemptReconnect();
    };

    this.ws.onerror = (error) => {
      console.error('WebSocket error:', error);
      this.status = 'error';
      this.notifyListeners();
    };
  };

  disconnect = () => {
    if (this.ws) {
      this.ws.close();
      this.ws = null;
    }
    this.status = 'disconnected';
    this.notifyListeners();
  };

  send = (message) => {
    if (this.ws?.readyState === WebSocket.OPEN) {
      this.ws.send(JSON.stringify(message));
    }
  };

  attemptReconnect = () => {
    if (this.reconnectAttempts < this.maxReconnectAttempts) {
      this.reconnectAttempts++;
      setTimeout(() => {
        this.connect();
      }, Math.pow(2, this.reconnectAttempts) * 1000); // Exponential backoff
    }
  };

  subscribe = (listener) => {
    this.listeners.add(listener);
    return () => this.listeners.delete(listener);
  };

  getSnapshot = () => ({
    data: this.data,
    status: this.status,
    reconnectAttempts: this.reconnectAttempts
  });

  notifyListeners = () => {
    this.listeners.forEach(listener => listener());
  };
}

// Custom hook for WebSocket
function useWebSocket(url) {
  const storeRef = useRef(null);

  if (!storeRef.current) {
    storeRef.current = new WebSocketStore(url);
  }

  const state = useSyncExternalStore(
    storeRef.current.subscribe,
    storeRef.current.getSnapshot,
    () => ({ data: null, status: 'disconnected', reconnectAttempts: 0 })
  );

  const connect = useCallback(() => {
    storeRef.current.connect();
  }, []);

  const disconnect = useCallback(() => {
    storeRef.current.disconnect();
  }, []);

  const send = useCallback((message) => {
    storeRef.current.send(message);
  }, []);

  return {
    ...state,
    connect,
    disconnect,
    send
  };
}

// Usage component
function RealTimeChat() {
  const { data, status, connect, disconnect, send } = useWebSocket('ws://localhost:8080/chat');
  const [message, setMessage] = React.useState('');
  const [messages, setMessages] = React.useState([]);

  React.useEffect(() => {
    if (data) {
      setMessages(prev => [...prev, data]);
    }
  }, [data]);

  React.useEffect(() => {
    connect();
    return () => disconnect();
  }, [connect, disconnect]);

  const sendMessage = () => {
    if (message.trim() && status === 'connected') {
      send({ type: 'message', content: message, timestamp: Date.now() });
      setMessage('');
    }
  };

  return (
    <div className="realtime-chat">
      <div className={`connection-status ${status}`}>
        Status: {status}
        {status === 'connecting' && ' 🔄'}
        {status === 'connected' && ' 🟢'}
        {status === 'disconnected' && ' 🔴'}
        {status === 'error' && ' ❌'}
      </div>

      <div className="messages-container">
        {messages.map((msg, index) => (
          <div key={index} className="message">
            <span className="timestamp">
              {new Date(msg.timestamp).toLocaleTimeString()}
            </span>
            <span className="content">{msg.content}</span>
          </div>
        ))}
      </div>

      <div className="message-input">
        <input
          type="text"
          value={message}
          onChange={(e) => setMessage(e.target.value)}
          onKeyPress={(e) => e.key === 'Enter' && sendMessage()}
          placeholder="Type a message..."
          disabled={status !== 'connected'}
        />
        <button
          onClick={sendMessage}
          disabled={status !== 'connected' || !message.trim()}
        >
          Send
        </button>
      </div>
    </div>
  );
}
```

### Third-Party Library Integration

```jsx
import React, { useSyncExternalStore } from 'react';

// Example integration with a hypothetical external library
class ExternalLibraryAdapter {
  constructor(library) {
    this.library = library;
    this.listeners = new Set();

    // Subscribe to library events
    this.library.on('change', this.handleChange);
    this.library.on('error', this.handleError);
  }

  handleChange = (data) => {
    this.currentData = data;
    this.listeners.forEach(listener => listener());
  };

  handleError = (error) => {
    this.currentError = error;
    this.listeners.forEach(listener => listener());
  };

  subscribe = (listener) => {
    this.listeners.add(listener);
    return () => {
      this.listeners.delete(listener);
      if (this.listeners.size === 0) {
        // Cleanup when no more listeners
        this.library.off('change', this.handleChange);
        this.library.off('error', this.handleError);
      }
    };
  };

  getSnapshot = () => ({
    data: this.currentData,
    error: this.currentError,
    isLoading: this.library.isLoading
  });

  getServerSnapshot = () => ({
    data: null,
    error: null,
    isLoading: false
  });
}

// Custom hook for external library
function useExternalLibrary(library) {
  const adapter = React.useMemo(
    () => new ExternalLibraryAdapter(library),
    [library]
  );

  return useSyncExternalStore(
    adapter.subscribe,
    adapter.getSnapshot,
    adapter.getServerSnapshot
  );
}

// Usage component
function DataVisualization({ dataSource }) {
  const { data, error, isLoading } = useExternalLibrary(dataSource);

  if (isLoading) {
    return <div className="loading">📊 Loading data visualization...</div>;
  }

  if (error) {
    return (
      <div className="error">
        ❌ Error loading data: {error.message}
      </div>
    );
  }

  return (
    <div className="data-visualization">
      <h3>Real-time Data</h3>
      {data && (
        <div className="chart-container">
          <Chart data={data} />
        </div>
      )}
    </div>
  );
}
```

## Performance Optimization Patterns

### Selective Subscriptions

```jsx
import React, { useSyncExternalStore, useMemo } from 'react';

// Store with selective subscription capability
class SelectiveStore {
  constructor() {
    this.state = {
      user: { name: 'John', email: 'john@example.com' },
      posts: [],
      notifications: [],
      settings: { theme: 'light' }
    };
    this.listeners = new Map(); // Key-based listeners
  }

  subscribe = (listener, selector) => {
    const key = selector.toString();
    if (!this.listeners.has(key)) {
      this.listeners.set(key, new Set());
    }
    this.listeners.get(key).add(listener);

    return () => {
      const keyListeners = this.listeners.get(key);
      if (keyListeners) {
        keyListeners.delete(listener);
        if (keyListeners.size === 0) {
          this.listeners.delete(key);
        }
      }
    };
  };

  getSnapshot = (selector) => {
    return selector(this.state);
  };

  setState = (updates) => {
    const prevState = this.state;
    this.state = { ...this.state, ...updates };

    // Notify only relevant listeners
    this.listeners.forEach((listeners, selectorKey) => {
      // This is a simplified check - in practice, you'd want more sophisticated comparison
      listeners.forEach(listener => listener());
    });
  };
}

const selectiveStore = new SelectiveStore();

// Custom hook with selector
function useSelectiveStore(selector) {
  const memoizedSelector = useMemo(() => selector, [selector]);

  return useSyncExternalStore(
    (listener) => selectiveStore.subscribe(listener, memoizedSelector),
    () => selectiveStore.getSnapshot(memoizedSelector),
    () => selectiveStore.getSnapshot(memoizedSelector)
  );
}

// Usage with different selectors
function UserProfile() {
  const user = useSelectiveStore(state => state.user);

  return (
    <div className="user-profile">
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}

function NotificationBadge() {
  const notificationCount = useSelectiveStore(state => state.notifications.length);

  return (
    <div className="notification-badge">
      {notificationCount > 0 && (
        <span className="badge">{notificationCount}</span>
      )}
    </div>
  );
}
```

## Wisdom of the External Store Ancients

- **Consistent Snapshots**: Always return the same reference for identical data to prevent unnecessary re-renders
- **Proper Cleanup**: Ensure subscription cleanup functions properly remove event listeners
- **Server-Side Rendering**: Provide appropriate server snapshots to prevent hydration mismatches
- **Error Handling**: Implement robust error handling for external data sources
- **Performance Optimization**: Use selectors and memoization to minimize unnecessary updates
- **Tearing Prevention**: The hook automatically prevents visual inconsistencies during concurrent updates

## Common Curses & Their Remedies

### Curse 1: Inconsistent Snapshot References
Returning new objects on every getSnapshot call causes unnecessary re-renders.

**Counter-Spell:**
```jsx
// ❌ Creating new objects every time
const getSnapshot = () => ({
  width: window.innerWidth,
  height: window.innerHeight
});

// ✅ Memoize or cache identical values
let cachedSnapshot = null;
const getSnapshot = () => {
  const current = {
    width: window.innerWidth,
    height: window.innerHeight
  };

  if (!cachedSnapshot ||
      cachedSnapshot.width !== current.width ||
      cachedSnapshot.height !== current.height) {
    cachedSnapshot = current;
  }

  return cachedSnapshot;
};
```

### Curse 2: Forgetting Server Snapshot
Missing server snapshots cause hydration mismatches in SSR applications.

**Banishment Ritual:**
```jsx
// ❌ No server snapshot provided
const value = useSyncExternalStore(subscribe, getSnapshot);

// ✅ Always provide server snapshot for SSR
const value = useSyncExternalStore(
  subscribe,
  getSnapshot,
  getServerSnapshot // Essential for SSR
);
```

### Curse 3: Memory Leaks from Improper Cleanup
Failing to properly clean up subscriptions leads to memory leaks.

**Protective Ward:**
```jsx
// ❌ No cleanup or improper cleanup
const subscribe = (callback) => {
  window.addEventListener('resize', callback);
  // Missing return cleanup function!
};

// ✅ Always return cleanup function
const subscribe = (callback) => {
  window.addEventListener('resize', callback);
  return () => window.removeEventListener('resize', callback);
};
```

### Curse 4: Synchronous Updates in Subscribe Function
Calling the callback synchronously during subscription can cause issues.

**Remedy Incantation:**
```jsx
// ❌ Synchronous callback during subscription
const subscribe = (callback) => {
  callback(); // Don't do this!
  return someCleanupFunction;
};

// ✅ Let React handle the initial snapshot
const subscribe = (callback) => {
  // Only call callback for actual changes
  someExternalSource.addEventListener('change', callback);
  return () => someExternalSource.removeEventListener('change', callback);
};
```

## Sacred Texts & Mystical Sources

{% embed url="https://react.dev/reference/react/useSyncExternalStore" %}

{% embed url="https://github.com/reactwg/react-18/discussions/86" %}

{% embed url="https://react.dev/learn/you-might-not-need-an-effect" %}
```
```
