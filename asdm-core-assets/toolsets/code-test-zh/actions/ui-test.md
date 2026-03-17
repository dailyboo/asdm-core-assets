# UI 测试

## 描述
为 Web 应用程序执行自动化 UI 测试，包括端到端工作流、视觉回归测试和可访问性验证。

## 用法
```
/ui-test [command] [options]
```

## 命令
- `run`：执行 UI 测试套件
- `record`：记录用户交互以创建测试
- `screenshot`：捕获视觉测试的截图
- `debug`：交互式调试模式
- `codegen`：从交互生成测试代码

## 参数

### 必需参数
- `--suite`, `-s`：测试套件文件或目录

### 可选参数
- `--browser`：要测试的浏览器（chrome、firefox、safari、edge）
- `--headless`：以无头模式运行（默认：true）
- `--viewport`：视口大小（例如：1920x1080）
- `--device`：模拟移动设备
- `--slow-mo`：按毫秒减慢执行速度
- `--video`：录制测试执行视频
- `--trace`：捕获调试跟踪
- `--screenshot`：失败时截图
- `--parallel`：并行浏览器数量
- `--retry`：重试失败的测试
- `--grep`：按模式过滤测试
- `--report`：测试报告输出路径
- `--base-url`：测试的基础 URL
- `--auth`：认证凭据

## 示例

### 运行 UI 测试
```
/ui-test run --suite tests/e2e/
```

### 在特定浏览器上运行
```
/ui-test run --suite tests/e2e/ --browser firefox --headless false
```

### 移动设备测试
```
/ui-test run --suite tests/mobile/ --device "iPhone 13"
```

### 记录测试交互
```
/ui-test record --url https://example.com --output test.spec.js
```

### 生成测试代码
```
/ui-test codegen --url https://example.com --framework playwright
```

### 视觉回归测试
```
/ui-test run --suite tests/visual/ --screenshot on --update-snapshots
```

### 调试模式
```
/ui-test debug --suite tests/e2e/login.spec.js
```

## 测试套件结构

### Playwright 风格
```javascript
import { test, expect } from '@playwright/test';

test.describe('用户认证', () => {
  test('应该成功登录', async ({ page }) => {
    await page.goto('/login');
    await page.fill('[name="email"]', 'user@example.com');
    await page.fill('[name="password"]', 'password123');
    await page.click('button[type="submit"]');
    
    await expect(page).toHaveURL('/dashboard');
    await expect(page.locator('.welcome-message')).toBeVisible();
  });
});
```

### Cypress 风格
```javascript
describe('用户认证', () => {
  it('应该成功登录', () => {
    cy.visit('/login');
    cy.get('[name="email"]').type('user@example.com');
    cy.get('[name="password"]').type('password123');
    cy.get('button[type="submit"]').click();
    
    cy.url().should('include', '/dashboard');
    cy.get('.welcome-message').should('be.visible');
  });
});
```

## 测试类型

### 1. 端到端测试
完整的用户工作流：
- 用户注册和登录
- 购物车操作
- 表单提交
- 多页导航

### 2. 视觉回归测试
UI 一致性验证：
- 截图比较
- 布局验证
- 响应式设计测试
- 跨浏览器截图

### 3. 可访问性测试
WCAG 合规性：
- 键盘导航
- 屏幕阅读器兼容性
- 颜色对比度
- ARIA 属性

### 4. 性能测试
前端性能：
- 页面加载时间
- 首次内容绘制
- 可交互时间
- 网络瀑布图

## 定位器策略

### CSS 选择器
```javascript
page.locator('.class-name')
page.locator('#element-id')
page.locator('[data-testid="submit-btn"]')
```

### 文本内容
```javascript
page.getByText('欢迎')
page.getByRole('button', { name: '提交' })
```

### 可访问性定位器
```javascript
page.getByRole('button')
page.getByLabel('邮箱')
page.getByPlaceholder('输入密码')
```

## 页面对象模型

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

## 视觉测试配置

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

## 移动测试

### 设备模拟
```javascript
const iPhone = devices['iPhone 13'];
test.use({
  ...iPhone,
  hasTouch: true
});
```

### 响应式测试
```
/ui-test run --suite tests/responsive/ --viewport 375x667,768x1024,1920x1080
```

## 相关规范
详细规范请参阅 [specs4ui-test.md](../specs/specs4ui-test.md)。

## 输出格式

### 测试结果
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
      "name": "应该成功登录",
      "status": "passed",
      "duration": "3.2s",
      "screenshots": ["login-1.png"]
    },
    {
      "name": "应该显示用户资料",
      "status": "failed",
      "error": "未找到元素：.profile-card",
      "screenshot": "profile-failure.png"
    }
  ]
}
```

## 最佳实践

1. **使用稳定的定位器**：优先使用 data-testid 属性
2. **等待策略**：使用自动等待而不是显式等待
3. **隔离**：测试之间重置状态
4. **页面对象**：使用页面对象模型组织代码
5. **并行执行**：并行运行独立测试
6. **失败时录制视频**：录制视频用于调试
7. **截图比较**：有目的地更新基线

## 调试功能

### 跟踪查看器
```
/ui-test run --suite tests/ --trace on
```
打开交互式跟踪查看器显示：
- 操作时间线
- DOM 快照
- 网络请求
- 控制台日志

### 单步调试
```
/ui-test debug --suite tests/login.spec.js
```

### 检查器模式
```
/ui-test codegen --url https://example.com
```

## CI/CD 集成

### GitHub Actions
```yaml
- name: 运行 UI 测试
  run: /ui-test run --suite tests/e2e/ --report report.html
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
    - /ui-test run --suite tests/e2e/
```
