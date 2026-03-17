# Test Generator

Automated test case generation engine that analyzes source code and generates comprehensive test suites.

## Overview

The Test Generator tool analyzes source code using AST (Abstract Syntax Tree) parsing and generates test cases based on code structure, function signatures, and identified patterns.

## Features

- **Multi-Language Support**: JavaScript, TypeScript, Python, Java, C#
- **Multi-Framework Support**: Jest, Mocha, PyTest, JUnit, xUnit
- **Intelligent Analysis**: AST-based code parsing
- **Edge Case Detection**: Automatic identification of boundary conditions
- **Mock Generation**: Automatic mock creation for dependencies
- **Coverage Optimization**: Generate tests to meet coverage targets

## Installation

```bash
# Install dependencies
pip install -r requirements.txt

# Or for Node.js version
npm install
```

## Usage

### Command Line

```bash
# Generate unit tests
python main.py --file src/utils.js --type unit --framework jest

# Generate integration tests
python main.py --file src/api/ --type integration --output tests/

# Analyze code only
python main.py analyze --file src/utils.js --output analysis.json

# Generate with coverage target
python main.py --file src/ --coverage 90 --output tests/
```

### Parameters

| Parameter | Short | Description | Required | Default |
|-----------|-------|-------------|----------|---------|
| `--file` | `-f` | Source file or directory | Yes | - |
| `--type` | `-t` | Test type (unit, integration, e2e) | Yes | - |
| `--framework` | | Test framework | No | jest |
| `--output` | `-o` | Output directory | No | ./tests |
| `--coverage` | | Coverage target percentage | No | 80 |
| `--overwrite` | | Overwrite existing tests | No | false |
| `--template` | | Custom template file | No | default |
| `--exclude` | | Patterns to exclude | No | - |
| `--include-private` | | Include private methods | No | false |
| `--mock-external` | | Auto-mock external deps | No | true |

## Configuration

### config.json

```json
{
  "version": "1.0.0",
  "defaultFramework": "jest",
  "coverageTarget": 80,
  "generateMocks": true,
  "includePrivate": false,
  "namingConvention": {
    "pattern": "{functionName}_should_{behavior}_when_{condition}",
    "separator": "_"
  },
  "templates": {
    "unit": "templates/unit-test.hbs",
    "integration": "templates/integration-test.hbs",
    "e2e": "templates/e2e-test.hbs"
  },
  "exclusions": [
    "node_modules",
    "dist",
    "build",
    "*.config.js"
  ],
  "mockConfig": {
    "auto": true,
    "exclude": ["fs", "path", "crypto"]
  }
}
```

## Test Generation Process

### 1. Code Analysis Phase

```python
def analyze_code(file_path: str) -> AnalysisResult:
    """
    Analyze source code and extract:
    - Function signatures
    - Parameter types
    - Return types
    - Dependencies
    - Control flow
    """
    pass
```

### 2. Test Planning Phase

```python
def generate_test_plan(analysis: AnalysisResult) -> TestPlan:
    """
    Generate test scenarios:
    - Happy path tests
    - Edge cases
    - Error scenarios
    - Performance tests
    """
    pass
```

### 3. Test Generation Phase

```python
def generate_tests(plan: TestPlan, template: str) -> List[TestFile]:
    """
    Generate test code from plan:
    - Apply templates
    - Generate test data
    - Create mocks
    - Add assertions
    """
    pass
```

## Output Structure

```
tests/
├── unit/
│   ├── services/
│   │   ├── auth.test.js
│   │   └── user.test.js
│   └── utils/
│       └── helpers.test.js
├── integration/
│   ├── api.test.js
│   └── database.test.js
└── e2e/
    └── workflow.test.js
```

## Test Templates

### Unit Test Template

```javascript
/**
 * Generated tests for {{sourceFile}}
 * Generated at: {{timestamp}}
 */

import { {{functions}} } from '{{sourcePath}}';
import { {{mocks}} } from '../helpers/mocks';

describe('{{moduleName}}', () => {
  beforeEach(() => {
    {{#each mockSetup}}
    {{this}}
    {{/each}}
  });

  afterEach(() => {
    {{#each mockTeardown}}
    {{this}}
    {{/each}}
  });

  {{#each testCases}}
  describe('{{functionName}}', () => {
    it('{{testName}}', () => {
      // Arrange
      {{#each arrange}}
      {{this}}
      {{/each}}

      // Act
      {{act}}

      // Assert
      {{#each assert}}
      {{this}}
      {{/each}}
    });
  });

  {{/each}}
});
```

## Test Case Categories

### Happy Path Tests
- Normal inputs
- Expected outputs
- Standard workflows

### Edge Cases
- Empty inputs
- Boundary values
- Null/undefined
- Maximum values
- Special characters

### Error Scenarios
- Invalid inputs
- Missing dependencies
- Network errors
- Permission denied

### Performance Tests
- Large datasets
- Concurrent access
- Memory constraints

## Mock Generation

### Function Mocks

```javascript
// Generated mock
jest.mock('../services/api', () => ({
  fetchUser: jest.fn().mockResolvedValue({ id: 1, name: 'Test User' }),
  createUser: jest.fn().mockResolvedValue({ id: 2 }),
}));
```

### Module Mocks

```javascript
jest.mock('axios', () => ({
  get: jest.fn(),
  post: jest.fn(),
}));
```

## Code Coverage

### Coverage Analysis

```python
def calculate_coverage(source_file: str, test_file: str) -> CoverageResult:
    """
    Calculate coverage metrics:
    - Line coverage
    - Branch coverage
    - Function coverage
    - Statement coverage
    """
    pass
```

### Coverage Report

```json
{
  "source": "src/utils.js",
  "test": "tests/utils.test.js",
  "coverage": {
    "lines": {"total": 50, "covered": 45, "percentage": 90},
    "branches": {"total": 20, "covered": 18, "percentage": 90},
    "functions": {"total": 10, "covered": 10, "percentage": 100},
    "statements": {"total": 55, "covered": 50, "percentage": 90.9}
  }
}
```

## Supported Languages

### JavaScript/TypeScript
- ES6+ syntax
- TypeScript types
- JSX/TSX components
- Async/await

### Python
- Python 3.8+
- Type hints
- Async functions
- Decorators

### Java
- Java 8+
- Annotations
- Generics
- Streams

### C#
- C# 8.0+
- LINQ
- Async/await
- Records

## API Reference

### Analyzer Class

```python
class CodeAnalyzer:
    def __init__(self, language: str):
        self.language = language
    
    def parse(self, code: str) -> AST:
        """Parse code into AST"""
        pass
    
    def extract_functions(self, ast: AST) -> List[Function]:
        """Extract function definitions"""
        pass
    
    def infer_types(self, function: Function) -> TypeSignature:
        """Infer parameter and return types"""
        pass
```

### Generator Class

```python
class TestGenerator:
    def __init__(self, framework: str, template: str):
        self.framework = framework
        self.template = template
    
    def generate(self, analysis: AnalysisResult) -> List[TestFile]:
        """Generate test files"""
        pass
    
    def apply_template(self, test_case: TestCase) -> str:
        """Apply template to test case"""
        pass
```

## Examples

### Generate Tests for a Service

```bash
python main.py \
  --file src/services/auth.js \
  --type unit \
  --framework jest \
  --coverage 90 \
  --output tests/unit/services/
```

### Generate Integration Tests

```bash
python main.py \
  --file src/api/ \
  --type integration \
  --mock-external \
  --output tests/integration/
```

### Analyze Code Structure

```bash
python main.py analyze \
  --file src/utils.js \
  --output analysis.json
```

## Error Handling

```json
{
  "status": "error",
  "error_code": "PARSE_ERROR",
  "message": "Unable to parse source file",
  "details": {
    "file": "src/utils.js",
    "line": 42,
    "reason": "Unexpected token"
  },
  "suggestion": "Fix syntax error before generating tests"
}
```

## Testing

```bash
# Run unit tests
pytest tests/unit/

# Run integration tests
pytest tests/integration/

# Run with coverage
pytest --cov=lib tests/
```

## Best Practices

1. Run analysis before generation
2. Review generated tests
3. Customize for your needs
4. Maintain tests over time
5. Set realistic coverage targets
6. Use meaningful test names
7. Include edge cases
