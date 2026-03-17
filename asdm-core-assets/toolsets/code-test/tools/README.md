# Tools Directory

This directory contains utility tools that support the CodeTest toolset functionality. These tools are designed to be called by AI agents in CLI environments.

## Available Tools

### 1. test-generator
Automated test case generation engine that analyzes source code and generates comprehensive test suites.

**Capabilities:**
- AST-based code analysis
- Multi-framework support (Jest, Mocha, PyTest, JUnit)
- Edge case detection
- Mock generation
- Coverage optimization

[View Documentation](./test-generator/README.md)

### 2. api-tester
API testing framework for REST, GraphQL, and WebSocket endpoints.

**Capabilities:**
- Request execution and validation
- Schema validation (OpenAPI, GraphQL)
- Performance benchmarking
- Mock server generation
- Environment management

[View Documentation](./api-tester/README.md)

### 3. ui-tester
UI testing automation framework for web applications.

**Capabilities:**
- Multi-browser testing
- Visual regression testing
- Accessibility testing
- Mobile device emulation
- Recording and playback

[View Documentation](./ui-tester/README.md)

### 4. report-generator
Test report generation and analytics engine.

**Capabilities:**
- Multi-format output (HTML, JSON, JUnit, Allure)
- Trend analysis
- Flaky test detection
- Coverage integration
- Notification dispatch

[View Documentation](./report-generator/README.md)

## Tool Structure

Each tool follows a consistent structure:

```
{tool-name}/
├── README.md           # Tool documentation
├── main.py            # Python entry point (or main.js for Node.js)
├── config.json        # Default configuration
├── lib/               # Core library code
├── templates/         # Template files
└── tests/             # Tool test suite
    ├── unit/
    └── integration/
```

## Usage

### Standalone Execution
Each tool can be executed independently:

```bash
# Python tools
python test-generator/main.py --file src/utils.js --framework jest

# Node.js tools
node api-tester/main.js run --collection api-tests.json
```

### Integration with Actions
Tools are typically invoked through action commands:

```
/generate-test-cases --file src/utils.js --type unit
/api-test run --collection api-tests.json --env staging
/ui-test run --suite tests/e2e/
/test-report generate --input results/ --format html
```

## Configuration

### Global Configuration
Tools share a global configuration at `~/.asdm/toolsets/codetest-toolset/config.json`:

```json
{
  "version": "1.0.0",
  "logLevel": "info",
  "outputDir": "./test-output",
  "cacheDir": "./.cache",
  "templatesDir": "./templates"
}
```

### Tool-Specific Configuration
Each tool has its own configuration file for specialized settings:

```json
// test-generator/config.json
{
  "defaultFramework": "jest",
  "coverageTarget": 85,
  "generateMocks": true,
  "includePrivate": false
}
```

## Development

### Adding a New Tool

1. Create a new directory under `tools/`
2. Implement the required interface:
   ```python
   class Tool:
       def __init__(self, config: dict):
           self.config = config
       
       def execute(self, params: dict) -> dict:
           """Execute the tool with given parameters"""
           pass
       
       def validate(self, params: dict) -> bool:
           """Validate input parameters"""
           pass
   ```
3. Create README.md with documentation
4. Add unit and integration tests
5. Update this README.md with the new tool

### Testing Tools
```bash
# Run all tool tests
pytest tools/*/tests/

# Run specific tool tests
pytest tools/test-generator/tests/

# Run with coverage
pytest --cov=tools tools/*/tests/
```

## Dependencies

### Python Tools
- Python 3.8+
- Required packages in `requirements.txt`

### Node.js Tools
- Node.js 16+
- Required packages in `package.json`

## Error Handling

All tools follow a consistent error handling pattern:

```json
{
  "status": "error",
  "error_code": "ERROR_CODE",
  "message": "Human-readable error message",
  "details": {
    "additional": "context"
  },
  "suggestion": "Suggested fix or action"
}
```

## Logging

Tools use structured logging:

```python
import logging

logger = logging.getLogger('codetest.tool')
logger.info("Operation started", extra={"tool": "test-generator", "operation": "analyze"})
```

## Output Formats

All tools return consistent output:

```json
{
  "status": "success",
  "data": {
    // Tool-specific results
  },
  "metadata": {
    "duration": 1234,
    "timestamp": "2024-01-15T10:30:00Z"
  }
}
```

## Best Practices

1. **Self-Contained**: Each tool should be independently executable
2. **Single Responsibility**: One clear purpose per tool
3. **Configurable**: Externalize configuration
4. **Testable**: Comprehensive test coverage
5. **Documented**: Clear README and inline documentation
6. **Robust**: Handle errors gracefully
7. **Efficient**: Optimize for performance

## Maintenance

### Updating Tools
1. Make changes to the tool code
2. Update tests if needed
3. Update documentation
4. Run full test suite
5. Update version in config.json

### Deprecating Tools
1. Mark as deprecated in README.md
2. Add deprecation warning in code
3. Provide migration path
4. Set removal date
