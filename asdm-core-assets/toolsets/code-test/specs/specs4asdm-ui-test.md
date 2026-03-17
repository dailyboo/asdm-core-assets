# Specifications for ASDM UI Test

## Purpose
This document provides detailed specifications for the ASDM UI Test action, defining test execution strategies, element interactions, and reporting standards for web UI testing.

## Architecture

### UI Testing Framework
```
┌─────────────────────────────────────────────────────────┐
│                    UI Test Runner                        │
├─────────────────────────────────────────────────────────┤
│  Browser Manager    │  Page Controller   │  Executor    │
├─────────────────────────────────────────────────────────┤
│  Element Locator    │  Action Handler    │  Validator   │
├─────────────────────────────────────────────────────────┤
│  Screenshot Capture │  Video Recorder    │  Reporter    │
└─────────────────────────────────────────────────────────┘
```

## Functional Requirements

### Browser Support

#### Desktop Browsers
| Browser | Versions | Headless | Mobile Emulation |
|---------|----------|----------|------------------|
| Chrome | Latest 3 | Yes | Yes |
| Firefox | Latest 3 | Yes | Yes |
| Safari | Latest 2 | No | No |
| Edge | Latest 3 | Yes | Yes |

#### Mobile Devices
```json
{
  "devices": [
    {"name": "iPhone 13", "viewport": "390x844", "userAgent": "iPhone"},
    {"name": "iPhone 13 Pro Max", "viewport": "428x926", "userAgent": "iPhone"},
    {"name": "Samsung Galaxy S21", "viewport": "360x800", "userAgent": "Android"},
    {"name": "iPad Pro", "viewport": "1024x1366", "userAgent": "iPad"},
    {"name": "Pixel 5", "viewport": "393x851", "userAgent": "Android"}
  ]
}
```

### Test Structure

#### Playwright Style
```javascript
import { test, expect, devices } from '@playwright/test';

test.describe('User Authentication @smoke @critical', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('should login with valid credentials', async ({ page }) => {
    // Arrange
    const credentials = {
      email: 'user@example.com',
      password: 'SecurePassword123!'
    };

    // Act
    await page.getByLabel('Email').fill(credentials.email);
    await page.getByLabel('Password').fill(credentials.password);
    await page.getByRole('button', { name: 'Sign In' }).click();

    // Assert
    await expect(page).toHaveURL('/dashboard');
    await expect(page.getByText('Welcome')).toBeVisible();
  });

  test('should show error for invalid credentials', async ({ page }) => {
    await page.getByLabel('Email').fill('invalid@example.com');
    await page.getByLabel('Password').fill('wrongpassword');
    await page.getByRole('button', { name: 'Sign In' }).click();
    
    await expect(page.getByRole('alert')).toContainText('Invalid credentials');
  });
});
```

#### Cypress Style
```javascript
describe('User Authentication', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('should login with valid credentials', () => {
    cy.get('[data-testid="email-input"]').type('user@example.com');
    cy.get('[data-testid="password-input"]').type('SecurePassword123!');
    cy.get('[data-testid="login-button"]').click();
    
    cy.url().should('include', '/dashboard');
    cy.contains('Welcome').should('be.visible');
  });
});
```

### Element Location Strategies

#### 1. Role-Based Locators (Preferred)
```javascript
// Buttons
page.getByRole('button', { name: 'Submit' })
page.getByRole('link', { name: 'Learn More' })

// Form elements
page.getByRole('textbox', { name: 'Email' })
page.getByRole('checkbox', { name: 'Remember me' })
page.getByRole('combobox', { name: 'Country' })

// Navigation
page.getByRole('navigation')
page.getByRole('menuitem', { name: 'Settings' })
```

#### 2. Label-Based Locators
```javascript
page.getByLabel('Email Address')
page.getByLabel('Password')
page.getByPlaceholder('Enter your email')
```

#### 3. Test ID Locators
```javascript
page.getByTestId('submit-button')
page.getByTestId('user-profile-card')
```

#### 4. Text Content Locators
```javascript
page.getByText('Welcome, User!')
page.getByText(/Hello, \w+/)
```

#### 5. CSS Selectors (Fallback)
```javascript
page.locator('.btn-primary')
page.locator('#user-menu')
page.locator('[data-custom="value"]')
```

### Interactions

#### Basic Actions
```javascript
// Click
await page.click('button');
await page.dblclick('.item');
await page.rightclick('.context-menu-trigger');

// Fill
await page.fill('input[name="email"]', 'user@example.com');
await page.type('input[name="search"]', 'query', { delay: 100 });

// Select
await page.selectOption('select[name="country"]', 'US');
await page.selectOption('select', { label: 'United States' });

// Check
await page.check('input[type="checkbox"]');
await page.uncheck('input[type="checkbox"]');

// File Upload
await page.setInputFiles('input[type="file"]', 'test-file.pdf');
```

#### Advanced Interactions
```javascript
// Drag and Drop
await page.dragAndDrop('.source', '.target');

// Hover
await page.hover('.menu-item');

// Keyboard
await page.keyboard.press('Enter');
await page.keyboard.type('Hello World');

// Mouse
await page.mouse.click(100, 200);
await page.mouse.move(100, 200);
```

#### Frame Handling
```javascript
// iframe
const frame = page.frameLocator('.iframe');
await frame.getByRole('button').click();

// Multiple frames
for (const frame of page.frames()) {
  console.log(frame.url());
}
```

### Assertions

#### Visibility Assertions
```javascript
await expect(page.locator('.element')).toBeVisible();
await expect(page.locator('.element')).toBeHidden();
await expect(page.locator('.element')).not.toBeVisible();
```

#### Text Assertions
```javascript
await expect(page.locator('.message')).toHaveText('Success');
await expect(page.locator('.message')).toContainText('Success');
await expect(page.locator('.message')).toHaveText(/Success \d+/);
```

#### Value Assertions
```javascript
await expect(page.locator('input')).toHaveValue('user@example.com');
await expect(page.locator('input')).toBeEmpty();
```

#### State Assertions
```javascript
await expect(page.locator('input')).toBeEnabled();
await expect(page.locator('input')).toBeDisabled();
await expect(page.locator('checkbox')).toBeChecked();
await expect(page.locator('input')).toBeEditable();
await expect(page.locator('input')).toBeFocused();
```

#### Attribute Assertions
```javascript
await expect(page.locator('a')).toHaveAttribute('href', '/about');
await expect(page.locator('input')).toHaveClass('form-control');
await expect(page.locator('img')).toHaveCSS('width', '100px');
```

#### URL Assertions
```javascript
await expect(page).toHaveURL('/dashboard');
await expect(page).toHaveURL(/dashboard/);
await expect(page).toHaveTitle('Dashboard | App');
```

### Visual Testing

#### Screenshot Comparison
```javascript
// Full page screenshot
await expect(page).toHaveScreenshot('homepage.png');

// Element screenshot
await expect(page.locator('.card')).toHaveScreenshot('card.png');

// With options
await expect(page).toHaveScreenshot('homepage.png', {
  fullPage: true,
  animations: 'disabled',
  threshold: 0.1
});
```

#### Viewport Testing
```javascript
test('responsive layout', async ({ page }) => {
  const viewports = [
    { width: 1920, height: 1080 },
    { width: 1366, height: 768 },
    { width: 375, height: 667 }
  ];

  for (const viewport of viewports) {
    await page.setViewportSize(viewport);
    await expect(page).toHaveScreenshot(`homepage-${viewport.width}x${viewport.height}.png`);
  }
});
```

### Accessibility Testing

```javascript
test('should be accessible', async ({ page }) => {
  await page.goto('/');
  
  const accessibilityScanResults = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa'])
    .analyze();
  
  expect(accessibilityScanResults.violations).toEqual([]);
});
```

### Network Handling

#### Intercepting Requests
```javascript
// Mock API response
await page.route('**/api/users', route => {
  route.fulfill({
    status: 200,
    contentType: 'application/json',
    body: JSON.stringify({ users: [] })
  });
});

// Block specific requests
await page.route('**/*.{png,jpg,jpeg}', route => route.abort());

// Modify requests
await page.route('**/api/**', route => {
  const headers = route.request().headers();
  headers['X-Custom-Header'] = 'value';
  route.continue({ headers });
});
```

#### Waiting for Network
```javascript
// Wait for specific request
const responsePromise = page.waitForResponse('**/api/users');
await page.click('button');
const response = await responsePromise;

// Wait for all network idle
await page.waitForLoadState('networkidle');
```

## Non-Functional Requirements

### Performance
- Test initialization: < 5 seconds
- Page load wait: configurable timeout
- Element wait: smart auto-waiting
- Screenshot capture: < 1 second

### Reliability
- Auto-retry for flaky operations
- Built-in waiting mechanisms
- Graceful failure handling
- Detailed error messages

### Security
- Secure credential storage
- Isolated test contexts
- Clean browser state
- No data leakage between tests

## Test Configuration

### Playwright Configuration
```javascript
// playwright.config.js
module.exports = {
  testDir: './tests',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: 'html',
  
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },

  projects: [
    {
      name: 'chromium',
      use: { browserName: 'chromium' },
    },
    {
      name: 'firefox',
      use: { browserName: 'firefox' },
    },
    {
      name: 'webkit',
      use: { browserName: 'webkit' },
    },
  ],

  webServer: {
    command: 'npm run start',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
};
```

### Cypress Configuration
```javascript
// cypress.config.js
module.exports = {
  e2e: {
    baseUrl: 'http://localhost:3000',
    viewportWidth: 1280,
    viewportHeight: 720,
    video: true,
    screenshotOnRunFailure: true,
    retries: {
      runMode: 2,
      openMode: 0,
    },
  },
};
```

## Output Specifications

### Test Result Format
```json
{
  "status": "completed",
  "config": {
    "browser": "chromium",
    "headless": true,
    "viewport": {"width": 1280, "height": 720}
  },
  "summary": {
    "total": 15,
    "passed": 14,
    "failed": 1,
    "skipped": 0,
    "duration": "45.2s"
  },
  "suites": [
    {
      "name": "Authentication",
      "tests": [
        {
          "name": "should login successfully",
          "status": "passed",
          "duration": "3.2s",
          "steps": [
            {"action": "goto", "url": "/login", "duration": "1.2s"},
            {"action": "fill", "selector": "[name='email']", "duration": "0.1s"},
            {"action": "fill", "selector": "[name='password']", "duration": "0.1s"},
            {"action": "click", "selector": "button[type='submit']", "duration": "0.1s"},
            {"action": "waitForURL", "url": "/dashboard", "duration": "1.5s"}
          ]
        }
      ]
    }
  ],
  "artifacts": {
    "screenshots": ["screenshot-1.png"],
    "videos": ["video-1.webm"],
    "traces": ["trace-1.zip"]
  }
}
```

## Page Object Model

### Page Object Structure
```javascript
// pages/LoginPage.js
class LoginPage {
  constructor(page) {
    this.page = page;
    this.emailInput = page.getByLabel('Email');
    this.passwordInput = page.getByLabel('Password');
    this.submitButton = page.getByRole('button', { name: 'Sign In' });
    this.errorMessage = page.getByRole('alert');
  }

  async goto() {
    await this.page.goto('/login');
  }

  async login(email, password) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.submitButton.click();
  }

  async getErrorMessage() {
    return await this.errorMessage.textContent();
  }
}

module.exports = { LoginPage };
```

### Test Using Page Object
```javascript
const { LoginPage } = require('./pages/LoginPage');

test('should login', async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.goto();
  await loginPage.login('user@example.com', 'password');
  await expect(page).toHaveURL('/dashboard');
});
```

## Testing Requirements

### Unit Tests
- Test page objects
- Test utility functions
- Test assertion helpers

### Integration Tests
- Test across browsers
- Test responsive design
- Test accessibility
- Test visual regression

## Constraints
- Maximum test duration: 5 minutes per test
- Maximum parallel workers: CPU cores
- Maximum screenshot size: 10MB
- Maximum video size: 100MB
