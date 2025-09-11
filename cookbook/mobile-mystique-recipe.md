---
description: >-
  Master cross-platform mobile development with React Native and Progressive Web Apps. This comprehensive recipe covers native app development, PWA implementation, offline capabilities, device integration, state synchronization, and app store deployment for reaching users across all platforms.

theme: "magic"
---

# 📱 Mobile Mystique: React Native & PWA Recipe

*When you need to conquer all platforms with one codebase*

---

## 🎯 Cast This When You Need

**Cross-Platform Power** • **Native Performance** • **Shared Codebase** • **App Store Distribution** • **Offline Capabilities**

Perfect for: Mobile apps, cross-platform solutions, teams wanting to maintain one codebase, apps needing native features

Terrible for: Web-only experiences, platform-specific features, when you need maximum native performance

---

## 🚀 The Cross-Platform Summoning

### Option A: React Native (True Native Apps)
```bash
# Create React Native project with Expo
npx create-expo-app@latest MyMobileApp --template

cd MyMobileApp

# Install essential superpowers
npx expo install expo-router expo-constants expo-status-bar
npx expo install @react-native-async-storage/async-storage
npx expo install expo-secure-store expo-notifications
npx expo install react-native-reanimated react-native-gesture-handler

# Launch development server
npx expo start
```

### Option B: PWA (Web App That Feels Native)
```bash
# Create PWA-ready Vite project
npm create vite@latest my-pwa-app -- --template react-ts

cd my-pwa-app
npm install

# Install PWA superpowers
npm install -D vite-plugin-pwa workbox-window
npm install @capacitor/core @capacitor/ios @capacitor/android

# Setup PWA capabilities
npm run build
```

---

## 🏗️ The Universal Architecture

```
src/
├── components/          # Shared UI components
│   ├── common/         # Cross-platform components
│   ├── native/         # React Native specific
│   └── web/           # Web/PWA specific
├── screens/           # App screens/pages
│   ├── HomeScreen.tsx
│   ├── ProfileScreen.tsx
│   └── SettingsScreen.tsx
├── navigation/        # Navigation setup
├── services/          # API & business logic
├── hooks/             # Custom React hooks
├── utils/             # Cross-platform utilities
├── store/             # State management
└── types/             # TypeScript definitions
```

---

## 📱 React Native Power Patterns

### Navigation Master Setup
```typescript
// src/navigation/AppNavigator.tsx
import { NavigationContainer } from '@react-navigation/native'
import { createNativeStackNavigator } from '@react-navigation/native-stack'
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs'
import { Ionicons } from '@expo/vector-icons'

import HomeScreen from '../screens/HomeScreen'
import ProfileScreen from '../screens/ProfileScreen'
import SettingsScreen from '../screens/SettingsScreen'
import LoginScreen from '../screens/auth/LoginScreen'

const Stack = createNativeStackNavigator()
const Tab = createBottomTabNavigator()

function TabNavigator() {
  return (
    <Tab.Navigator
      screenOptions={({ route }) => ({
        tabBarIcon: ({ focused, color, size }) => {
          let iconName: keyof typeof Ionicons.glyphMap

          switch (route.name) {
            case 'Home':
              iconName = focused ? 'home' : 'home-outline'
              break
            case 'Profile':
              iconName = focused ? 'person' : 'person-outline'
              break
            case 'Settings':
              iconName = focused ? 'settings' : 'settings-outline'
              break
            default:
              iconName = 'circle'
          }

          return <Ionicons name={iconName} size={size} color={color} />
        },
        tabBarActiveTintColor: '#007AFF',
        tabBarInactiveTintColor: 'gray',
        headerShown: false,
      })}
    >
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Profile" component={ProfileScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  )
}

export default function AppNavigator() {
  const [isAuthenticated, setIsAuthenticated] = useState(false)
  
  useEffect(() => {
    // Check authentication status
    checkAuthStatus()
  }, [])

  return (
    <NavigationContainer>
      <Stack.Navigator screenOptions={{ headerShown: false }}>
        {isAuthenticated ? (
          <Stack.Screen name="Main" component={TabNavigator} />
        ) : (
          <Stack.Screen name="Login" component={LoginScreen} />
        )}
      </Stack.Navigator>
    </NavigationContainer>
  )
}
```

### Universal Component Pattern
```typescript
// src/components/common/Button.tsx
import React from 'react'
import { 
  TouchableOpacity, 
  Text, 
  StyleSheet, 
  ViewStyle, 
  TextStyle 
} from 'react-native'

interface ButtonProps {
  title: string
  onPress: () => void
  variant?: 'primary' | 'secondary' | 'danger'
  size?: 'small' | 'medium' | 'large'
  disabled?: boolean
  loading?: boolean
  style?: ViewStyle
}

const Button: React.FC<ButtonProps> = ({
  title,
  onPress,
  variant = 'primary',
  size = 'medium',
  disabled = false,
  loading = false,
  style,
}) => {
  const buttonStyle = [
    styles.base,
    styles[variant],
    styles[size],
    disabled && styles.disabled,
    style,
  ]

  const textStyle = [
    styles.text,
    styles[`${variant}Text`],
    styles[`${size}Text`],
    disabled && styles.disabledText,
  ]

  return (
    <TouchableOpacity
      style={buttonStyle}
      onPress={onPress}
      disabled={disabled || loading}
      activeOpacity={0.7}
    >
      <Text style={textStyle}>
        {loading ? 'Loading...' : title}
      </Text>
    </TouchableOpacity>
  )
}

const styles = StyleSheet.create({
  base: {
    borderRadius: 8,
    justifyContent: 'center',
    alignItems: 'center',
    flexDirection: 'row',
  },
  primary: {
    backgroundColor: '#007AFF',
  },
  secondary: {
    backgroundColor: '#F2F2F7',
    borderWidth: 1,
    borderColor: '#C7C7CC',
  },
  danger: {
    backgroundColor: '#FF3B30',
  },
  small: {
    paddingHorizontal: 12,
    paddingVertical: 6,
  },
  medium: {
    paddingHorizontal: 20,
    paddingVertical: 12,
  },
  large: {
    paddingHorizontal: 32,
    paddingVertical: 16,
  },
  disabled: {
    opacity: 0.5,
  },
  text: {
    fontWeight: '600',
  },
  primaryText: {
    color: 'white',
  },
  secondaryText: {
    color: '#007AFF',
  },
  dangerText: {
    color: 'white',
  },
  smallText: {
    fontSize: 14,
  },
  mediumText: {
    fontSize: 16,
  },
  largeText: {
    fontSize: 18,
  },
  disabledText: {
    opacity: 0.7,
  },
})

export default Button
```

### Native Device Features Integration
```typescript
// src/services/DeviceService.ts
import * as Notifications from 'expo-notifications'
import * as Location from 'expo-location'
import * as Camera from 'expo-camera'
import AsyncStorage from '@react-native-async-storage/async-storage'
import * as SecureStore from 'expo-secure-store'

export class DeviceService {
  // Push Notifications
  static async setupNotifications() {
    const { status } = await Notifications.requestPermissionsAsync()
    
    if (status !== 'granted') {
      throw new Error('Notification permission not granted')
    }

    const token = (await Notifications.getExpoPushTokenAsync()).data
    
    // Configure notification behavior
    Notifications.setNotificationHandler({
      handleNotification: async () => ({
        shouldShowAlert: true,
        shouldPlaySound: false,
        shouldSetBadge: false,
      }),
    })

    return token
  }

  static async sendLocalNotification(title: string, body: string, data?: any) {
    await Notifications.scheduleNotificationAsync({
      content: {
        title,
        body,
        data,
      },
      trigger: { seconds: 1 },
    })
  }

  // Location Services
  static async getCurrentLocation() {
    const { status } = await Location.requestForegroundPermissionsAsync()
    
    if (status !== 'granted') {
      throw new Error('Location permission not granted')
    }

    const location = await Location.getCurrentPositionAsync({
      accuracy: Location.Accuracy.High,
    })

    return {
      latitude: location.coords.latitude,
      longitude: location.coords.longitude,
      accuracy: location.coords.accuracy,
      timestamp: location.timestamp,
    }
  }

  // Camera & Media
  static async takePicture() {
    const { status } = await Camera.requestCameraPermissionsAsync()
    
    if (status !== 'granted') {
      throw new Error('Camera permission not granted')
    }

    // Return camera component props
    return { hasPermission: true }
  }

  // Secure Storage
  static async storeSecurely(key: string, value: string) {
    await SecureStore.setItemAsync(key, value)
  }

  static async getSecurely(key: string) {
    return await SecureStore.getItemAsync(key)
  }

  static async removeSecurely(key: string) {
    await SecureStore.deleteItemAsync(key)
  }

  // Regular Storage
  static async store(key: string, value: any) {
    await AsyncStorage.setItem(key, JSON.stringify(value))
  }

  static async retrieve(key: string) {
    const value = await AsyncStorage.getItem(key)
    return value ? JSON.parse(value) : null
  }

  static async remove(key: string) {
    await AsyncStorage.removeItem(key)
  }
}
```

### Smooth Animations & Gestures
```typescript
// src/components/AnimatedCard.tsx
import React from 'react'
import {
  View,
  StyleSheet,
  Dimensions,
} from 'react-native'
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  useAnimatedGestureHandler,
  withSpring,
  withTiming,
  interpolate,
  runOnJS,
} from 'react-native-reanimated'
import {
  PanGestureHandler,
  TapGestureHandler,
} from 'react-native-gesture-handler'

const { width: screenWidth } = Dimensions.get('window')
const CARD_WIDTH = screenWidth * 0.8

interface AnimatedCardProps {
  children: React.ReactNode
  onSwipeLeft?: () => void
  onSwipeRight?: () => void
  onTap?: () => void
}

const AnimatedCard: React.FC<AnimatedCardProps> = ({
  children,
  onSwipeLeft,
  onSwipeRight,
  onTap,
}) => {
  const translateX = useSharedValue(0)
  const scale = useSharedValue(1)
  const opacity = useSharedValue(1)

  const panGestureHandler = useAnimatedGestureHandler({
    onStart: () => {
      scale.value = withSpring(0.95)
    },
    onActive: (event) => {
      translateX.value = event.translationX
      opacity.value = interpolate(
        Math.abs(event.translationX),
        [0, screenWidth / 2],
        [1, 0.5]
      )
    },
    onEnd: (event) => {
      scale.value = withSpring(1)
      
      const shouldSwipeLeft = event.translationX < -CARD_WIDTH / 3
      const shouldSwipeRight = event.translationX > CARD_WIDTH / 3

      if (shouldSwipeLeft) {
        translateX.value = withTiming(-screenWidth)
        opacity.value = withTiming(0, {}, () => {
          onSwipeLeft && runOnJS(onSwipeLeft)()
        })
      } else if (shouldSwipeRight) {
        translateX.value = withTiming(screenWidth)
        opacity.value = withTiming(0, {}, () => {
          onSwipeRight && runOnJS(onSwipeRight)()
        })
      } else {
        translateX.value = withSpring(0)
        opacity.value = withSpring(1)
      }
    },
  })

  const tapGestureHandler = useAnimatedGestureHandler({
    onStart: () => {
      scale.value = withSpring(0.98)
    },
    onEnd: () => {
      scale.value = withSpring(1)
      onTap && runOnJS(onTap)()
    },
  })

  const animatedStyle = useAnimatedStyle(() => ({
    transform: [
      { translateX: translateX.value },
      { scale: scale.value },
    ],
    opacity: opacity.value,
  }))

  return (
    <PanGestureHandler onGestureEvent={panGestureHandler}>
      <Animated.View>
        <TapGestureHandler onGestureEvent={tapGestureHandler}>
          <Animated.View style={[styles.card, animatedStyle]}>
            {children}
          </Animated.View>
        </TapGestureHandler>
      </Animated.View>
    </PanGestureHandler>
  )
}

const styles = StyleSheet.create({
  card: {
    width: CARD_WIDTH,
    height: 200,
    backgroundColor: 'white',
    borderRadius: 16,
    padding: 20,
    shadowColor: '#000',
    shadowOffset: {
      width: 0,
      height: 4,
    },
    shadowOpacity: 0.3,
    shadowRadius: 4.65,
    elevation: 8,
  },
})

export default AnimatedCard
```

---

## 🌐 PWA Superpowers

### Service Worker & Offline Magic
```typescript
// src/sw.ts - Service Worker
import { precacheAndRoute, cleanupOutdatedCaches } from 'workbox-precaching'
import { registerRoute } from 'workbox-routing'
import { StaleWhileRevalidate, CacheFirst, NetworkFirst } from 'workbox-strategies'

declare const self: ServiceWorkerGlobalScope

// Precache app shell
precacheAndRoute(self.__WB_MANIFEST)
cleanupOutdatedCaches()

// Cache API responses
registerRoute(
  ({ url }) => url.pathname.startsWith('/api/'),
  new NetworkFirst({
    cacheName: 'api-cache',
    networkTimeoutSeconds: 3,
    plugins: [{
      cacheKeyWillBeUsed: async ({ request }) => {
        return `${request.url}?timestamp=${Math.floor(Date.now() / 1000 / 60)}`
      }
    }]
  })
)

// Cache images
registerRoute(
  ({ request }) => request.destination === 'image',
  new CacheFirst({
    cacheName: 'images',
    plugins: [{
      cacheMaxEntries: 100,
      cacheMaxAgeSeconds: 30 * 24 * 60 * 60, // 30 days
    }]
  })
)

// Cache fonts
registerRoute(
  ({ url }) => url.pathname.includes('/fonts/'),
  new CacheFirst({
    cacheName: 'fonts',
    plugins: [{
      cacheMaxAgeSeconds: 365 * 24 * 60 * 60, // 1 year
    }]
  })
)

// Background sync for offline actions
self.addEventListener('sync', (event) => {
  if (event.tag === 'background-sync') {
    event.waitUntil(handleBackgroundSync())
  }
})

async function handleBackgroundSync() {
  // Process queued offline actions
  const offlineQueue = await getOfflineQueue()
  
  for (const action of offlineQueue) {
    try {
      await fetch(action.url, action.options)
      await removeFromOfflineQueue(action.id)
    } catch (error) {
      console.error('Background sync failed:', error)
    }
  }
}
```

### PWA Installation & Native Features
```typescript
// src/hooks/usePWAInstall.ts
import { useState, useEffect } from 'react'

interface BeforeInstallPromptEvent extends Event {
  prompt(): Promise<void>
  userChoice: Promise<{ outcome: 'accepted' | 'dismissed' }>
}

export function usePWAInstall() {
  const [deferredPrompt, setDeferredPrompt] = useState<BeforeInstallPromptEvent | null>(null)
  const [isInstallable, setIsInstallable] = useState(false)
  const [isInstalled, setIsInstalled] = useState(false)

  useEffect(() => {
    const handler = (e: BeforeInstallPromptEvent) => {
      e.preventDefault()
      setDeferredPrompt(e)
      setIsInstallable(true)
    }

    window.addEventListener('beforeinstallprompt', handler as any)

    // Check if already installed
    if (window.matchMedia('(display-mode: standalone)').matches) {
      setIsInstalled(true)
    }

    return () => {
      window.removeEventListener('beforeinstallprompt', handler as any)
    }
  }, [])

  const installPWA = async () => {
    if (!deferredPrompt) return

    deferredPrompt.prompt()
    const { outcome } = await deferredPrompt.userChoice

    if (outcome === 'accepted') {
      setIsInstalled(true)
    }

    setDeferredPrompt(null)
    setIsInstallable(false)
  }

  return { isInstallable, isInstalled, installPWA }
}
```

### Native-like Navigation
```typescript
// src/components/BottomNavigation.tsx
import React from 'react'
import { useLocation, useNavigate } from 'react-router-dom'
import './BottomNavigation.css'

interface NavItem {
  path: string
  icon: string
  label: string
  badge?: number
}

const navItems: NavItem[] = [
  { path: '/', icon: '🏠', label: 'Home' },
  { path: '/search', icon: '🔍', label: 'Search' },
  { path: '/favorites', icon: '❤️', label: 'Favorites', badge: 3 },
  { path: '/profile', icon: '👤', label: 'Profile' },
]

export function BottomNavigation() {
  const location = useLocation()
  const navigate = useNavigate()

  return (
    <nav className="bottom-nav">
      {navItems.map((item) => (
        <button
          key={item.path}
          className={`nav-item ${location.pathname === item.path ? 'active' : ''}`}
          onClick={() => navigate(item.path)}
          aria-label={item.label}
        >
          <div className="nav-icon-container">
            <span className="nav-icon">{item.icon}</span>
            {item.badge && (
              <span className="nav-badge">{item.badge}</span>
            )}
          </div>
          <span className="nav-label">{item.label}</span>
        </button>
      ))}
    </nav>
  )
}
```

```css
/* src/components/BottomNavigation.css */
.bottom-nav {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(20px);
  border-top: 1px solid #e5e5e5;
  padding: 8px 0;
  z-index: 1000;
  safe-area-inset-bottom: env(safe-area-inset-bottom, 0px);
  padding-bottom: calc(8px + env(safe-area-inset-bottom, 0px));
}

.nav-item {
  flex: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 8px;
  border: none;
  background: none;
  color: #666;
  text-decoration: none;
  transition: all 0.2s ease;
  cursor: pointer;
}

.nav-item:hover {
  color: #007AFF;
  transform: scale(1.05);
}

.nav-item.active {
  color: #007AFF;
}

.nav-icon-container {
  position: relative;
  margin-bottom: 4px;
}

.nav-icon {
  font-size: 20px;
  display: block;
}

.nav-badge {
  position: absolute;
  top: -8px;
  right: -8px;
  background: #FF3B30;
  color: white;
  border-radius: 10px;
  padding: 2px 6px;
  font-size: 12px;
  font-weight: bold;
  min-width: 16px;
  text-align: center;
}

.nav-label {
  font-size: 12px;
  font-weight: 500;
}

/* iOS safe area support */
@supports (padding-bottom: env(safe-area-inset-bottom)) {
  .bottom-nav {
    padding-bottom: calc(8px + env(safe-area-inset-bottom));
  }
}
```

---

## 🔋 State Management & Data Sync

### Universal Store with Zustand
```typescript
// src/store/useAppStore.ts
import { create } from 'zustand'
import { persist, createJSONStorage } from 'zustand/middleware'
import AsyncStorage from '@react-native-async-storage/async-storage'

interface User {
  id: string
  name: string
  email: string
  avatar?: string
}

interface AppState {
  // User state
  user: User | null
  isAuthenticated: boolean
  
  // App state
  theme: 'light' | 'dark'
  notifications: boolean
  isOnline: boolean
  
  // Data
  favorites: string[]
  searchHistory: string[]
  
  // Actions
  setUser: (user: User | null) => void
  login: (email: string, password: string) => Promise<void>
  logout: () => void
  toggleTheme: () => void
  addFavorite: (id: string) => void
  removeFavorite: (id: string) => void
  addToSearchHistory: (query: string) => void
  setOnlineStatus: (isOnline: boolean) => void
}

export const useAppStore = create<AppState>()(
  persist(
    (set, get) => ({
      // Initial state
      user: null,
      isAuthenticated: false,
      theme: 'light',
      notifications: true,
      isOnline: true,
      favorites: [],
      searchHistory: [],

      // Actions
      setUser: (user) => set({ user, isAuthenticated: !!user }),

      login: async (email, password) => {
        try {
          const response = await fetch('/api/auth/login', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({ email, password }),
          })

          if (response.ok) {
            const user = await response.json()
            set({ user, isAuthenticated: true })
          } else {
            throw new Error('Login failed')
          }
        } catch (error) {
          throw error
        }
      },

      logout: () => {
        set({ user: null, isAuthenticated: false })
      },

      toggleTheme: () => {
        const newTheme = get().theme === 'light' ? 'dark' : 'light'
        set({ theme: newTheme })
      },

      addFavorite: (id) => {
        const favorites = get().favorites
        if (!favorites.includes(id)) {
          set({ favorites: [...favorites, id] })
        }
      },

      removeFavorite: (id) => {
        const favorites = get().favorites.filter(fav => fav !== id)
        set({ favorites })
      },

      addToSearchHistory: (query) => {
        const history = get().searchHistory
        const newHistory = [query, ...history.filter(h => h !== query)].slice(0, 10)
        set({ searchHistory: newHistory })
      },

      setOnlineStatus: (isOnline) => set({ isOnline }),
    }),
    {
      name: 'app-storage',
      storage: createJSONStorage(() => AsyncStorage), // React Native
      // storage: createJSONStorage(() => localStorage), // Web
      partialize: (state) => ({
        user: state.user,
        isAuthenticated: state.isAuthenticated,
        theme: state.theme,
        notifications: state.notifications,
        favorites: state.favorites,
        searchHistory: state.searchHistory,
      }),
    }
  )
)
```

### Offline Data Sync
```typescript
// src/services/SyncService.ts
import NetInfo from '@react-native-netinfo'
import { useAppStore } from '../store/useAppStore'

interface SyncAction {
  id: string
  type: 'CREATE' | 'UPDATE' | 'DELETE'
  endpoint: string
  data: any
  timestamp: number
}

export class SyncService {
  private static syncQueue: SyncAction[] = []
  private static isOnline = true

  static initialize() {
    // Monitor network status
    const unsubscribe = NetInfo.addEventListener(state => {
      const wasOffline = !this.isOnline
      this.isOnline = state.isConnected ?? false
      
      useAppStore.getState().setOnlineStatus(this.isOnline)
      
      // Sync when coming back online
      if (wasOffline && this.isOnline) {
        this.syncPendingActions()
      }
    })

    return unsubscribe
  }

  static async makeRequest(endpoint: string, options: RequestInit = {}) {
    if (this.isOnline) {
      try {
        const response = await fetch(endpoint, options)
        return response
      } catch (error) {
        // Network error - queue for later
        this.queueAction(endpoint, options)
        throw error
      }
    } else {
      // Offline - queue action
      this.queueAction(endpoint, options)
      throw new Error('Offline - action queued')
    }
  }

  private static queueAction(endpoint: string, options: RequestInit) {
    const action: SyncAction = {
      id: Date.now().toString(),
      type: (options.method as any) || 'GET',
      endpoint,
      data: options.body ? JSON.parse(options.body as string) : null,
      timestamp: Date.now(),
    }

    this.syncQueue.push(action)
    this.persistSyncQueue()
  }

  private static async syncPendingActions() {
    const queue = [...this.syncQueue]
    this.syncQueue = []

    for (const action of queue) {
      try {
        await fetch(action.endpoint, {
          method: action.type,
          headers: { 'Content-Type': 'application/json' },
          body: action.data ? JSON.stringify(action.data) : undefined,
        })
      } catch (error) {
        // Re-queue failed actions
        this.syncQueue.push(action)
      }
    }

    this.persistSyncQueue()
  }

  private static persistSyncQueue() {
    // Save sync queue to AsyncStorage
    AsyncStorage.setItem('syncQueue', JSON.stringify(this.syncQueue))
  }

  static async loadPersistedQueue() {
    try {
      const stored = await AsyncStorage.getItem('syncQueue')
      if (stored) {
        this.syncQueue = JSON.parse(stored)
      }
    } catch (error) {
      console.error('Failed to load sync queue:', error)
    }
  }
}
```

---

## 📦 Build & Distribution

### React Native Build Configuration
```json
// app.json
{
  "expo": {
    "name": "My Mobile App",
    "slug": "my-mobile-app",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "userInterfaceStyle": "automatic",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#ffffff"
    },
    "assetBundlePatterns": [
      "**/*"
    ],
    "ios": {
      "supportsTablet": true,
      "bundleIdentifier": "com.yourcompany.mymobileapp",
      "buildNumber": "1",
      "infoPlist": {
        "NSCameraUsageDescription": "This app uses camera to take photos",
        "NSLocationWhenInUseUsageDescription": "This app uses location for better user experience"
      }
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#FFFFFF"
      },
      "package": "com.yourcompany.mymobileapp",
      "versionCode": 1,
      "permissions": [
        "android.permission.CAMERA",
        "android.permission.ACCESS_FINE_LOCATION",
        "android.permission.VIBRATE"
      ]
    },
    "web": {
      "favicon": "./assets/favicon.png",
      "bundler": "metro"
    },
    "plugins": [
      "expo-router",
      [
        "expo-notifications",
        {
          "icon": "./assets/notification-icon.png",
          "color": "#ffffff"
        }
      ]
    ]
  }
}
```

### PWA Manifest & Configuration
```json
// public/manifest.json
{
  "name": "My PWA App",
  "short_name": "PWA App",
  "description": "A powerful Progressive Web App",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ffffff",
  "theme_color": "#007AFF",
  "orientation": "portrait-primary",
  "icons": [
    {
      "src": "/icons/icon-72x72.png",
      "sizes": "72x72",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-96x96.png",
      "sizes": "96x96",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-128x128.png",
      "sizes": "128x128",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-152x152.png",
      "sizes": "152x152",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-384x384.png",
      "sizes": "384x384",
      "type": "image/png"
    },
    {
      "src": "/icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ],
  "categories": ["productivity", "utilities"],
  "screenshots": [
    {
      "src": "/screenshots/mobile-1.png",
      "sizes": "640x1136",
      "type": "image/png",
      "form_factor": "narrow"
    },
    {
      "src": "/screenshots/desktop-1.png",
      "sizes": "1280x720",
      "type": "image/png",
      "form_factor": "wide"
    }
  ]
}
```

### Vite PWA Configuration
```typescript
// vite.config.ts
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import { VitePWA } from 'vite-plugin-pwa'

export default defineConfig({
  plugins: [
    react(),
    VitePWA({
      registerType: 'autoUpdate',
      workbox: {
        globPatterns: ['**/*.{js,css,html,ico,png,svg}'],
        runtimeCaching: [
          {
            urlPattern: /^https:\/\/api\.myapp\.com\/.*/i,
            handler: 'NetworkFirst',
            options: {
              cacheName: 'api-cache',
              networkTimeoutSeconds: 3,
              cacheableResponse: {
                statuses: [0, 200],
              },
            },
          },
          {
            urlPattern: /\.(?:png|jpg|jpeg|svg|gif|webp)$/,
            handler: 'CacheFirst',
            options: {
              cacheName: 'images',
              expiration: {
                maxEntries: 100,
                maxAgeSeconds: 60 * 60 * 24 * 30, // 30 days
              },
            },
          },
        ],
      },
      manifest: {
        name: 'My PWA App',
        short_name: 'PWA App',
        description: 'A powerful Progressive Web App',
        theme_color: '#007AFF',
        background_color: '#ffffff',
        display: 'standalone',
        start_url: '/',
        icons: [
          {
            src: '/icons/icon-192x192.png',
            sizes: '192x192',
            type: 'image/png',
          },
          {
            src: '/icons/icon-512x512.png',
            sizes: '512x512',
            type: 'image/png',
          },
        ],
      },
    }),
  ],
})
```

---

## 🐛 Cross-Platform Debugging

### Universal Debug Panel
```typescript
// src/components/DebugPanel.tsx
import React, { useState, useEffect } from 'react'
import { useAppStore } from '../store/useAppStore'

export function DebugPanel() {
  const [isVisible, setIsVisible] = useState(__DEV__)
  const [logs, setLogs] = useState<string[]>([])
  const { user, isOnline, favorites } = useAppStore()

  useEffect(() => {
    const originalLog = console.log
    const originalError = console.error
    
    console.log = (...args) => {
      setLogs(prev => [...prev.slice(-49), args.join(' ')])
      originalLog(...args)
    }
    
    console.error = (...args) => {
      setLogs(prev => [...prev.slice(-49), `ERROR: ${args.join(' ')}`])
      originalError(...args)
    }

    return () => {
      console.log = originalLog
      console.error = originalError
    }
  }, [])

  if (!isVisible) return null

  return (
    <div className="debug-panel">
      <div className="debug-header">
        <h3>Debug Panel</h3>
        <button onClick={() => setIsVisible(false)}>×</button>
      </div>
      
      <div className="debug-section">
        <h4>App State</h4>
        <div>User: {user?.name || 'Not logged in'}</div>
        <div>Online: {isOnline ? '✅' : '❌'}</div>
        <div>Favorites: {favorites.length}</div>
      </div>
      
      <div className="debug-section">
        <h4>Recent Logs</h4>
        <div className="logs">
          {logs.map((log, index) => (
            <div key={index} className="log-entry">{log}</div>
          ))}
        </div>
      </div>
    </div>
  )
}
```

### Performance Monitoring
```typescript
// src/utils/performance.ts
export class PerformanceMonitor {
  private static metrics = new Map<string, number>()
  
  static startTimer(name: string) {
    this.metrics.set(name, performance.now())
  }
  
  static endTimer(name: string) {
    const startTime = this.metrics.get(name)
    if (startTime) {
      const duration = performance.now() - startTime
      console.log(`⏱️ ${name}: ${duration.toFixed(2)}ms`)
      
      // Send to analytics if duration is concerning
      if (duration > 1000) {
        this.reportSlowOperation(name, duration)
      }
      
      this.metrics.delete(name)
      return duration
    }
    return 0
  }
  
  private static reportSlowOperation(operation: string, duration: number) {
    // Send to your analytics service
    fetch('/api/performance', {
      method: 'POST',
      body: JSON.stringify({
        operation,
        duration,
        userAgent: navigator.userAgent,
        timestamp: Date.now(),
      }),
    }).catch(() => {
      // Silently fail - don't break the app
    })
  }
}

// Usage in components
export function usePerformanceTimer(name: string) {
  useEffect(() => {
    PerformanceMonitor.startTimer(name)
    return () => PerformanceMonitor.endTimer(name)
  }, [name])
}
```

