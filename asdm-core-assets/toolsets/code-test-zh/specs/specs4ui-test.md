# UI 测试规范

## 目的
本文档提供 UI 测试动作的详细规范，定义 Web UI 测试的测试执行策略、元素交互和报告标准。

## 架构

### UI 测试框架
```
┌─────────────────────────────────────────────────────────┐
│                    UI 测试运行器                          │
├─────────────────────────────────────────────────────────┤
│  浏览器管理器      │  页面控制器   │  执行器      │
├─────────────────────────────────────────────────────────┤
│  元素定位器        │  动作处理器    │  验证器       │
├─────────────────────────────────────────────────────────┤
│  截图捕获 │  视频录制器    │  报告器     │
└─────────────────────────────────────────────────────────┘
```

## 功能需求

### 浏览器支持

#### 桌面浏览器
| 浏览器 | 版本 | 无头模式 | 移动模拟 |
|---------|----------|----------|------------------|
| Chrome | 最新 3 个 | 是 | 是 |
| Firefox | 最新 3 个 | 是 | 是 |
| Safari | 最新 2 个 | 否 | 否 |
| Edge | 最新 3 个 | 是 | 是 |

#### 移动设备
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

### 测试结构

#### Playwright 风格
```javascript
import { test, expect, devices } from '@playwright/test';

test.describe('用户认证 @smoke @critical', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('应该用有效凭据登录', async ({ page }) => {
    // 准备
    const credentials = {
      email: 'user@example.com',
      password: 'SecurePassword123!'
    };

    // 执行
    await page.getByLabel('邮箱').fill(credentials.email);
    await page.getByLabel('密码').fill(credentials.password);
    await page.getByRole('button', { name: '登录' }).click();

    // 断言
    await expect(page).toHaveURL('/dashboard');
    await expect(page.getByText('欢迎')).toBeVisible();
  });

  test('应该显示无效凭据错误', async ({ page }) => {
    await page.getByLabel('邮箱').fill('invalid@example.com');
    await page.getByLabel('密码').fill('wrongpassword');
    await page.getByRole('button', { name: '登录' }).click();
    
    await expect(page.getByRole('alert')).toContainText('无效凭据');
  });
});
```

#### Cypress 风格
```javascript
describe('用户认证', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('应该用有效凭据登录', () => {
    cy.get('[data-testid="email-input"]').type('user@example.com');
    cy.get('[data-testid="password-input"]').type('SecurePassword123!');
    cy.get('[data-testid="login-button"]').click();
    
    cy.url().should('include', '/dashboard');
    cy.contains('欢迎').should('be.visible');
  });
});
```

### 元素定位策略

#### 1. 基于角色的定位器（首选）
```javascript
// 按钮
page.getByRole('button', { name: '提交' })
page.getByRole('link', { name: '了解更多' })

// 表单元素
page.getByRole('textbox', { name: '邮箱' })
page.getByRole('checkbox', { name: '记住我' })
page.getByRole('combobox', { name: '国家' })

// 导航
page.getByRole('navigation')
page.getByRole('menuitem', { name: '设置' })
```

#### 2. 基于标签的定位器
```javascript
page.getByLabel('邮箱地址')
page.getByLabel('密码')
page.getByPlaceholder('输入您的邮箱')
```

#### 3. 测试 ID 定位器
```javascript
page.getByTestId('submit-button')
page.getByTestId('user-profile-card')
```

#### 4. 文本内容定位器
```javascript
page.getByText('欢迎，用户！')
page.getByText(/你好, \w+/)
```

#### 5. CSS 选择器（后备）
```javascript
page.locator('.btn-primary')
page.locator('#user-menu')
page.locator('[data-custom="value"]')
```

### 交互

#### 基本动作
```javascript
// 点击
await page.click('button');
await page.dblclick('.item');
await page.rightclick('.context-menu-trigger');

// 填充
await page.fill('input[name="email"]', 'user@example.com');
await page.type('input[name="search"]', 'query', { delay: 100 });

// 选择
await page.selectOption('select[name="country"]', 'US');
await page.selectOption('select', { label: '美国' });

// 勾选
await page.check('input[type="checkbox"]');
await page.uncheck('input[type="checkbox"]');

// 文件上传
await page.setInputFiles('input[type="file"]', 'test-file.pdf');
```

#### 高级交互
```javascript
// 拖放
await page.dragAndDrop('.source', '.target');

// 悬停
await page.hover('.menu-item');

// 键盘
await page.keyboard.press('Enter');
await page.keyboard.type('你好世界');

// 鼠标
await page.mouse.click(100, 200);
await page.mouse.move(100, 200);
```

#### 框架处理
```javascript
// iframe
const frame = page.frameLocator('.iframe');
await frame.getByRole('button').click();

// 多个框架
for (const frame of page.frames()) {
  console.log(frame.url());
}
```

### 断言

#### 可见性断言
```javascript
await expect(page.locator('.element')).toBeVisible();
await expect(page.locator('.element')).toBeHidden();
await expect(page.locator('.element')).not.toBeVisible();
```

#### 文本断言
```javascript
await expect(page.locator('.message')).toHaveText('成功');
await expect(page.locator('.message')).toContainText('成功');
await expect(page.locator('.message')).toHaveText(/成功 \d+/);
```

#### 值断言
```javascript
await expect(page.locator('input')).toHaveValue('user@example.com');
await expect(page.locator('input')).toBeEmpty();
```

#### 状态断言
```javascript
await expect(page.locator('input')).toBeEnabled();
await expect(page.locator('input')).toBeDisabled();
await expect(page.locator('checkbox')).toBeChecked();
await expect(page.locator('input')).toBeEditable();
await expect(page.locator('input')).toBeFocused();
```

#### 属性断言
```javascript
await expect(page.locator('a')).toHaveAttribute('href', '/about');
await expect(page.locator('input')).toHaveClass('form-control');
await expect(page.locator('img')).toHaveCSS('width', '100px');
```

#### URL 断言
```javascript
await expect(page).toHaveURL('/dashboard');
await expect(page).toHaveURL(/dashboard/);
await expect(page).toHaveTitle('仪表板 | App');
```

### 视觉测试

#### 截图比较
```javascript
// 全页截图
await expect(page).toHaveScreenshot('homepage.png');

// 元素截图
await expect(page.locator('.card')).toHaveScreenshot('card.png');

// 带选项
await expect(page).toHaveScreenshot('homepage.png', {
  fullPage: true,
  animations: 'disabled',
  threshold: 0.1
});
```

#### 视口测试
```javascript
test('响应式布局', async ({ page }) => {
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

### 可访问性测试

```javascript
test('应该是可访问的', async ({ page }) => {
  await page.goto('/');
  
  const accessibilityScanResults = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa'])
    .analyze();
  
  expect(accessibilityScanResults.violations).toEqual([]);
});
```

### 网络处理

#### 拦截请求
```javascript
// 模拟 API 响应
await page.route('**/api/users', route => {
  route.fulfill({
    status: 200,
    contentType: 'application/json',
    body: JSON.stringify({ users: [] })
  });
});

// 阻止特定请求
await page.route('**/*.{png,jpg,jpeg}', route => route.abort());

// 修改请求
await page.route('**/api/**', route => {
  const headers = route.request().headers();
  headers['X-Custom-Header'] = 'value';
  route.continue({ headers });
});
```

#### 等待网络
```javascript
// 等待特定请求
const responsePromise = page.waitForResponse('**/api/users');
await page.click('button');
const response = await responsePromise;

// 等待所有网络空闲
await page.waitForLoadState('networkidle');
```

## 非功能需求

### 性能
- 测试初始化：< 5 秒
- 页面加载等待：可配置超时
- 元素等待：智能自动等待
- 截图捕获：< 1 秒

### 可靠性
- 不稳定操作的自动重试
- 内置等待机制
- 优雅的失败处理
- 详细的错误消息

### 安全性
- 安全的凭证存储
- 隔离的测试上下文
- 清理的浏览器状态
- 测试之间无数据泄漏

## 测试配置

### Playwright 配置
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

### Cypress 配置
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

## 输出规范

### 测试结果格式
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
      "name": "认证",
      "tests": [
        {
          "name": "应该成功登录",
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

## 页面对象模型

### 页面对象结构
```javascript
// pages/LoginPage.js
class LoginPage {
  constructor(page) {
    this.page = page;
    this.emailInput = page.getByLabel('邮箱');
    this.passwordInput = page.getByLabel('密码');
    this.submitButton = page.getByRole('button', { name: '登录' });
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

### 使用页面对象的测试
```javascript
const { LoginPage } = require('./pages/LoginPage');

test('应该登录', async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.goto();
  await loginPage.login('user@example.com', 'password');
  await expect(page).toHaveURL('/dashboard');
});
```

## 测试需求

### 单元测试
- 测试页面对象
- 测试工具函数
- 测试断言助手

### 集成测试
- 跨浏览器测试
- 测试响应式设计
- 测试可访问性
- 测试视觉回归

## 约束
- 最大测试持续时间：每个测试 5 分钟
- 最大并行工作进程：CPU 核心
- 最大截图大小：10MB
- 最大视频大小：100MB
