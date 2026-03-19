# ASDM UI Test

## Description
Execute automated UI tests for web applications including end-to-end workflows, visual regression testing, and accessibility validation.

## Usage
```
/asdm-test-ui [command] [options]
```

## Commands
- `run`: Execute UI test suite
- `record`: Record user interactions for test creation
- `screenshot`: Capture screenshots for visual testing
- `debug`: Interactive debugging mode
- `codegen`: Generate test code from interactions

## Parameters

### Required Parameters
- `--suite`, `-s`: Test suite file or directory

### Optional Parameters
- `--browser`: Browser to test (chrome, firefox, safari, edge)
- `--headless`: Run in headless mode (default: true)
- `--viewport`: Viewport size (e.g., 1920x1080)
- `--device`: Emulate mobile device
- `--slow-mo`: Slow down execution by milliseconds
- `--video`: Record video of test execution
- `--trace`: Capture trace for debugging
- `--screenshot`: Screenshot on failure
- `--parallel`: Number of parallel browsers
- `--retry`: Retry failed tests
- `--grep`: Filter tests by pattern
- `--report`: Test report output path
- `--base-url`: Base URL for tests
- `--auth`: Authentication credentials

## Examples

### Run UI tests
```
/asdm-test-ui run --suite tests/e2e/
```

### Run on specific browser
```
/asdm-test-ui run --suite tests/e2e/ --browser firefox --headless false
```

### Mobile device testing
```
/asdm-test-ui run --suite tests/mobile/ --device "iPhone 13"
```

### Record test interactions
```
/asdm-test-ui record --url https://example.com --output test.spec.js
```

### Generate test code
```
/asdm-test-ui codegen --url https://example.com --framework playwright
```

### Visual regression testing
```
/asdm-test-ui run --suite tests/visual/ --screenshot on --update-snapshots
```

### Debug mode
```
/asdm-test-ui debug --suite tests/e2e/login.spec.js
```

## Test Suite Structure

### Playwright Style
```javascript
import { test, expect } from '@playwright/test';

test.describe('User Authentication', () => {
  test('should login successfully', async ({ page }) => {
    await page.goto('/login');
    await page.fill('[name="email"]', 'user@example.com');
    await page.fill('[name="password"]', 'password123');
    await page.click('button[type="submit"]');
    
    await expect(page).toHaveURL('/dashboard');
    await expect(page.locator('.welcome-message')).toBeVisible();
  });
});
```

### Cypress Style
```javascript
describe('User Authentication', () => {
  it('should login successfully', () => {
    cy.visit('/login');
    cy.get('[name="email"]').type('user@example.com');
    cy.get('[name="password"]').type('password123');
    cy.get('button[type="submit"]').click();
    
    cy.url().should('include', '/dashboard');
    cy.get('.welcome-message').should('be.visible');
  });
});
```

## Test Types

### 1. End-to-End Tests
Complete user workflows:
- User registration and login
- Shopping cart operations
- Form submissions
- Multi-page navigation

### 2. Visual Regression Tests
UI consistency validation:
- Screenshot comparison
- Layout verification
- Responsive design testing
- Cross-browser screenshots

### 3. Accessibility Tests
WCAG compliance:
- Keyboard navigation
- Screen reader compatibility
- Color contrast
- ARIA attributes

### 4. Performance Tests
Frontend performance:
- Page load time
- First contentful paint
- Time to interactive
- Network waterfall

## Locator Strategies

### CSS Selectors
```javascript
page.locator('.class-name')
page.locator('#element-id')
page.locator('[data-testid="submit-btn"]')
```

### Text Content
```javascript
page.getByText('Welcome')
page.getByRole('button', { name: 'Submit' })
```

### Accessibility Locators
```javascript
page.getByRole('button')
page.getByLabel('Email')
page.getByPlaceholder('Enter password')
```

## Page Object Model

```javascript
class LoginPage {
  constructor(page) {
    this.page = page;
    this.emailInput = page.locator('[name="email"]');
    this.passwordInput = page.locator('[name="password"]');
    this.submitButton = page.locator('button[type="submit"]');
  }
  
  async login(email, password) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.submitButton.click();
  }
}
```

## Visual Testing Configuration

```json
{
  "visual": {
    "baselineDir": "./screenshots/baseline",
    "actualDir": "./screenshots/actual",
    "diffDir": "./screenshots/diff",
    "threshold": 0.1,
    "viewports": [
      {"width": 1920, "height": 1080},
      {"width": 1366, "height": 768},
      {"width": 375, "height": 667}
    ]
  }
}
```

## Mobile Testing

### Device Emulation
```javascript
const iPhone = devices['iPhone 13'];
test.use({
  ...iPhone,
  hasTouch: true
});
```

### Responsive Testing
```
/asdm-test-ui run --suite tests/responsive/ --viewport 375x667,768x1024,1920x1080
```

## Related Specifications
See [specs4asdm-test-ui.md](../specs/specs4asdm-test-ui.md) for detailed specifications.

## Output Format

### Test Result
```json
{
  "status": "completed",
  "summary": {
    "total": 15,
    "passed": 14,
    "failed": 1,
    "skipped": 0,
    "duration": "45.2s"
  },
  "browser": "chrome",
  "results": [
    {
      "name": "should login successfully",
      "status": "passed",
      "duration": "3.2s",
      "screenshots": ["login-1.png"]
    },
    {
      "name": "should display user profile",
      "status": "failed",
      "error": "Element not found: .profile-card",
      "screenshot": "profile-failure.png"
    }
  ]
}
```

## Best Practices

1. **Use Stable Locators**: Prefer data-testid attributes
2. **Wait Strategies**: Use auto-waiting instead of explicit waits
3. **Isolation**: Reset state between tests
4. **Page Objects**: Organize code with Page Object Model
5. **Parallel Execution**: Run independent tests in parallel
6. **Video on Failure**: Record videos for debugging
7. **Screenshot Comparison**: Update baselines intentionally

## Debugging Features

### Trace Viewer
```
/asdm-test-ui run --suite tests/ --trace on
```
Opens interactive trace viewer showing:
- Action timeline
- DOM snapshots
- Network requests
- Console logs

### Step-through Debug
```
/asdm-test-ui debug --suite tests/login.spec.js
```

### Inspector Mode
```
/asdm-test-ui codegen --url https://example.com
```

## CI/CD Integration

### GitHub Actions
```yaml
- name: Run UI Tests
  run: /asdm-test-ui run --suite tests/e2e/ --report report.html
  artifacts:
    paths:
      - screenshots/
      - videos/
```

### GitLab CI
```yaml
ui_test:
  image: mcr.microsoft.com/playwright
  script:
    - /asdm-test-ui run --suite tests/e2e/
```
