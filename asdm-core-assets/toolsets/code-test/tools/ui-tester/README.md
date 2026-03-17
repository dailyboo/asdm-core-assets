# UI Tester

UI testing automation framework for web applications with multi-browser support, visual regression testing, and accessibility validation.

## Overview

The UI Tester tool provides comprehensive web UI testing capabilities, including end-to-end testing, visual regression, cross-browser compatibility, and accessibility compliance.

## Features

- **Multi-Browser Support**: Chrome, Firefox, Safari, Edge
- **Visual Regression**: Screenshot comparison and diff detection
- **Accessibility Testing**: WCAG compliance validation
- **Mobile Testing**: Device emulation and responsive testing
- **Parallel Execution**: Concurrent test runs
- **Auto-Waiting**: Smart element waiting mechanisms
- **Recording**: Video and trace capture
- **Page Object Model**: Maintainable test structure

## Installation

```bash
# Install dependencies
npm install

# Install browsers (Playwright)
npx playwright install
```

## Usage

### Command Line

```bash
# Run UI tests
node main.js run --suite tests/e2e/

# Run on specific browser
node main.js run --suite tests/e2e/ --browser firefox

# Run in headed mode
node main.js run --suite tests/e2e/ --headless false

# Mobile testing
node main.js run --suite tests/mobile/ --device "iPhone 13"

# Debug mode
node main.js debug --suite tests/e2e/login.spec.js

# Generate test code
node main.js codegen --url https://example.com --output test.spec.js

# Visual regression
node main.js run --suite tests/visual/ --screenshot on
```

### Parameters

| Parameter | Short | Description | Required | Default |
|-----------|-------|-------------|----------|---------|
| `--suite` | `-s` | Test suite directory | Yes | - |
| `--browser` | `-b` | Browser to use | No | chromium |
| `--headless` | | Run headless | No | true |
| `--viewport` | | Viewport size | No | 1280x720 |
| `--device` | | Mobile device | No | - |
| `--parallel` | | Parallel workers | No | 1 |
| `--retry` | | Retry count | No | 0 |
| `--video` | | Record video | No | off |
| `--trace` | | Capture trace | No | off |
| `--screenshot` | | Screenshot mode | No | only-on-failure |
| `--report` | | Report path | No | - |
| `--base-url` | | Base URL | No | - |

## Configuration

### config.json

```json
{
  "version": "1.0.0",
  "browsers": {
    "chromium": {
      "enabled": true,
      "headless": true,
      "viewport": {"width": 1280, "height": 720}
    },
    "firefox": {
      "enabled": true,
      "headless": true
    },
    "webkit": {
      "enabled": true,
      "headless": false
    }
  },
  "devices": [
    {"name": "iPhone 13", "viewport": {"width": 390, "height": 844}},
    {"name": "iPad Pro", "viewport": {"width": 1024, "height": 1366}}
  ],
  "testDir": "./tests",
  "timeout": 30000,
  "expect": {
    "timeout": 5000
  },
  "retries": 0,
  "workers": 1,
  "reporter": "html",
  "use": {
    "trace": "on-first-retry",
    "screenshot": "only-on-failure",
    "video": "retain-on-failure"
  }
}
```

## Test Structure

### Playwright Style

```javascript
import { test, expect } from '@playwright/test';

test.describe('User Authentication', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('should login successfully', async ({ page }) => {
    // Arrange
    const email = 'user@example.com';
    const password = 'SecurePassword123!';

    // Act
    await page.getByLabel('Email').fill(email);
    await page.getByLabel('Password').fill(password);
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

### Cypress Style

```javascript
describe('User Authentication', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('should login successfully', () => {
    cy.get('[data-testid="email-input"]').type('user@example.com');
    cy.get('[data-testid="password-input"]').type('SecurePassword123!');
    cy.get('[data-testid="login-button"]').click();

    cy.url().should('include', '/dashboard');
    cy.contains('Welcome').should('be.visible');
  });
});
```

## Locator Strategies

### Role-Based (Preferred)

```javascript
page.getByRole('button', { name: 'Submit' })
page.getByRole('link', { name: 'Learn More' })
page.getByRole('textbox', { name: 'Email' })
page.getByRole('checkbox', { name: 'Remember me' })
```

### Label-Based

```javascript
page.getByLabel('Email Address')
page.getByPlaceholder('Enter your email')
```

### Test ID

```javascript
page.getByTestId('submit-button')
page.getByTestId('user-profile-card')
```

### Text Content

```javascript
page.getByText('Welcome, User!')
page.getByText(/Hello, \w+/)
```

## Actions

### Basic Interactions

```javascript
// Click
await page.click('button');
await page.dblclick('.item');
await page.rightclick('.context-menu');

// Fill
await page.fill('[name="email"]', 'user@example.com');
await page.type('[name="search"]', 'query', { delay: 100 });

// Select
await page.selectOption('select[name="country"]', 'US');

// Check
await page.check('input[type="checkbox"]');
await page.uncheck('input[type="checkbox"]');

// File Upload
await page.setInputFiles('input[type="file"]', 'test-file.pdf');
```

### Advanced Interactions

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
```

## Assertions

### Visibility

```javascript
await expect(page.locator('.element')).toBeVisible();
await expect(page.locator('.element')).toBeHidden();
```

### Text Content

```javascript
await expect(page.locator('.message')).toHaveText('Success');
await expect(page.locator('.message')).toContainText('Success');
```

### Value

```javascript
await expect(page.locator('input')).toHaveValue('user@example.com');
await expect(page.locator('input')).toBeEmpty();
```

### State

```javascript
await expect(page.locator('input')).toBeEnabled();
await expect(page.locator('input')).toBeDisabled();
await expect(page.locator('checkbox')).toBeChecked();
```

### URL

```javascript
await expect(page).toHaveURL('/dashboard');
await expect(page).toHaveTitle('Dashboard');
```

## Visual Regression

### Screenshot Comparison

```javascript
// Full page
await expect(page).toHaveScreenshot('homepage.png');

// Element
await expect(page.locator('.card')).toHaveScreenshot('card.png');

// With options
await expect(page).toHaveScreenshot('homepage.png', {
  fullPage: true,
  threshold: 0.1
});
```

### Responsive Testing

```javascript
const viewports = [
  { width: 1920, height: 1080 },
  { width: 1366, height: 768 },
  { width: 375, height: 667 }
];

for (const viewport of viewports) {
  await page.setViewportSize(viewport);
  await expect(page).toHaveScreenshot(`homepage-${viewport.width}x${viewport.height}.png`);
}
```

## Accessibility Testing

```javascript
import AxeBuilder from '@axe-core/playwright';

test('should be accessible', async ({ page }) => {
  await page.goto('/');

  const results = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa'])
    .analyze();

  expect(results.violations).toEqual([]);
});
```

## Network Interception

### Mock Responses

```javascript
await page.route('**/api/users', route => {
  route.fulfill({
    status: 200,
    contentType: 'application/json',
    body: JSON.stringify({ users: [] })
  });
});
```

### Block Requests

```javascript
await page.route('**/*.{png,jpg,jpeg}', route => route.abort());
```

## Page Object Model

### Page Object

```javascript
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

### Usage

```javascript
const { LoginPage } = require('./pages/LoginPage');

test('should login', async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.goto();
  await loginPage.login('user@example.com', 'password');
  await expect(page).toHaveURL('/dashboard');
});
```

## Output Format

### Test Result

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
    "duration": "45.2s"
  },
  "suites": [
    {
      "name": "Authentication",
      "tests": [
        {
          "name": "should login successfully",
          "status": "passed",
          "duration": "3.2s"
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

## Error Handling

```json
{
  "status": "error",
  "error_code": "ELEMENT_NOT_FOUND",
  "message": "Element not found",
  "details": {
    "selector": ".non-existent-element",
    "timeout": 30000
  },
  "screenshot": "error-screenshot.png"
}
```

## Examples

### Run all tests

```bash
node main.js run --suite tests/
```

### Run with video recording

```bash
node main.js run --suite tests/e2e/ --video on
```

### Debug specific test

```bash
node main.js debug --suite tests/login.spec.js
```

## CI/CD Integration

### GitHub Actions

```yaml
- name: Run UI Tests
  run: node main.js run --suite tests/e2e/
  artifacts:
    paths:
      - screenshots/
      - videos/
```

## Testing

```bash
# Run unit tests
npm test

# Run integration tests
npm run test:integration
```

## Best Practices

1. Use stable locators (data-testid)
2. Implement auto-waiting
3. Reset state between tests
4. Use Page Object Model
5. Run in parallel when possible
6. Capture videos on failure
7. Keep tests independent
