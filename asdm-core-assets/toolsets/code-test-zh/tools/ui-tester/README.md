# UI 测试器

Web 应用程序的 UI 测试自动化框架，支持多浏览器、视觉回归测试和可访问性验证。

## 概述

UI 测试器工具为 Web UI 测试提供全面的功能，包括端到端测试、视觉回归、跨浏览器兼容性和可访问性合规。

## 特性

- **多浏览器支持**：Chrome、Firefox、Safari、Edge
- **视觉回归**：截图比较和差异检测
- **可访问性测试**：WCAG 合规验证
- **移动测试**：设备模拟和响应式测试
- **并行执行**：并发测试运行
- **自动等待**：智能元素等待机制
- **录制**：视频和跟踪捕获
- **页面对象模型**：可维护的测试结构

## 安装

```bash
# 安装依赖
npm install

# 安装浏览器（Playwright）
npx playwright install
```

## 用法

### 命令行

```bash
# 运行 UI 测试
node main.js run --suite tests/e2e/

# 在特定浏览器上运行
node main.js run --suite tests/e2e/ --browser firefox

# 以有头模式运行
node main.js run --suite tests/e2e/ --headless false

# 移动测试
node main.js run --suite tests/mobile/ --device "iPhone 13"

# 调试模式
node main.js debug --suite tests/e2e/login.spec.js

# 生成测试代码
node main.js codegen --url https://example.com --output test.spec.js

# 视觉回归
node main.js run --suite tests/visual/ --screenshot on
```

### 参数

| 参数 | 简写 | 描述 | 必需 | 默认值 |
|-----------|-------|-------------|----------|---------|
| `--suite` | `-s` | 测试套件目录 | 是 | - |
| `--browser` | `-b` | 使用的浏览器 | 否 | chromium |
| `--headless` | | 无头运行 | 否 | true |
| `--viewport` | | 视口大小 | 否 | 1280x720 |
| `--device` | | 移动设备 | 否 | - |
| `--parallel` | | 并行工作进程数 | 否 | 1 |
| `--retry` | | 重试次数 | 否 | 0 |
| `--video` | | 录制视频 | 否 | off |
| `--trace` | | 捕获跟踪 | 否 | off |
| `--screenshot` | | 截图模式 | 否 | only-on-failure |
| `--report` | | 报告路径 | 否 | - |
| `--base-url` | | 基础 URL | 否 | - |

## 配置

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

## 测试结构

### Playwright 风格

```javascript
import { test, expect } from '@playwright/test';

test.describe('用户认证', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/login');
  });

  test('应该成功登录', async ({ page }) => {
    // 准备
    const email = 'user@example.com';
    const password = 'SecurePassword123!';

    // 执行
    await page.getByLabel('邮箱').fill(email);
    await page.getByLabel('密码').fill(password);
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

### Cypress 风格

```javascript
describe('用户认证', () => {
  beforeEach(() => {
    cy.visit('/login');
  });

  it('应该成功登录', () => {
    cy.get('[data-testid="email-input"]').type('user@example.com');
    cy.get('[data-testid="password-input"]').type('SecurePassword123!');
    cy.get('[data-testid="login-button"]').click();

    cy.url().should('include', '/dashboard');
    cy.contains('欢迎').should('be.visible');
  });
});
```

## 定位器策略

### 基于角色（首选）

```javascript
page.getByRole('button', { name: '提交' })
page.getByRole('link', { name: '了解更多' })
page.getByRole('textbox', { name: '邮箱' })
page.getByRole('checkbox', { name: '记住我' })
```

### 基于标签

```javascript
page.getByLabel('邮箱地址')
page.getByPlaceholder('输入您的邮箱')
```

### 测试 ID

```javascript
page.getByTestId('submit-button')
page.getByTestId('user-profile-card')
```

### 文本内容

```javascript
page.getByText('欢迎，用户！')
page.getByText(/你好, \w+/)
```

## 动作

### 基本交互

```javascript
// 点击
await page.click('button');
await page.dblclick('.item');
await page.rightclick('.context-menu');

// 填充
await page.fill('[name="email"]', 'user@example.com');
await page.type('[name="search"]', 'query', { delay: 100 });

// 选择
await page.selectOption('select[name="country"]', 'US');

// 勾选
await page.check('input[type="checkbox"]');
await page.uncheck('input[type="checkbox"]');

// 文件上传
await page.setInputFiles('input[type="file"]', 'test-file.pdf');
```

### 高级交互

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
```

## 断言

### 可见性

```javascript
await expect(page.locator('.element')).toBeVisible();
await expect(page.locator('.element')).toBeHidden();
```

### 文本内容

```javascript
await expect(page.locator('.message')).toHaveText('成功');
await expect(page.locator('.message')).toContainText('成功');
```

### 值

```javascript
await expect(page.locator('input')).toHaveValue('user@example.com');
await expect(page.locator('input')).toBeEmpty();
```

### 状态

```javascript
await expect(page.locator('input')).toBeEnabled();
await expect(page.locator('input')).toBeDisabled();
await expect(page.locator('checkbox')).toBeChecked();
```

### URL

```javascript
await expect(page).toHaveURL('/dashboard');
await expect(page).toHaveTitle('仪表板');
```

## 视觉回归

### 截图比较

```javascript
// 全页
await expect(page).toHaveScreenshot('homepage.png');

// 元素
await expect(page.locator('.card')).toHaveScreenshot('card.png');

// 带选项
await expect(page).toHaveScreenshot('homepage.png', {
  fullPage: true,
  threshold: 0.1
});
```

### 响应式测试

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

## 可访问性测试

```javascript
import AxeBuilder from '@axe-core/playwright';

test('应该是可访问的', async ({ page }) => {
  await page.goto('/');

  const results = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa'])
    .analyze();

  expect(results.violations).toEqual([]);
});
```

## 网络拦截

### 模拟响应

```javascript
await page.route('**/api/users', route => {
  route.fulfill({
    status: 200,
    contentType: 'application/json',
    body: JSON.stringify({ users: [] })
  });
});
```

### 阻止请求

```javascript
await page.route('**/*.{png,jpg,jpeg}', route => route.abort());
```

## 页面对象模型

### 页面对象

```javascript
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

### 用法

```javascript
const { LoginPage } = require('./pages/LoginPage');

test('应该登录', async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.goto();
  await loginPage.login('user@example.com', 'password');
  await expect(page).toHaveURL('/dashboard');
});
```

## 输出格式

### 测试结果

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
      "name": "认证",
      "tests": [
        {
          "name": "应该成功登录",
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

## 错误处理

```json
{
  "status": "error",
  "error_code": "ELEMENT_NOT_FOUND",
  "message": "未找到元素",
  "details": {
    "selector": ".non-existent-element",
    "timeout": 30000
  },
  "screenshot": "error-screenshot.png"
}
```

## 示例

### 运行所有测试

```bash
node main.js run --suite tests/
```

### 带视频录制运行

```bash
node main.js run --suite tests/e2e/ --video on
```

### 调试特定测试

```bash
node main.js debug --suite tests/login.spec.js
```

## CI/CD 集成

### GitHub Actions

```yaml
- name: 运行 UI 测试
  run: node main.js run --suite tests/e2e/
  artifacts:
    paths:
      - screenshots/
      - videos/
```

## 测试

```bash
# 运行单元测试
npm test

# 运行集成测试
npm run test:integration
```

## 最佳实践

1. 使用稳定的定位器（data-testid）
2. 实现自动等待
3. 测试之间重置状态
4. 使用页面对象模型
5. 尽可能并行运行
6. 失败时捕获视频
7. 保持测试独立
