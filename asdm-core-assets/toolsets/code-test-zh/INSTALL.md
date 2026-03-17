# CodeTest 工具集安装指南

## 前提条件

### 必需项
- **ASDM Bootstrapper** - ASDM 核心框架
- **Python 3.8+** - 用于基于 Python 的工具
- **Node.js 16+** - 用于 JavaScript/TypeScript 工具
- **Git** - 版本控制

### 可选项（根据测试需求）
- **Java 11+** - 用于 JUnit/TestNG 的 Java 测试
- **.NET SDK 6.0+** - 用于 xUnit/NUnit 的 C# 测试
- **Docker** - 用于容器化测试执行

## 安装步骤

### 1. 安装工具集

```bash
# 使用 ASDM Bootstrapper
asdm-bootstrapper install codetest-toolset

# 或手动克隆
git clone https://github.com/asdm/codetest-toolset.git
cd codetest-toolset
```

### 2. 验证安装

```bash
# 列出已安装的工具集
asdm-toolset list

# 验证 codetest-toolset
asdm-toolset show codetest-toolset
```

### 3. 初始化工具集

```bash
# 初始化配置
asdm-toolset init codetest-toolset

# 这将创建：
# ~/.asdm/toolsets/codetest-toolset/
# ├── config.json          - 主配置
# ├── templates/           - 测试模板
# └── environments/        - 环境配置
```

### 4. 安装依赖

#### Python 依赖

```bash
cd ~/.asdm/toolsets/codetest-toolset
pip install -r requirements.txt
```

#### Node.js 依赖

```bash
cd ~/.asdm/toolsets/codetest-toolset
npm install
```

### 5. 安装浏览器驱动（用于 UI 测试）

```bash
# 安装 Playwright 浏览器
npx playwright install

# 或安装特定浏览器
npx playwright install chromium
npx playwright install firefox
npx playwright install webkit
```

## 配置

### 主配置文件

编辑 `~/.asdm/toolsets/codetest-toolset/config.json`：

```json
{
  "version": "1.0.0",
  "logLevel": "info",
  "outputDir": "./test-output",
  "cacheDir": "./.cache",
  "templatesDir": "./templates",
  "defaultFramework": "jest",
  "coverageTarget": 80,
  "testTimeout": 30000,
  "parallelWorkers": 4
}
```

### 环境配置

在 `~/.asdm/toolsets/codetest-toolset/environments/` 中创建环境文件：

#### development.json

```json
{
  "name": "development",
  "baseUrl": "http://localhost:3000",
  "apiUrl": "http://localhost:8080/api",
  "credentials": {
    "username": "dev_user",
    "password": "{{env.DEV_PASSWORD}}"
  }
}
```

#### staging.json

```json
{
  "name": "staging",
  "baseUrl": "https://staging.example.com",
  "apiUrl": "https://api.staging.example.com",
  "credentials": {
    "token": "{{env.STAGING_TOKEN}}"
  }
}
```

#### production.json

```json
{
  "name": "production",
  "baseUrl": "https://example.com",
  "apiUrl": "https://api.example.com",
  "credentials": {
    "token": "{{env.PROD_TOKEN}}"
  }
}
```

### IDE 集成

#### VS Code

安装推荐的扩展：

```json
{
  "recommendations": [
    "ms-playwright.playwright",
    "orta.vscode-jest",
    "ms-vscode.test-adapter-converter"
  ]
}
```

#### WebStorm / IntelliJ IDEA

启用集成测试工具：
1. 打开 Settings > Languages & Frameworks > JavaScript > Testing
2. 配置 Jest/Mocha/Playwright
3. 将测试目录设置为 `tests/`

## 快速开始

### 1. 生成测试用例

```bash
/generate-test-cases --file src/utils.js --type unit --framework jest
```

### 2. 运行 API 测试

```bash
/api-test run --collection api-tests.json --env staging
```

### 3. 运行 UI 测试

```bash
/ui-test run --suite tests/e2e/ --browser chromium
```

### 4. 生成报告

```bash
/test-report generate --input test-results/ --format html --output reports/
```

## CI/CD 设置

### GitHub Actions

创建 `.github/workflows/test.yml`：

```yaml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'

      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Install dependencies
        run: |
          npm install
          pip install -r requirements.txt
          npx playwright install --with-deps

      - name: Run unit tests
        run: /generate-test-cases --file src/ --type unit

      - name: Run API tests
        run: /api-test run --collection api-tests.json --env staging
        env:
          API_TOKEN: ${{ secrets.API_TOKEN }}

      - name: Run UI tests
        run: /ui-test run --suite tests/e2e/

      - name: Generate report
        run: /test-report generate --input results/ --format html

      - name: Upload report
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: reports/
```

### GitLab CI

创建 `.gitlab-ci.yml`：

```yaml
stages:
  - test
  - report

variables:
  NODE_VERSION: '18'

test:
  stage: test
  image: node:${NODE_VERSION}
  script:
    - npm install
    - npx playwright install --with-deps
    - /ui-test run --suite tests/e2e/
  artifacts:
    paths:
      - test-results/
      - screenshots/
    expire_in: 1 week

report:
  stage: report
  script:
    - /test-report generate --input test-results/ --format html
  artifacts:
    paths:
      - reports/
    expire_in: 30 days
```

### Jenkins

创建 `Jenkinsfile`：

```groovy
pipeline {
  agent any

  stages {
    stage('Install') {
      steps {
        sh 'npm install'
        sh 'pip install -r requirements.txt'
        sh 'npx playwright install'
      }
    }

    stage('Test') {
      parallel {
        stage('Unit Tests') {
          steps {
            sh '/generate-test-cases --file src/ --type unit'
          }
        }
        stage('API Tests') {
          steps {
            withCredentials([string(credentialsId: 'api-token', variable: 'API_TOKEN')]) {
              sh '/api-test run --collection api-tests.json --env staging'
            }
          }
        }
        stage('UI Tests') {
          steps {
            sh '/ui-test run --suite tests/e2e/'
          }
        }
      }
    }

    stage('Report') {
      steps {
        sh '/test-report generate --input test-results/ --format html'
        publishHTML(target: [
          allowMissing: false,
          alwaysLinkToLastBuild: true,
          keepAll: true,
          reportDir: 'reports',
          reportFiles: 'index.html',
          reportName: 'Test Report'
        ])
      }
    }
  }
}
```

## Docker 设置

### Dockerfile

```dockerfile
FROM node:18

# 安装 Python
RUN apt-get update && apt-get install -y python3 python3-pip

# 安装 Playwright 依赖
RUN npx playwright install-deps

# 安装工具集
WORKDIR /app
COPY . .
RUN npm install
RUN pip install -r requirements.txt

# 运行测试
CMD ["/ui-test", "run", "--suite", "tests/"]
```

### docker-compose.yml

```yaml
version: '3.8'

services:
  test-runner:
    build: .
    volumes:
      - ./test-results:/app/test-results
      - ./reports:/app/reports
    environment:
      - API_TOKEN=${API_TOKEN}
      - ENV=staging
```

## 升级

### 检查更新

```bash
asdm-bootstrapper check-update codetest-toolset
```

### 更新到最新版本

```bash
asdm-bootstrapper update codetest-toolset
```

### 更新依赖

```bash
# 更新 npm 包
npm update

# 更新 pip 包
pip install --upgrade -r requirements.txt

# 更新浏览器
npx playwright install
```

## 卸载

### 移除工具集

```bash
asdm-bootstrapper uninstall codetest-toolset
```

### 清理

```bash
# 移除配置
rm -rf ~/.asdm/toolsets/codetest-toolset

# 移除缓存
rm -rf ./.cache

# 移除测试产物
rm -rf ./test-output
rm -rf ./reports
```

## 故障排除

### 常见问题

#### 浏览器安装失败

```bash
# 安装系统依赖
sudo npx playwright install-deps

# 或手动安装
sudo apt-get install -y libwoff1 libopus0 libwebp6 libwebpdemux2 libenchant-1-2-1 libgudev-1.0-0 libsecret-1-0 libhyphen0 libgdk-pixbuf2.0-0 libegl1 libgles2 libevent-2.1-7
```

#### 权限被拒绝

```bash
# 修复权限
chmod -R 755 ~/.asdm/toolsets/codetest-toolset
```

#### 端口已被占用

```bash
# 查找占用端口的进程
lsof -i :3000

# 终止进程
kill -9 <PID>
```

### 获取帮助

- **文档**：查看 `docs/` 目录
- **问题**：在 GitHub Issues 上报告错误
- **社区**：加入 ASDM Discord 频道
- **支持**：发送邮件至 support@asdm.dev

## 安全注意事项

### 凭证管理

- 在环境变量中存储机密信息
- 使用 `.env` 文件（切勿提交到 git）
- 正确配置 `.gitignore`：

```gitignore
.env
.env.local
.env.*.local
credentials.json
secrets.json
```

### CI/CD 机密

- 使用 GitHub Secrets 存储敏感数据
- 配置 GitLab CI 变量
- 使用 Jenkins 凭证

## 下一步

1. 配置测试环境
2. 创建第一个测试集合
3. 运行初始测试
4. 设置 CI/CD 集成
5. 配置通知

详细使用方法请参阅 [README.md](README.md) 文件。
