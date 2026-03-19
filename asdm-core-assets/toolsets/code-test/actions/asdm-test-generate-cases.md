# ASDM Generate Test Cases

## Description
Automatically generate comprehensive test cases for source code, including unit tests, integration tests, and edge case scenarios.

## Usage
```
/asdm-test-generate-cases [options]
```

## Commands
- `analyze`: Analyze code and generate test plan
- `generate`: Generate test cases
- `update`: Update existing test cases

## Parameters

### Required Parameters
- `--file`, `-f`: Target file or directory to generate tests for
- `--type`, `-t`: Type of tests to generate (unit, integration, e2e)

### Optional Parameters
- `--framework`: Test framework to use (jest, mocha, pytest, junit)
- `--output`, `-o`: Output directory for generated tests
- `--coverage`: Target coverage percentage (default: 80)
- `--overwrite`: Overwrite existing test files
- `--template`: Custom template file for test generation
- `--exclude`: Patterns to exclude from test generation
- `--include-private`: Include private methods in test generation
- `--mock-external`: Auto-mock external dependencies

## Examples

### Basic unit test generation
```
/asdm-test-generate-cases --file src/utils.js --type unit
```

### Generate integration tests with specific framework
```
/asdm-test-generate-cases --file src/api/ --type integration --framework jest
```

### Generate tests with coverage target
```
/asdm-test-generate-cases --file src/services/ --type unit --coverage 90 --output tests/
```

### Analyze and generate test plan
```
/asdm-test-generate-cases analyze --file src/ --output test-plan.json
```

### Update existing tests
```
/asdm-test-generate-cases update --file src/utils.js --test-file tests/utils.test.js
```

## Test Generation Strategy

### Unit Tests
- Function/method level testing
- Input validation scenarios
- Boundary value analysis
- Error handling paths
- Mock external dependencies

### Integration Tests
- Module interaction testing
- Database operations
- API endpoints
- Service integration
- Event handling

### E2E Tests
- User workflow scenarios
- Critical business paths
- Cross-component interactions
- System-level validation

## Output Structure

Generated tests follow the standard structure:

```
tests/
├── unit/
│   ├── utils.test.js
│   └── services/
│       ├── auth.test.js
│       └── user.test.js
├── integration/
│   └── api.test.js
└── e2e/
    └── workflow.test.js
```

## Test Case Categories

### 1. Happy Path Tests
- Normal input scenarios
- Expected successful outcomes
- Standard workflow completion

### 2. Edge Cases
- Boundary values
- Empty inputs
- Maximum limits
- Null/undefined handling

### 3. Error Scenarios
- Invalid inputs
- Missing dependencies
- Network failures
- Permission errors

### 4. Performance Tests
- Large data sets
- Concurrent operations
- Memory constraints
- Timeout scenarios

## Related Specifications
See [specs4asdm-test-generate-cases.md](../specs/specs4asdm-test-generate-cases.md) for detailed specifications.

## Output Format

### Success Response
```json
{
  "status": "success",
  "files_generated": 5,
  "test_cases": 42,
  "coverage_estimate": "85%",
  "files": [
    {
      "path": "tests/unit/utils.test.js",
      "test_count": 12,
      "functions_covered": ["function1", "function2"]
    }
  ],
  "recommendations": [
    "Consider adding edge case tests for null inputs",
    "Increase coverage for error handling paths"
  ]
}
```

### Error Response
```json
{
  "status": "error",
  "error_code": "ANALYSIS_FAILED",
  "message": "Unable to analyze source file",
  "details": {
    "file": "src/utils.js",
    "reason": "Syntax error in source file"
  },
  "suggestion": "Fix syntax errors before generating tests"
}
```

## Best Practices

1. **Run analysis first**: Use `analyze` command to understand code structure
2. **Review generated tests**: Always review and customize generated tests
3. **Incremental generation**: Start with critical components
4. **Maintain tests**: Update tests when source code changes
5. **Coverage goals**: Set realistic coverage targets

## Integration with IDE

The generated tests can be:
- Directly executed in IDE
- Integrated with test runners
- Added to CI/CD pipelines
- Used for debugging

## Limitations

- Complex dynamic code may require manual test design
- External API dependencies need mock configuration
- UI components may require additional setup
- Performance tests need environment-specific tuning
