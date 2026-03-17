# 报告生成器

测试报告生成和分析引擎，用于全面的测试结果可视化和分析。

## 概述

报告生成器工具处理测试执行结果，生成包含分析、趋势和可操作见解的全面报告。

## 特性

- **多格式输出**：HTML、JSON、JUnit XML、Allure
- **趋势分析**：历史测试结果跟踪
- **不稳定测试检测**：识别不稳定测试
- **覆盖率集成**：代码覆盖率可视化
- **通知支持**：Slack、Email、Webhook
- **可自定义模板**：自定义品牌和布局
- **CI/CD 集成**：Jenkins、GitLab CI、GitHub Actions

## 安装

```bash
# 安装依赖
npm install

# 或 Python 版本
pip install -r requirements.txt
```

## 用法

### 命令行

```bash
# 生成 HTML 报告
node main.js generate --input test-results/ --format html --output reports/

# 生成 Allure 报告
node main.js generate --input test-results/ --format allure --output allure-report/

# 合并多个报告
node main.js merge --input report1.json,report2.json --output merged-report.json

# 比较测试运行
node main.js compare --baseline baseline.json --current current.json

# 生成趋势分析
node main.js trend --input test-history/ --period 30d --output trends.html

# 导出为 JUnit 格式
node main.js export --input test-results.json --format junit --output junit.xml
```

### 参数

| 参数 | 简写 | 描述 | 必需 | 默认值 |
|-----------|-------|-------------|----------|---------|
| `--input` | `-i` | 测试结果文件/目录 | 是 | - |
| `--format` | `-f` | 输出格式 | 是 | html |
| `--output` | `-o` | 输出目录 | 否 | ./reports |
| `--title` | | 报告标题 | 否 | 测试报告 |
| `--project` | | 项目名称 | 否 | - |
| `--env` | | 环境 | 否 | - |
| `--include-screenshots` | | 包含截图 | 否 | true |
| `--include-videos` | | 包含视频 | 否 | false |
| `--show-trends` | | 显示趋势 | 否 | true |
| `--flaky-detection` | | 检测不稳定测试 | 否 | true |
| `--coverage` | | 覆盖率文件 | 否 | - |

## 配置

### config.json

```json
{
  "version": "1.0.0",
  "title": "测试执行报告",
  "formats": ["html", "json", "junit"],
  "outputDir": "./reports",
  "includeScreenshots": true,
  "includeVideos": true,
  "includeTraces": false,
  "groupBy": "suite",
  "showTrends": true,
  "flakyDetection": true,
  "retention": {
    "enabled": true,
    "days": 90,
    "maxReports": 100
  },
  "notifications": {
    "slack": {
      "enabled": false,
      "channel": "#qa",
      "onFailure": true,
      "onSuccess": false
    },
    "email": {
      "enabled": false,
      "recipients": ["qa@example.com"],
      "onFailure": true,
      "onSuccess": false
    }
  },
  "template": {
    "custom": null,
    "theme": {
      "primary": "#007bff",
      "success": "#28a745",
      "danger": "#dc3545",
      "warning": "#ffc107"
    },
    "logo": null,
    "sections": ["summary", "failures", "performance", "coverage", "trends"]
  }
}
```

## 输入格式

### JUnit XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites>
  <testsuite name="用户测试" tests="5" failures="1" time="12.5">
    <testcase name="test_login" time="2.5"/>
    <testcase name="test_logout" time="1.5">
      <failure message="断言失败">预期为 true，实际为 false</failure>
    </testcase>
  </testsuite>
</testsuites>
```

### JSON 结果

```json
{
  "stats": {
    "suites": 10,
    "tests": 50,
    "passes": 48,
    "failures": 2,
    "duration": 125000
  },
  "results": [
    {
      "title": "应该成功登录",
      "fullTitle": "认证 应该成功登录",
      "duration": 2500,
      "pass": true
    }
  ]
}
```

## 输出格式

### HTML 仪表板

生成交互式 HTML 报告，包含：
- 执行摘要卡片
- 测试结果细分
- 带截图的失败详情
- 历史趋势
- 覆盖率图表

### JSON 报告

```json
{
  "meta": {
    "reportId": "report-20240115103000",
    "generatedAt": "2024-01-15T10:30:00Z",
    "project": "my-project",
    "environment": "staging"
  },
  "summary": {
    "total": 125,
    "passed": 118,
    "failed": 5,
    "skipped": 2,
    "duration": 225000,
    "coverage": 87.5
  },
  "suites": [...],
  "failures": [...]
}
```

### JUnit XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="测试套件" tests="125" failures="5" time="225">
  <testsuite name="认证" tests="25" failures="0" time="45">
    <testcase name="应该成功登录" time="2.5"/>
  </testsuite>
</testsuites>
```

## 分析功能

### 趋势分析

```javascript
{
  "trend": {
    "period": "30d",
    "dataPoints": [
      {
        "date": "2024-01-15",
        "total": 125,
        "passed": 118,
        "failed": 5,
        "passRate": 94.4,
        "avgDuration": 225000
      }
    ],
    "direction": "improving"
  }
}
```

### 不稳定测试检测

```javascript
{
  "flakyTests": [
    {
      "name": "应该加载用户数据",
      "suite": "用户管理",
      "flakiness": 0.25,
      "runs": 20,
      "passes": 15,
      "failures": 5
    }
  ]
}
```

### 性能指标

```javascript
{
  "performance": {
    "avgDuration": 180000,
    "medianDuration": 150000,
    "p95Duration": 300000,
    "slowestTests": [
      {
        "name": "应该加载大数据集",
        "duration": 45000
      }
    ]
  }
}
```

## 覆盖率集成

```javascript
{
  "coverage": {
    "lines": {"total": 5000, "covered": 4375, "percentage": 87.5},
    "branches": {"total": 1200, "covered": 1020, "percentage": 85.0},
    "functions": {"total": 250, "covered": 230, "percentage": 92.0}
  }
}
```

## 报告部分

### 1. 执行摘要

```
┌─────────────────────────────────────────────┐
│         测试执行摘要                         │
├─────────────────────────────────────────────┤
│ 总测试数：        125                       │
│ 通过：             118 (94.4%)              │
│ 失败：             5   (4.0%)               │
│ 持续时间：         3m 45s                   │
│ 覆盖率：           87.5%                    │
└─────────────────────────────────────────────┘
```

### 2. 按套件的测试结果

| 套件 | 总数 | 通过 | 失败 | 持续时间 |
|-------|-------|--------|--------|----------|
| 认证 | 25 | 25 | 0 | 45.2s |
| 用户管理 | 30 | 28 | 2 | 62.1s |

### 3. 失败详情

```
❌ 用户管理：应该更新用户资料
   错误：AssertionError：预期状态 200，实际为 500
   持续时间：1.2s
   截图：screenshots/failure-001.png
```

## 通知

### Slack 通知

```json
{
  "channel": "#qa-notifications",
  "attachments": [{
    "color": "good",
    "title": "测试执行完成",
    "fields": [
      {"title": "通过", "value": "118 (94.4%)", "short": true},
      {"title": "失败", "value": "5 (4.0%)", "short": true}
    ]
  }]
}
```

### 邮件报告

```
主题：测试报告 - 2024-01-15 - 94.4% 通过率

摘要：
- 总测试数：125
- 通过：118 (94.4%)
- 失败：5 (4.0%)
- 持续时间：3m 45s
```

## API 参考

### ReportGenerator 类

```javascript
class ReportGenerator {
  constructor(config) {
    this.config = config;
  }

  async generate(results, format) {
    /** 从测试结果生成报告 */
  }

  async merge(reports) {
    /** 合并多个报告 */
  }

  async compare(baseline, current) {
    /** 比较两次测试运行 */
  }
}
```

### TrendAnalyzer 类

```javascript
class TrendAnalyzer {
  async analyze(history, period) {
    /** 分析随时间变化的趋势 */
  }

  async detectFlakyTests(history) {
    /** 检测不稳定测试 */
  }
}
```

## 输出格式

### 生成响应

```json
{
  "status": "success",
  "report": {
    "id": "report-20240115103000",
    "path": "reports/test-report-2024-01-15.html",
    "format": "html",
    "size": "2.5MB",
    "url": "https://reports.example.com/report-20240115103000"
  },
  "summary": {
    "total": 125,
    "passed": 118,
    "failed": 5,
    "passRate": 94.4
  },
  "alerts": [
    {
      "level": "warning",
      "message": "通过率下降了 2%"
    }
  ]
}
```

## 示例

### 生成 HTML 报告

```bash
node main.js generate \
  --input test-results/ \
  --format html \
  --output reports/ \
  --title "我的项目测试报告"
```

### 合并和比较报告

```bash
node main.js merge \
  --input reports/*.json \
  --output merged-report.json

node main.js compare \
  --baseline reports/last-week.json \
  --current merged-report.json
```

### 带通知生成

```bash
node main.js generate \
  --input test-results/ \
  --format html \
  --notify slack \
  --channel "#qa"
```

## CI/CD 集成

### GitHub Actions

```yaml
- name: 生成报告
  run: node main.js generate --input results/ --format html

- name: 发布报告
  uses: actions/upload-artifact@v3
  with:
    name: test-report
    path: reports/
```

### GitLab CI

```yaml
report:
  script:
    - node main.js generate --input results/ --format html
  artifacts:
    paths:
      - reports/
```

## 测试

```bash
# 运行单元测试
npm test

# 运行集成测试
npm run test:integration
```

## 最佳实践

1. 保留历史报告用于趋势分析
2. 配置有意义的通知
3. 为失败包含截图
4. 设置覆盖率集成
5. 为品牌自定义模板
6. 定期归档旧报告
7. 监控不稳定测试
