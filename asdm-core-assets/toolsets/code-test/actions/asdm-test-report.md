# ASDM Test Report

## Description
Generate comprehensive test reports with analytics, trends, and actionable insights from test execution results.

## Usage
```
/asdm-test-report [command] [options]
```

## Commands
- `generate`: Generate test report
- `merge`: Merge multiple test reports
- `compare`: Compare test results across runs
- `trend`: Analyze test trends over time
- `export`: Export report to various formats

## Parameters

### Required Parameters
- `--input`, `-i`: Test result file or directory
- `--format`, `-f`: Output format (html, json, junit, allure)

### Optional Parameters
- `--output`, `-o`: Output directory for report
- `--title`: Report title
- `--project`: Project name
- `--env`: Environment tested
- `--timestamp`: Custom timestamp for report
- `--include-screenshots`: Include screenshots in report
- `--include-videos`: Include video recordings
- `--include-traces`: Include execution traces
- `--groupBy`: Group results by (suite, feature, severity)
- `--show-trends`: Show historical trends
- `--flaky-detection`: Highlight flaky tests
- `--coverage`: Coverage report file
- `--metrics`: Additional metrics JSON

## Examples

### Generate HTML report
```
/asdm-test-report generate --input test-results/ --format html --output reports/
```

### Generate Allure report
```
/asdm-test-report generate --input test-results/ --format allure --output allure-report/
```

### Merge multiple reports
```
/asdm-test-report merge --input report1.json,report2.json --output merged-report.json
```

### Compare test runs
```
/asdm-test-report compare --baseline baseline.json --current current.json
```

### Generate trend analysis
```
/asdm-test-report trend --input test-history/ --period 30d --output trends.html
```

### Export to JUnit format
```
/asdm-test-report export --input test-results.json --format junit --output junit.xml
```

## Supported Input Formats

### 1. JUnit XML
```xml
<?xml version="1.0" encoding="UTF-8"?>
<testsuites>
  <testsuite name="User Tests" tests="5" failures="1" time="12.5">
    <testcase name="test_login" classname="AuthTests" time="2.5"/>
    <testcase name="test_logout" classname="AuthTests" time="1.5">
      <failure message="Assertion failed">Expected true, got false</failure>
    </testcase>
  </testsuite>
</testsuites>
```

### 2. JSON Results
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
      "currentRetry": 0,
      "err": {}
    }
  ]
}
```

### 3. Playwright Report
```json
{
  "config": {
    "projects": [{"name": "chromium"}]
  },
  "suites": [
    {
      "title": "Login Tests",
      "tests": [
        {
          "title": "should login",
          "status": "passed",
          "duration": 3000
        }
      ]
    }
  ]
}
```

## Report Types

### 1. HTML Dashboard
Interactive web-based report:
- Summary cards with key metrics
- Test result breakdown
- Execution timeline
- Failure details with screenshots
- Historical trends

### 2. JSON Report
Structured data for integration:
- Machine-readable format
- API consumption
- Custom processing
- Database import

### 3. JUnit XML
CI/CD standard format:
- Jenkins integration
- GitLab CI support
- Azure DevOps compatibility
- TeamCity support

### 4. Allure Report
Rich visualization:
- Historical trends
- Flaky test detection
- Severity levels
- Categories and features

## Report Sections

### Executive Summary
```
┌─────────────────────────────────────────────┐
│         Test Execution Summary              │
├─────────────────────────────────────────────┤
│ Total Tests:        125                     │
│ Passed:             118 (94.4%)             │
│ Failed:             5    (4.0%)             │
│ Skipped:            2    (1.6%)             │
│ Duration:           3m 45s                  │
│ Coverage:           87.5%                   │
└─────────────────────────────────────────────┘
```

### Test Results by Suite
```
┌──────────────────┬───────┬─────────┬─────────┬──────────┐
│ Suite            │ Total │ Passed  │ Failed  │ Duration │
├──────────────────┼───────┼─────────┼─────────┼──────────┤
│ Authentication   │   25  │   25    │    0    │   45.2s  │
│ User Management  │   30  │   28    │    2    │   62.1s  │
│ API Tests        │   40  │   38    │    2    │   52.3s  │
│ UI Tests         │   30  │   27    │    1    │   45.4s  │
└──────────────────┴───────┴─────────┴─────────┴──────────┘
```

### Failed Tests Details
```
❌ User Management: should update user profile
   Location: tests/user.test.js:45
   Error: AssertionError: Expected status 200, got 500
   Duration: 1.2s
   Stack Trace:
     at Context.<anonymous> (tests/user.test.js:48:12)
```

## Trend Analysis

### Pass Rate Trend
```
100% ┤     ╭─╮
 95% ┤   ╭─╯ ╰╮
 90% ┤ ╭─╯    ╰──╮
 85% ┤─╯         ╰─
 80% ┤
     └──────────────────
      Run 1  Run 2  Run 3
```

### Duration Trend
```
5m ┤
4m ┤     ╭─────╮
3m ┤ ╭───╯     ╰──╮
2m ┤─╯            ╰─
1m ┤
   └──────────────────
    Run 1  Run 2  Run 3
```

## Metrics

### Test Quality Metrics
- **Pass Rate**: Percentage of passed tests
- **Flakiness**: Unstable test detection
- **Coverage**: Code coverage percentage
- **Duration**: Execution time trends
- **Failure Rate**: Failure frequency over time

### Performance Metrics
- **Average Duration**: Mean test execution time
- **Slowest Tests**: Top 10 slowest tests
- **Parallel Efficiency**: Concurrent execution performance
- **Setup Time**: Test environment preparation

## Coverage Integration

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

## Related Specifications
See [specs4asdm-test-report.md](../specs/specs4asdm-test-report.md) for detailed specifications.

## Output Format

### Report Generation Response
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
      "message": "Flaky test detected: User Management - should update profile"
    }
  ]
}
```

## CI/CD Integration

### Publish to Artifacts
```
/asdm-test-report generate --input results/ --format html --output reports/
```

### Send Notifications
```
/asdm-test-report generate --input results/ --notify slack --channel #qa
```

### Upload to Dashboard
```
/asdm-test-report generate --input results/ --upload https://reports.example.com
```

## Best Practices

1. **Consistent Naming**: Use consistent test naming conventions
2. **Historical Data**: Keep historical reports for trend analysis
3. **Failure Details**: Include meaningful error messages
4. **Screenshots**: Attach screenshots for UI test failures
5. **Categorization**: Use tags and suites for organization
6. **Regular Review**: Review reports to identify patterns
7. **Actionable Insights**: Focus on actionable metrics

## Report Customization

### Custom Templates
```
/asdm-test-report generate --input results/ --template custom-template.html
```

### Custom Metrics
```
/asdm-test-report generate --input results/ --metrics custom-metrics.json
```

### Branding
```
/asdm-test-report generate --input results/ --logo company.png --theme dark
```
