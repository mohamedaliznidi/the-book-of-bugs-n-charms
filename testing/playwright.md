---
description: >-
  Master the mystical arts of Playwright for end-to-end testing enchantments of modern
  web applications. Learn sacred setup rituals, configuration spells, testing patterns,
  and advanced features for reliable cross-browser divination with Next.js and React realms.

theme: "magic"
---

# The Playwright Testing Sorcery Grimoire

## The Ancient Knowledge

Playwright is a modern end-to-end testing framework that enables reliable mystical testing across Chromium, Firefox, and WebKit browser realms. It provides powerful automation enchantments, built-in waiting mechanisms, and excellent developer experience for testing modern web application manifestations through comprehensive user journey divination.

## When to Cast These Spells

1. **End-to-End Testing Rituals**: Complete user journey testing across multiple mystical pages
2. **Cross-Browser Divination**: Ensure compatibility across different browser realms
3. **Visual Regression Enchantments**: Detect visual changes in your application manifestations
4. **API Testing Sorcery**: Test backend APIs alongside frontend functionality through mystical integration

## Your First Casting

### 1. Install the Playwright Testing Enchantment

```bash
# Install Playwright
pnpm add -D @playwright/test

# Install browsers
pnpm dlx playwright install

# Install system dependencies (Linux)
pnpm dlx playwright install-deps
```

### 2. Initialize the Playwright Mystical Configuration

```bash
# Initialize Playwright configuration enchantment
pnpm dlx playwright init
```

### 3. Playwright Configuration Spells

```typescript
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test'

export default defineConfig({
  // Test directory
  testDir: './e2e',
  
  // Run tests in files in parallel
  fullyParallel: true,
  
  // Fail the build on CI if you accidentally left test.only in the source code
  forbidOnly: !!process.env.CI,
  
  // Retry on CI only
  retries: process.env.CI ? 2 : 0,
  
  // Opt out of parallel tests on CI
  workers: process.env.CI ? 1 : undefined,
  
  // Reporter to use
  reporter: [
    ['html'],
    ['json', { outputFile: 'test-results.json' }],
    ['junit', { outputFile: 'test-results.xml' }],
  ],
  
  // Shared settings for all the projects below
  use: {
    // Base URL to use in actions like `await page.goto('/')`
    baseURL: 'http://localhost:3000',
    
    // Collect trace when retrying the failed test
    trace: 'on-first-retry',
    
    // Record video on failure
    video: 'retain-on-failure',
    
    // Take screenshot on failure
    screenshot: 'only-on-failure',
  },

  // Configure projects for major browsers
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
    {
      name: 'Mobile Chrome',
      use: { ...devices['Pixel 5'] },
    },
    {
      name: 'Mobile Safari',
      use: { ...devices['iPhone 12'] },
    },
  ],

  // Run your local dev server before starting the tests
  webServer: {
    command: 'pnpm dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
})
```

## Advanced Sorcery

### 1. Page Navigation and Interaction Enchantments

```typescript
// e2e/basic-navigation.spec.ts
import { test, expect } from '@playwright/test'

test.describe('Basic Navigation', () => {
  test('should navigate to home page', async ({ page }) => {
    await page.goto('/')
    
    await expect(page).toHaveTitle(/My App/)
    await expect(page.locator('h1')).toContainText('Welcome')
  })

  test('should navigate between pages', async ({ page }) => {
    await page.goto('/')
    
    // Click on navigation link
    await page.click('nav a[href="/about"]')
    
    // Verify navigation
    await expect(page).toHaveURL('/about')
    await expect(page.locator('h1')).toContainText('About Us')
  })

  test('should handle form submission', async ({ page }) => {
    await page.goto('/contact')
    
    // Fill out form
    await page.fill('input[name="name"]', 'John Doe')
    await page.fill('input[name="email"]', 'john@example.com')
    await page.fill('textarea[name="message"]', 'Hello, this is a test message.')
    
    // Submit form
    await page.click('button[type="submit"]')
    
    // Verify success message
    await expect(page.locator('.success-message')).toContainText('Message sent successfully')
  })
})
```

### 2. User Authentication Flow Rituals

```typescript
// e2e/auth.spec.ts
import { test, expect } from '@playwright/test'

test.describe('Authentication', () => {
  test('should login with valid credentials', async ({ page }) => {
    await page.goto('/login')
    
    // Fill login form
    await page.fill('input[name="email"]', 'user@example.com')
    await page.fill('input[name="password"]', 'password123')
    
    // Submit form
    await page.click('button[type="submit"]')
    
    // Verify redirect to dashboard
    await expect(page).toHaveURL('/dashboard')
    await expect(page.locator('[data-testid="user-menu"]')).toBeVisible()
  })

  test('should show error for invalid credentials', async ({ page }) => {
    await page.goto('/login')
    
    await page.fill('input[name="email"]', 'invalid@example.com')
    await page.fill('input[name="password"]', 'wrongpassword')
    
    await page.click('button[type="submit"]')
    
    // Verify error message
    await expect(page.locator('.error-message')).toContainText('Invalid credentials')
    await expect(page).toHaveURL('/login')
  })

  test('should logout successfully', async ({ page }) => {
    // Login first
    await page.goto('/login')
    await page.fill('input[name="email"]', 'user@example.com')
    await page.fill('input[name="password"]', 'password123')
    await page.click('button[type="submit"]')
    
    // Wait for dashboard
    await expect(page).toHaveURL('/dashboard')
    
    // Logout
    await page.click('[data-testid="user-menu"]')
    await page.click('text=Logout')
    
    // Verify redirect to home
    await expect(page).toHaveURL('/')
  })
})
```

### 3. E-commerce Flow Enchantments

```typescript
// e2e/ecommerce.spec.ts
import { test, expect } from '@playwright/test'

test.describe('E-commerce Flow', () => {
  test('should complete purchase flow', async ({ page }) => {
    // Browse products
    await page.goto('/products')
    
    // Select a product
    await page.click('.product-card:first-child')
    await expect(page.locator('h1')).toContainText('Product Name')
    
    // Add to cart
    await page.click('button:has-text("Add to Cart")')
    await expect(page.locator('.cart-notification')).toContainText('Added to cart')
    
    // Go to cart
    await page.click('[data-testid="cart-icon"]')
    await expect(page).toHaveURL('/cart')
    
    // Verify item in cart
    await expect(page.locator('.cart-item')).toBeVisible()
    
    // Proceed to checkout
    await page.click('button:has-text("Checkout")')
    
    // Fill shipping information
    await page.fill('input[name="firstName"]', 'John')
    await page.fill('input[name="lastName"]', 'Doe')
    await page.fill('input[name="address"]', '123 Main St')
    await page.fill('input[name="city"]', 'New York')
    await page.fill('input[name="zipCode"]', '10001')
    
    // Continue to payment
    await page.click('button:has-text("Continue to Payment")')
    
    // Fill payment information (test mode)
    await page.fill('input[name="cardNumber"]', '4242424242424242')
    await page.fill('input[name="expiryDate"]', '12/25')
    await page.fill('input[name="cvv"]', '123')
    
    // Complete purchase
    await page.click('button:has-text("Complete Purchase")')
    
    // Verify success
    await expect(page.locator('.success-message')).toContainText('Order placed successfully')
    await expect(page).toHaveURL(/\/order\/\d+/)
  })
})
```

## Master's Rituals

### 1. API Testing Divination

```typescript
// e2e/api.spec.ts
import { test, expect } from '@playwright/test'

test.describe('API Testing', () => {
  test('should test API endpoints', async ({ request }) => {
    // GET request
    const response = await request.get('/api/users')
    expect(response.ok()).toBeTruthy()
    
    const users = await response.json()
    expect(users).toHaveLength(3)
    expect(users[0]).toHaveProperty('id')
    expect(users[0]).toHaveProperty('name')
  })

  test('should create user via API', async ({ request }) => {
    const newUser = {
      name: 'John Doe',
      email: 'john@example.com',
    }
    
    const response = await request.post('/api/users', {
      data: newUser,
    })
    
    expect(response.ok()).toBeTruthy()
    
    const createdUser = await response.json()
    expect(createdUser).toMatchObject(newUser)
    expect(createdUser).toHaveProperty('id')
  })

  test('should handle API errors', async ({ request }) => {
    const response = await request.get('/api/users/999')
    expect(response.status()).toBe(404)
    
    const error = await response.json()
    expect(error).toHaveProperty('message', 'User not found')
  })
})
```

### 2. Visual Testing Enchantments

```typescript
// e2e/visual.spec.ts
import { test, expect } from '@playwright/test'

test.describe('Visual Testing', () => {
  test('should match homepage screenshot', async ({ page }) => {
    await page.goto('/')
    
    // Wait for page to be fully loaded
    await page.waitForLoadState('networkidle')
    
    // Take screenshot and compare
    await expect(page).toHaveScreenshot('homepage.png')
  })

  test('should match component screenshots', async ({ page }) => {
    await page.goto('/components')
    
    // Screenshot specific component
    const button = page.locator('.primary-button')
    await expect(button).toHaveScreenshot('primary-button.png')
    
    // Screenshot with different states
    await button.hover()
    await expect(button).toHaveScreenshot('primary-button-hover.png')
  })

  test('should test responsive design', async ({ page }) => {
    await page.goto('/')
    
    // Desktop view
    await page.setViewportSize({ width: 1200, height: 800 })
    await expect(page).toHaveScreenshot('homepage-desktop.png')
    
    // Tablet view
    await page.setViewportSize({ width: 768, height: 1024 })
    await expect(page).toHaveScreenshot('homepage-tablet.png')
    
    // Mobile view
    await page.setViewportSize({ width: 375, height: 667 })
    await expect(page).toHaveScreenshot('homepage-mobile.png')
  })
})
```

### 3. Performance Testing Sorcery

```typescript
// e2e/performance.spec.ts
import { test, expect } from '@playwright/test'

test.describe('Performance Testing', () => {
  test('should meet performance metrics', async ({ page }) => {
    // Start tracing
    await page.tracing.start({ screenshots: true, snapshots: true })
    
    await page.goto('/')
    
    // Wait for page to be fully loaded
    await page.waitForLoadState('networkidle')
    
    // Stop tracing
    await page.tracing.stop({ path: 'trace.zip' })
    
    // Measure performance
    const performanceMetrics = await page.evaluate(() => {
      const navigation = performance.getEntriesByType('navigation')[0] as PerformanceNavigationTiming
      return {
        domContentLoaded: navigation.domContentLoadedEventEnd - navigation.fetchStart,
        loadComplete: navigation.loadEventEnd - navigation.fetchStart,
        firstPaint: performance.getEntriesByName('first-paint')[0]?.startTime || 0,
        firstContentfulPaint: performance.getEntriesByName('first-contentful-paint')[0]?.startTime || 0,
      }
    })
    
    // Assert performance thresholds
    expect(performanceMetrics.domContentLoaded).toBeLessThan(2000) // 2 seconds
    expect(performanceMetrics.loadComplete).toBeLessThan(3000) // 3 seconds
    expect(performanceMetrics.firstContentfulPaint).toBeLessThan(1500) // 1.5 seconds
  })

  test('should handle large datasets efficiently', async ({ page }) => {
    await page.goto('/large-table')
    
    const startTime = Date.now()
    
    // Wait for table to render
    await page.waitForSelector('table tbody tr:nth-child(100)')
    
    const renderTime = Date.now() - startTime
    
    // Assert render time is reasonable
    expect(renderTime).toBeLessThan(5000) // 5 seconds
    
    // Test scrolling performance
    const scrollStartTime = Date.now()
    await page.evaluate(() => {
      window.scrollTo(0, document.body.scrollHeight)
    })
    await page.waitForTimeout(100) // Allow scroll to complete
    
    const scrollTime = Date.now() - scrollStartTime
    expect(scrollTime).toBeLessThan(1000) // 1 second
  })
})
```

## Mystical Test Organization

### 1. Page Object Model Enchantments

```typescript
// e2e/pages/LoginPage.ts
import { Page, Locator } from '@playwright/test'

export class LoginPage {
  readonly page: Page
  readonly emailInput: Locator
  readonly passwordInput: Locator
  readonly loginButton: Locator
  readonly errorMessage: Locator

  constructor(page: Page) {
    this.page = page
    this.emailInput = page.locator('input[name="email"]')
    this.passwordInput = page.locator('input[name="password"]')
    this.loginButton = page.locator('button[type="submit"]')
    this.errorMessage = page.locator('.error-message')
  }

  async goto() {
    await this.page.goto('/login')
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email)
    await this.passwordInput.fill(password)
    await this.loginButton.click()
  }

  async getErrorMessage() {
    return await this.errorMessage.textContent()
  }
}
```

```typescript
// e2e/auth-with-pom.spec.ts
import { test, expect } from '@playwright/test'
import { LoginPage } from './pages/LoginPage'

test.describe('Authentication with POM', () => {
  test('should login successfully', async ({ page }) => {
    const loginPage = new LoginPage(page)
    
    await loginPage.goto()
    await loginPage.login('user@example.com', 'password123')
    
    await expect(page).toHaveURL('/dashboard')
  })

  test('should show error for invalid credentials', async ({ page }) => {
    const loginPage = new LoginPage(page)
    
    await loginPage.goto()
    await loginPage.login('invalid@example.com', 'wrongpassword')
    
    const errorMessage = await loginPage.getErrorMessage()
    expect(errorMessage).toContain('Invalid credentials')
  })
})
```

### 2. Fixtures and Hooks Rituals

```typescript
// e2e/fixtures.ts
import { test as base } from '@playwright/test'
import { LoginPage } from './pages/LoginPage'

type MyFixtures = {
  loginPage: LoginPage
  authenticatedPage: Page
}

export const test = base.extend<MyFixtures>({
  loginPage: async ({ page }, use) => {
    const loginPage = new LoginPage(page)
    await use(loginPage)
  },

  authenticatedPage: async ({ page }, use) => {
    // Login before each test that uses this fixture
    await page.goto('/login')
    await page.fill('input[name="email"]', 'user@example.com')
    await page.fill('input[name="password"]', 'password123')
    await page.click('button[type="submit"]')
    await page.waitForURL('/dashboard')
    
    await use(page)
  },
})

export { expect } from '@playwright/test'
```

```typescript
// e2e/dashboard.spec.ts
import { test, expect } from './fixtures'

test.describe('Dashboard', () => {
  test('should display user dashboard', async ({ authenticatedPage }) => {
    // Page is already authenticated
    await expect(authenticatedPage.locator('h1')).toContainText('Dashboard')
    await expect(authenticatedPage.locator('[data-testid="user-menu"]')).toBeVisible()
  })
})
```

## Wisdom of the Ancients

### 1. Environment Configuration Spells

```typescript
// playwright.config.ts
import { defineConfig } from '@playwright/test'

export default defineConfig({
  use: {
    baseURL: process.env.BASE_URL || 'http://localhost:3000',
    
    // Use different credentials for different environments
    extraHTTPHeaders: {
      'Authorization': process.env.API_TOKEN ? `Bearer ${process.env.API_TOKEN}` : '',
    },
  },

  projects: [
    {
      name: 'development',
      use: {
        baseURL: 'http://localhost:3000',
      },
    },
    {
      name: 'staging',
      use: {
        baseURL: 'https://staging.example.com',
      },
    },
    {
      name: 'production',
      use: {
        baseURL: 'https://example.com',
      },
    },
  ],
})
```

### 2. CI/CD Integration Enchantments

```yaml
# .github/workflows/playwright.yml
name: Playwright Tests
on:
  push:
    branches: [ main, master ]
  pull_request:
    branches: [ main, master ]

jobs:
  test:
    timeout-minutes: 60
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-node@v3
      with:
        node-version: 18
    - name: Install dependencies
      run: npm ci
    - name: Install Playwright Browsers
      run: npx playwright install --with-deps
    - name: Run Playwright tests
      run: npx playwright test
    - uses: actions/upload-artifact@v3
      if: always()
      with:
        name: playwright-report
        path: playwright-report/
        retention-days: 30
```

### Sacred Testing Principles

- **Reliable Selectors**: Use data-testid attributes for stable element selection in your mystical tests
- **Wait Strategies**: Use Playwright's built-in waiting mechanisms instead of fixed timeouts for reliable enchantments
- **Page Object Model**: Organize tests with reusable page objects for maintainability and mystical clarity
- **Parallel Execution**: Run tests in parallel for faster feedback and efficient testing rituals
- **Visual Testing**: Use screenshot comparison for visual regression testing enchantments
- **Performance Monitoring**: Include performance assertions in critical user journey divination

## Common Curses & Their Remedies

### Curse 1: The Flaky Test Timing Hex
Tests failing intermittently due to mystical timing issues that disrupt the testing enchantments.

**Counter-Spell:**
Use Playwright's built-in waiting mechanisms for reliable test sorcery:
```typescript
// ❌ Wrong - fixed timeout
await page.waitForTimeout(5000)

// ✅ Correct - wait for specific condition
await page.waitForSelector('.loading-spinner', { state: 'hidden' })
await expect(page.locator('.content')).toBeVisible()
```

### Curse 2: The Brittle Selector Curse
Tests breaking when UI changes due to fragile selectors that cannot withstand interface transformations.

**Counter-Spell:**
Use stable, semantic selectors for resilient testing enchantments:
```typescript
// ❌ Wrong - fragile CSS selector
await page.click('.btn.btn-primary.mt-4')

// ✅ Better - data-testid
await page.click('[data-testid="submit-button"]')

// ✅ Best - semantic selector
await page.click('button:has-text("Submit")')
```

### Curse 3: The Test Isolation Contamination
Tests affecting each other due to shared mystical state, causing unpredictable test behavior.

**Counter-Spell:**
Ensure proper test isolation through cleansing rituals:
```typescript
test.beforeEach(async ({ page }) => {
  // Clear cookies and local storage
  await page.context().clearCookies()
  await page.evaluate(() => localStorage.clear())
})
```

## Sacred Texts & Mystical Sources

{% embed url="https://playwright.dev/" %}

{% embed url="https://playwright.dev/docs/test-configuration" %}

{% embed url="https://playwright.dev/docs/best-practices" %}
