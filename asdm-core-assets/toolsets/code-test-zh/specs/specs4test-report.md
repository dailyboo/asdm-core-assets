# 测试报告规范

## 目的
本文档提供测试报告动作的详细规范，定义测试执行结果的报告生成、分析和可视化标准。

## 架构

### 报告生成流水线
```
┌─────────────────────────────────────────────────────────┐
│                  报告生成器                               │
├─────────────────────────────────────────────────────────┤
│  结果解析器      │  数据聚合器  │  分析器       │
├─────────────────────────────────────────────────────────┤
│  模板引擎        │  图表生成器  │  导出器       │
├─────────────────────────────────────────────────────────┤
│  趋势计算器      │  不稳定检测器   │  通知器     │
└─────────────────────────────────────────────────────────┘
```

## 功能需求

### 支持的输入格式

#### 1. JUnit XML
```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites>
  <testsuite name="用户测试" tests="5" failures="1" errors="0" skipped="0" time="12.5">
    <properties>
      <property name="browser" value="chrome"/>
      <property name="env" value="staging"/>
    </properties>
    <testcase name="test_login_success" classname="AuthTests" time="2.5"/>
    <testcase name="test_login_failure" classname="AuthTests" time="1.5">
      <failure message="断言失败" type="AssertionError">
        预期：true
        实际：false
        at AuthTests.test_login_failure (tests/auth.test.js:45)
      </failure>
    </testcase>
    <testcase name="test_slow_operation" classname="PerfTests" time="5.0">
      <skipped message="已禁用性能测试"/>
    </testcase>
  </testsuite>
</testsuites>
```

#### 2. JSON 测试结果
```json
{
  "stats": {
    "suites": 10,
    "tests": 50,
    "passes": 48,
    "failures": 2,
    "pending": 0,
    "skipped": 0,
    "start": "2024-01-15T10:00:00.000Z",
    "end": "2024-01-15T10:05:00.000Z",
    "duration": 300000
  },
  "results": [
    {
      "uuid": "test-001",
      "title": "应该成功登录",
      "fullTitle": "认证 应该成功登录",
      "duration": 2500,
      "state": "passed",
      "speed": "fast",
      "pass": true,
      "fail": false,
      "pending": false,
      "context": {
        "screenshot": "screenshots/test-001.png"
      }
    }
  ]
}
```

#### 3. Playwright 报告
```json
{
  "config": {
    "projects": [
      {"name": "chromium", "outputDir": "test-results"}
    ]
  },
  "suites": [
    {
      "title": "认证测试",
      "specs": [
        {
          "title": "应该登录",
          "ok": true,
          "tests": [
            {
              "status": "passed",
              "duration": 3000,
              "annotations": []
            }
          ]
        }
      ]
    }
  ]
}
```

#### 4. Allure 结果
```json
{
  "uuid": "test-001",
  "historyId": "login-test",
  "name": "应该成功登录",
  "status": "passed",
  "stage": "finished",
  "start": 1705312800000,
  "stop": 1705312803000,
  "parameters": [
    {"name": "browser", "value": "chrome"}
  ],
  "steps": [
    {
      "name": "导航到登录页面",
      "status": "passed",
      "stage": "finished",
      "start": 1705312800000,
      "stop": 1705312801000
    }
  ]
}
```

### 报告输出格式

#### 1. HTML 仪表板

##### 摘要部分
```html
<div class="summary-section">
  <div class="stat-card passed">
    <div class="stat-value">118</div>
    <div class="stat-label">通过 (94.4%)</div>
  </div>
  <div class="stat-card failed">
    <div class="stat-value">5</div>
    <div class="stat-label">失败 (4.0%)</div>
  </div>
  <div class="stat-card skipped">
    <div class="stat-value">2</div>
    <div class="stat-label">跳过 (1.6%)</div>
  </div>
  <div class="stat-card duration">
    <div class="stat-value">3:45</div>
    <div class="stat-label">持续时间</div>
  </div>
</div>
```

##### 测试结果表格
| 套件 | 测试 | 状态 | 持续时间 | 操作 |
|-------|------|--------|----------|---------|
| 认证 | 应该登录 | ✅ 通过 | 2.5s | [查看] |
| 用户管理 | 应该更新资料 | ❌ 失败 | 1.2s | [查看] [重新运行] |

##### 失败详情
```html
<div class="failure-details">
  <h3>测试：应该更新用户资料</h3>
  <div class="error-message">
    AssertionError：预期状态 200，实际为 500
  </div>
  <div class="stack-trace">
    at Context.&lt;anonymous&gt; (tests/user.test.js:48:12)
    at processTicksAndRejections (internal/process/task_queues.js:95:5)
  </div>
  <div class="artifacts">
    <a href="screenshots/failure-001.png">截图</a>
    <a href="videos/test-001.webm">视频</a>
  </div>
</div>
```

#### 2. JSON 报告
```json
{
  "meta": {
    "reportId": "report-20240115103000",
    "generatedAt": "2024-01-15T10:30:00Z",
    "project": "my-project",
    "environment": "staging",
    "branch": "main",
    "commit": "abc123"
  },
  "summary": {
    "total": 125,
    "passed": 118,
    "failed": 5,
    "skipped": 2,
    "duration": 225000,
    "coverage": 87.5
  },
  "suites": [
    {
      "name": "认证",
      "total": 25,
      "passed": 25,
      "failed": 0,
      "duration": 45000,
      "tests": [
        {
          "name": "应该成功登录",
          "status": "passed",
          "duration": 2500,
          "timestamp": "2024-01-15T10:00:00Z"
        }
      ]
    }
  ],
  "failures": [
    {
      "suite": "用户管理",
      "test": "应该更新资料",
      "error": {
        "message": "预期状态 200，实际为 500",
        "type": "AssertionError",
        "stack": "at tests/user.test.js:48"
      },
      "artifacts": {
        "screenshot": "screenshots/failure-001.png",
        "video": "videos/test-001.webm"
      }
    }
  ]
}
```

#### 3. JUnit XML 导出
```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites name="测试套件" tests="125" failures="5" time="225">
  <testsuite name="认证" tests="25" failures="0" time="45">
    <testcase name="应该成功登录" time="2.5"/>
  </testsuite>
  <testsuite name="用户管理" tests="30" failures="2" time="62">
    <testcase name="应该更新资料" time="1.2">
      <failure message="AssertionError：预期状态 200，实际为 500">
        at tests/user.test.js:48
      </failure>
    </testcase>
  </testsuite>
</testsuites>
```

#### 4. Allure 报告
```
allure-report/
├── data/
│   ├── categories.json
│   ├── graph.json
│   ├── timeline.json
│   └── behaviors.json
├── plugins/
├── index.html
└── app.js
```

### 分析功能

#### 1. 趋势分析
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
      },
      {
        "date": "2024-01-14",
        "total": 125,
        "passed": 120,
        "failed": 3,
        "passRate": 96.0,
        "avgDuration": 210000
      }
    ]
  }
}
```

#### 2. 不稳定测试检测
```javascript
{
  "flakyTests": [
    {
      "name": "应该加载用户数据",
      "suite": "用户管理",
      "flakiness": 0.25,
      "runs": 20,
      "passes": 15,
      "failures": 5,
      "recentFailures": [
        "2024-01-15T10:00:00Z",
        "2024-01-14T10:00:00Z"
      ],
      "possibleCauses": [
        "网络超时",
        "异步测试中的竞态条件"
      ]
    }
  ]
}
```

#### 3. 性能指标
```javascript
{
  "performance": {
    "avgDuration": 180000,
    "medianDuration": 150000,
    "p95Duration": 300000,
    "slowestTests": [
      {
        "name": "应该加载大数据集",
        "suite": "数据测试",
        "duration": 45000,
        "avgDuration": 42000
      }
    ]
  }
}
```

#### 4. 覆盖率集成
```javascript
{
  "coverage": {
    "lines": {
      "total": 5000,
      "covered": 4375,
      "percentage": 87.5,
      "diff": "+2.5%"
    },
    "branches": {
      "total": 1200,
      "covered": 1020,
      "percentage": 85.0
    },
    "functions": {
      "total": 250,
      "covered": 230,
      "percentage": 92.0
    },
    "uncoveredFiles": [
      {
        "file": "src/utils/helpers.js",
        "coverage": 45.5,
        "missedLines": [10, 15, 23, 45]
      }
    ]
  }
}
```

### 报告部分

#### 1. 执行摘要
- 整体通过率
- 测试执行时间
- 覆盖率百分比
- 关键失败数量
- 趋势方向（改进/恶化）

#### 2. 测试结果
- 按套件组织
- 按状态、持续时间排序
- 按标签过滤
- 按名称搜索

#### 3. 失败分析
- 按错误类型分组
- 相似失败聚类
- 堆栈跟踪分析
- 截图/视频附件

#### 4. 性能分析
- 最慢的测试
- 持续时间趋势
- 瓶颈识别
- 优化建议

#### 5. 覆盖率报告
- 按文件覆盖
- 覆盖率趋势
- 未覆盖代码高亮
- 覆盖率目标进度

### 通知集成

#### Slack 通知
```json
{
  "channel": "#qa-notifications",
  "attachments": [
    {
      "color": "good",
      "title": "测试执行完成",
      "fields": [
        {"title": "通过", "value": "118 (94.4%)", "short": true},
        {"title": "失败", "value": "5 (4.0%)", "short": true},
        {"title": "持续时间", "value": "3m 45s", "short": true},
        {"title": "覆盖率", "value": "87.5%", "short": true}
      ],
      "actions": [
        {
          "type": "button",
          "text": "查看报告",
          "url": "https://reports.example.com/test-20240115"
        }
      ]
    }
  ]
}
```

#### 邮件报告
```
主题：测试报告 - 2024-01-15 - 94.4% 通过率

摘要：
- 总测试数：125
- 通过：118 (94.4%)
- 失败：5 (4.0%)
- 持续时间：3m 45s

失败测试：
1. 用户管理 - 应该更新资料
   错误：预期状态 200，实际为 500
   
2. API 测试 - 应该处理超时
   错误：超时超过 30000ms

[查看完整报告] [下载产物]
```

## 非功能需求

### 性能
- 报告生成：1000 个测试 < 5 秒
- 大报告处理：分页支持
- 大数据集的内存效率

### 可扩展性
- 处理 10,000+ 测试的报告
- 支持多个并发报告请求
- 高效归档旧报告

### 可访问性
- WCAG 2.1 AA 合规报告
- 键盘导航支持
- 屏幕阅读器兼容

## 配置

### 报告配置
```json
{
  "report": {
    "title": "测试执行报告",
    "project": "my-project",
    "formats": ["html", "json", "junit"],
    "output": "./reports",
    "includeScreenshots": true,
    "includeVideos": true,
    "groupBy": "suite",
    "showTrends": true,
    "flakyDetection": true,
    "notifications": {
      "slack": {
        "channel": "#qa",
        "onFailure": true,
        "onSuccess": false
      },
      "email": {
        "recipients": ["qa@example.com"],
        "onFailure": true
      }
    }
  }
}
```

### 模板自定义
```javascript
// 自定义报告模板
{
  "template": "custom-template.html",
  "theme": {
    "primary": "#007bff",
    "success": "#28a745",
    "danger": "#dc3545",
    "warning": "#ffc107"
  },
  "logo": "company-logo.png",
  "sections": [
    "summary",
    "failures",
    "performance",
    "coverage",
    "trends"
  ]
}
```

## 输出规范

### 报告生成响应
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
    "passRate": 94.4,
    "duration": "3m 45s"
  },
  "alerts": [
    {
      "level": "warning",
      "message": "与上次运行相比，通过率下降了 2%"
    },
    {
      "level": "info",
      "message": "检测到 2 个不稳定测试"
    }
  ]
}
```

## 测试需求

### 单元测试
- 测试结果解析
- 测试数据聚合
- 测试图表生成
- 测试格式转换

### 集成测试
- 测试端到端报告生成
- 使用真实测试结果测试
- 测试通知投递
- 测试报告归档

## 约束
- 最大报告大小：50MB
- 最大历史保留：90 天
- 每个报告最大测试数：10,000
- 最大并发报告数：10
