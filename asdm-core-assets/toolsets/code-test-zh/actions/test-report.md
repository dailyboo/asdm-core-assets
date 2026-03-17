# 测试报告

## 描述
从测试执行结果生成包含分析、趋势和可操作见解的全面测试报告。

## 用法
```
/test-report [command] [options]
```

## 命令
- `generate`：生成测试报告
- `merge`：合并多个测试报告
- `compare`：比较不同运行的测试结果
- `trend`：分析测试趋势
- `export`：将报告导出为各种格式

## 参数

### 必需参数
- `--input`, `-i`：测试结果文件或目录
- `--format`, `-f`：输出格式（html、json、junit、allure）

### 可选参数
- `--output`, `-o`：报告输出目录
- `--title`：报告标题
- `--project`：项目名称
- `--env`：测试环境
- `--timestamp`：报告的自定义时间戳
- `--include-screenshots`：在报告中包含截图
- `--include-videos`：包含视频录制
- `--include-traces`：包含执行跟踪
- `--groupBy`：按（suite、feature、severity）分组结果
- `--show-trends`：显示历史趋势
- `--flaky-detection`：高亮不稳定测试
- `--coverage`：覆盖率报告文件
- `--metrics`：额外的指标 JSON

## 示例

### 生成 HTML 报告
```
/test-report generate --input test-results/ --format html --output reports/
```

### 生成 Allure 报告
```
/test-report generate --input test-results/ --format allure --output allure-report/
```

### 合并多个报告
```
/test-report merge --input report1.json,report2.json --output merged-report.json
```

### 比较测试运行
```
/test-report compare --baseline baseline.json --current current.json
```

### 生成趋势分析
```
/test-report trend --input test-history/ --period 30d --output trends.html
```

### 导出为 JUnit 格式
```
/test-report export --input test-results.json --format junit --output junit.xml
```

## 支持的输入格式

### 1. JUnit XML
```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites>
  <testsuite name="用户测试" tests="5" failures="1" time="12.5">
    <testcase name="test_login" classname="AuthTests" time="2.5"/>
    <testcase name="test_logout" classname="AuthTests" time="1.5">
      <failure message="断言失败">预期为 true，实际为 false</failure>
    </testcase>
  </testsuite>
</testsuites>
```

### 2. JSON 结果
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
      "currentRetry": 0,
      "err": {}
    }
  ]
}
```

### 3. Playwright 报告
```json
{
  "config": {
    "projects": [{"name": "chromium"}]
  },
  "suites": [
    {
      "title": "登录测试",
      "tests": [
        {
          "title": "应该登录",
          "status": "passed",
          "duration": 3000
        }
      ]
    }
  ]
}
```

## 报告类型

### 1. HTML 仪表板
交互式基于 Web 的报告：
- 关键指标摘要卡片
- 测试结果细分
- 执行时间线
- 带截图的失败详情
- 历史趋势

### 2. JSON 报告
用于集成的结构化数据：
- 机器可读格式
- API 消费
- 自定义处理
- 数据库导入

### 3. JUnit XML
CI/CD 标准格式：
- Jenkins 集成
- GitLab CI 支持
- Azure DevOps 兼容性
- TeamCity 支持

### 4. Allure 报告
丰富的可视化：
- 历史趋势
- 不稳定测试检测
- 严重级别
- 类别和特性

## 报告部分

### 执行摘要
```
┌─────────────────────────────────────────────┐
│         测试执行摘要                         │
├─────────────────────────────────────────────┤
│ 总测试数：        125                       │
│ 通过：             118 (94.4%)              │
│ 失败：             5    (4.0%)              │
│ 跳过：             2    (1.6%)              │
│ 持续时间：         3m 45s                   │
│ 覆盖率：           87.5%                    │
└─────────────────────────────────────────────┘
```

### 按套件的测试结果
```
┌──────────────────┬───────┬─────────┬─────────┬──────────┐
│ 套件             │ 总数  │ 通过    │ 失败    │ 持续时间 │
├──────────────────┼───────┼─────────┼─────────┼──────────┤
│ 认证             │   25  │   25    │    0    │   45.2s  │
│ 用户管理         │   30  │   28    │    2    │   62.1s  │
│ API 测试         │   40  │   38    │    2    │   52.3s  │
│ UI 测试          │   30  │   27    │    1    │   45.4s  │
└──────────────────┴───────┴─────────┴─────────┴──────────┘
```

### 失败测试详情
```
❌ 用户管理：应该更新用户资料
   位置：tests/user.test.js:45
   错误：AssertionError：预期状态 200，实际为 500
   持续时间：1.2s
   堆栈跟踪：
     at Context.<anonymous> (tests/user.test.js:48:12)
```

## 趋势分析

### 通过率趋势
```
100% ┤     ╭─╮
 95% ┤   ╭─╯ ╰╮
 90% ┤ ╭─╯    ╰──╮
 85% ┤─╯         ╰─
 80% ┤
     └──────────────────
      运行1  运行2  运行3
```

### 持续时间趋势
```
5m ┤
4m ┤     ╭─────╮
3m ┤ ╭───╯     ╰──╮
2m ┤─╯            ╰─
1m ┤
   └──────────────────
    运行1  运行2  运行3
```

## 指标

### 测试质量指标
- **通过率**：通过的测试百分比
- **不稳定性**：不稳定测试检测
- **覆盖率**：代码覆盖率百分比
- **持续时间**：执行时间趋势
- **失败率**：随时间推移的失败频率

### 性能指标
- **平均持续时间**：平均测试执行时间
- **最慢的测试**：前 10 个最慢的测试
- **并行效率**：并发执行性能
- **设置时间**：测试环境准备

## 覆盖率集成

```json
{
  "coverage": {
    "lines": {
      "total": 5000,
      "covered": 4375,
      "percentage": 87.5
    },
    "branches": {
      "total": 1200,
      "covered": 1020,
      "percentage": 85
    },
    "functions": {
      "total": 250,
      "covered": 230,
      "percentage": 92
    }
  }
}
```

## 相关规范
详细规范请参阅 [specs4test-report.md](../specs/specs4test-report.md)。

## 输出格式

### 报告生成响应
```json
{
  "status": "success",
  "report": {
    "path": "reports/test-report-2024-01-15.html",
    "format": "html",
    "size": "2.5MB",
    "url": "file:///reports/test-report-2024-01-15.html"
  },
  "summary": {
    "total": 125,
    "passed": 118,
    "failed": 5,
    "skipped": 2,
    "duration": "3m 45s"
  },
  "alerts": [
    {
      "type": "warning",
      "message": "检测到不稳定测试：用户管理 - 应该更新资料"
    }
  ]
}
```

## CI/CD 集成

### 发布到产物
```
/test-report generate --input results/ --format html --output reports/
```

### 发送通知
```
/test-report generate --input results/ --notify slack --channel #qa
```

### 上传到仪表板
```
/test-report generate --input results/ --upload https://reports.example.com
```

## 最佳实践

1. **一致的命名**：使用一致的测试命名约定
2. **历史数据**：保留历史报告用于趋势分析
3. **失败详情**：包含有意义的错误消息
4. **截图**：为 UI 测试失败附上截图
5. **分类**：使用标签和套件进行组织
6. **定期审查**：审查报告以识别模式
7. **可操作的见解**：关注可操作的指标

## 报告自定义

### 自定义模板
```
/test-report generate --input results/ --template custom-template.html
```

### 自定义指标
```
/test-report generate --input results/ --metrics custom-metrics.json
```

### 品牌化
```
/test-report generate --input results/ --logo company.png --theme dark
```
