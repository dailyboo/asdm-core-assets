# CodeTest Toolset

A comprehensive testing toolset for automated test case generation, API testing, UI testing, and test reporting.

## Overview

The CodeTest Toolset provides a complete testing solution that integrates with AI coding tools to automate the testing lifecycle. It supports multiple testing paradigms including unit tests, integration tests, API tests, and UI tests.

## Features

### 1. Test Case Generation
Automatically generate comprehensive test cases based on code analysis and specifications:
- Unit test generation for functions and methods
- Integration test scenarios
- Edge case detection and coverage
- Test data generation

### 2. API Testing
Generate and execute API tests with full request/response validation:
- RESTful API testing
- GraphQL API support
- Authentication testing
- Performance benchmarking
- Schema validation

### 3. UI Testing
Automated UI testing for web applications:
- End-to-end testing workflows
- Visual regression testing
- Cross-browser compatibility
- Accessibility testing
- Responsive design validation

### 4. Test Reporting
Comprehensive test reporting and analytics:
- Test execution summaries
- Coverage analysis
- Trend visualization
- CI/CD integration
- Export to multiple formats (HTML, JSON, JUnit XML)

## Components

### Actions
Actions are converted to slash commands for AI coding tools:

| Action | Command | Description |
|--------|---------|-------------|
| `generate-test-cases` | `/generate-test-cases` | Generate test cases from code |
| `api-test` | `/api-test` | Create and run API tests |
| `ui-test` | `/ui-test` | Execute UI tests |
| `test-report` | `/test-report` | Generate test reports |

### Specifications
Detailed specifications for each action are available in the `specs/` directory:
- [Test Case Generation Specs](specs/specs4generate-test-cases.md)
- [API Test Specs](specs/specs4api-test.md)
- [UI Test Specs](specs/specs4ui-test.md)
- [Test Report Specs](specs/specs4test-report.md)

### Tools
Utility tools for CLI environments are located in the `tools/` directory:
- `test-generator/` - Test case generation engine
- `api-tester/` - API testing framework
- `ui-tester/` - UI testing automation
- `report-generator/` - Report generation utilities

## Quick Start

1. **Generate test cases for a file:**
   ```
   /generate-test-cases --file src/utils.js --type unit
   ```

2. **Run API tests:**
   ```
   /api-test run --collection api-tests.json --env staging
   ```

3. **Execute UI tests:**
   ```
   /ui-test run --suite e2e --browser chrome
   ```

4. **Generate test report:**
   ```
   /test-report generate --format html --output reports/
   ```

## Installation

See [INSTALL.md](INSTALL.md) for detailed installation instructions.

## Supported Technologies

### Test Frameworks
- Jest, Mocha, Vitest (JavaScript/TypeScript)
- PyTest, unittest (Python)
- JUnit, TestNG (Java)
- xUnit, NUnit (.NET)

### API Testing
- REST Clients
- GraphQL
- OpenAPI/Swagger
- Postman Collections

### UI Testing
- Playwright
- Selenium
- Cypress
- Puppeteer

### Reporting
- Allure
- Mochawesome
- HTML Reports
- JUnit XML

## Best Practices

1. **Test Coverage**: Aim for at least 80% code coverage
2. **Test Isolation**: Each test should be independent
3. **Descriptive Names**: Use clear test case naming
4. **Assertions**: Include meaningful assertions
5. **Cleanup**: Properly clean up test resources

## Configuration

Configuration files are located in `~/.asdm/toolsets/codetest-toolset/`:
- `config.json` - Main configuration
- `templates/` - Test templates
- `environments/` - Environment configurations

## CI/CD Integration

The toolset supports integration with popular CI/CD platforms:
- GitHub Actions
- GitLab CI
- Jenkins
- Azure DevOps
- CircleCI

## Contributing

When adding new test utilities:
1. Follow the existing directory structure
2. Include comprehensive documentation
3. Add unit tests for new functionality
4. Update relevant specifications

## License

This toolset is part of the ASDM ecosystem.
