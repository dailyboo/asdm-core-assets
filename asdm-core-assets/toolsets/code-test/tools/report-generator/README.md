# Report Generator

Test report generation and analytics engine for comprehensive test result visualization and analysis.

## Overview

The Report Generator tool processes test execution results and produces comprehensive reports with analytics, trends, and actionable insights.

## Features

- **Multi-Format Output**: HTML, JSON, JUnit XML, Allure
- **Trend Analysis**: Historical test result tracking
- **Flaky Test Detection**: Identify unstable tests
- **Coverage Integration**: Code coverage visualization
- **Notification Support**: Slack, Email, Webhook
- **Customizable Templates**: Custom branding and layout
- **CI/CD Integration**: Jenkins, GitLab CI, GitHub Actions

## Installation

```bash
# Install dependencies
npm install

# Or for Python version
pip install -r requirements.txt
```

## Usage

### Command Line

```bash
# Generate HTML report
node main.js generate --input test-results/ --format html --output reports/

# Generate Allure report
node main.js generate --input test-results/ --format allure --output allure-report/

# Merge multiple reports
node main.js merge --input report1.json,report2.json --output merged-report.json

# Compare test runs
node main.js compare --baseline baseline.json --current current.json

# Generate trend analysis
node main.js trend --input test-history/ --period 30d --output trends.html

# Export to JUnit format
node main.js export --input test-results.json --format junit --output junit.xml
```

### Parameters

| Parameter | Short | Description | Required | Default |
|-----------|-------|-------------|----------|---------|
| `--input` | `-i` | Test result file/dir | Yes | - |
| `--format` | `-f` | Output format | Yes | html |
| `--output` | `-o` | Output directory | No | ./reports |
| `--title` | | Report title | No | Test Report |
| `--project` | | Project name | No | - |
| `--env` | | Environment | No | - |
| `--include-screenshots` | | Include screenshots | No | true |
| `--include-videos` | | Include videos | No | false |
| `--show-trends` | | Show trends | No | true |
| `--flaky-detection` | | Detect flaky tests | No | true |
| `--coverage` | | Coverage file | No | - |

## Configuration

### config.json

```json
{
  "version": "1.0.0",
  "title": "Test Execution Report",
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

## Input Formats

### JUnit XML

```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites>
  <testsuite name="User Tests" tests="5" failures="1" time="12.5">
    <testcase name="test_login" time="2.5"/>
    <testcase name="test_logout" time="1.5">
      <failure message="Assertion failed">Expected true, got false</failure>
    </testcase>
  </testsuite>
</testsuites>
```

### JSON Results

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
      "title": "should login successfully",
      "fullTitle": "Auth should login successfully",
      "duration": 2500,
      "pass": true
    }
  ]
}
```

## Output Formats

### HTML Dashboard

Generates an interactive HTML report with:
- Executive summary cards
- Test result breakdown
- Failure details with screenshots
- Historical trends
- Coverage charts

### JSON Report

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
<testsuites name="Test Suite" tests="125" failures="5" time="225">
  <testsuite name="Authentication" tests="25" failures="0" time="45">
    <testcase name="should_login_successfully" time="2.5"/>
  </testsuite>
</testsuites>
```

## Analytics Features

### Trend Analysis

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

### Flaky Test Detection

```javascript
{
  "flakyTests": [
    {
      "name": "should load user data",
      "suite": "User Management",
      "flakiness": 0.25,
      "runs": 20,
      "passes": 15,
      "failures": 5
    }
  ]
}
```

### Performance Metrics

```javascript
{
  "performance": {
    "avgDuration": 180000,
    "medianDuration": 150000,
    "p95Duration": 300000,
    "slowestTests": [
      {
        "name": "should load large dataset",
        "duration": 45000
      }
    ]
  }
}
```

## Coverage Integration

```javascript
{
  "coverage": {
    "lines": {"total": 5000, "covered": 4375, "percentage": 87.5},
    "branches": {"total": 1200, "covered": 1020, "percentage": 85.0},
    "functions": {"total": 250, "covered": 230, "percentage": 92.0}
  }
}
```

## Report Sections

### 1. Executive Summary

```
┌─────────────────────────────────────────────┐
│         Test Execution Summary              │
├─────────────────────────────────────────────┤
│ Total Tests:        125                     │
│ Passed:             118 (94.4%)             │
│ Failed:             5   (4.0%)              │
│ Duration:           3m 45s                  │
│ Coverage:           87.5%                   │
└─────────────────────────────────────────────┘
```

### 2. Test Results by Suite

| Suite | Total | Passed | Failed | Duration |
|-------|-------|--------|--------|----------|
| Authentication | 25 | 25 | 0 | 45.2s |
| User Management | 30 | 28 | 2 | 62.1s |

### 3. Failure Details

```
❌ User Management: should update user profile
   Error: AssertionError: Expected status 200, got 500
   Duration: 1.2s
   Screenshot: screenshots/failure-001.png
```

## Notifications

### Slack Notification

```json
{
  "channel": "#qa-notifications",
  "attachments": [{
    "color": "good",
    "title": "Test Execution Complete",
    "fields": [
      {"title": "Passed", "value": "118 (94.4%)", "short": true},
      {"title": "Failed", "value": "5 (4.0%)", "short": true}
    ]
  }]
}
```

### Email Report

```
Subject: Test Report - 2024-01-15 - 94.4% Pass Rate

Summary:
- Total Tests: 125
- Passed: 118 (94.4%)
- Failed: 5 (4.0%)
- Duration: 3m 45s
```

## API Reference

### ReportGenerator Class

```javascript
class ReportGenerator {
  constructor(config) {
    this.config = config;
  }

  async generate(results, format) {
    /** Generate report from test results */
  }

  async merge(reports) {
    /** Merge multiple reports */
  }

  async compare(baseline, current) {
    /** Compare two test runs */
  }
}
```

### TrendAnalyzer Class

```javascript
class TrendAnalyzer {
  async analyze(history, period) {
    /** Analyze trends over time */
  }

  async detectFlakyTests(history) {
    /** Detect flaky tests */
  }
}
```

## Output Format

### Generation Response

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
      "message": "Pass rate decreased by 2%"
    }
  ]
}
```

## Examples

### Generate HTML report

```bash
node main.js generate \
  --input test-results/ \
  --format html \
  --output reports/ \
  --title "My Project Test Report"
```

### Merge and compare reports

```bash
node main.js merge \
  --input reports/*.json \
  --output merged-report.json

node main.js compare \
  --baseline reports/last-week.json \
  --current merged-report.json
```

### Generate with notifications

```bash
node main.js generate \
  --input test-results/ \
  --format html \
  --notify slack \
  --channel "#qa"
```

## CI/CD Integration

### GitHub Actions

```yaml
- name: Generate Report
  run: node main.js generate --input results/ --format html

- name: Publish Report
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

## Testing

```bash
# Run unit tests
npm test

# Run integration tests
npm run test:integration
```

## Best Practices

1. Keep historical reports for trend analysis
2. Configure meaningful notifications
3. Include screenshots for failures
4. Set up coverage integration
5. Customize templates for branding
6. Archive old reports regularly
7. Monitor flaky tests
