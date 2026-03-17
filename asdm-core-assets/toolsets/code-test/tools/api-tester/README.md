# API Tester

API testing framework for REST, GraphQL, and WebSocket endpoints with comprehensive validation and performance testing capabilities.

## Overview

The API Tester tool provides a complete solution for API testing, including request execution, response validation, schema compliance, and performance benchmarking.

## Features

- **Multi-Protocol Support**: REST, GraphQL, WebSocket
- **Schema Validation**: OpenAPI 3.0, Swagger 2.0, GraphQL Schema
- **Authentication**: Bearer, Basic, OAuth 2.0, API Key
- **Performance Testing**: Load, Stress, Spike testing
- **Mock Server**: Built-in mock server for testing
- **Environment Management**: Multiple environment configurations
- **Parallel Execution**: Concurrent test execution

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
# Run API tests
node main.js run --collection api-tests.json --env staging

# Generate tests from OpenAPI spec
node main.js generate --schema api-spec.yaml --output tests/

# Validate responses
node main.js validate --collection api-tests.json --schema api-spec.yaml

# Run performance benchmark
node main.js benchmark --collection load-tests.json --iterations 100

# Start mock server
node main.js mock --port 3000 --responses mock-data.json
```

### Parameters

| Parameter | Short | Description | Required | Default |
|-----------|-------|-------------|----------|---------|
| `--collection` | `-c` | Test collection file | Yes | - |
| `--env` | `-e` | Environment name | Yes | - |
| `--parallel` | | Parallel workers | No | 1 |
| `--timeout` | | Request timeout (ms) | No | 30000 |
| `--retry` | | Retry count | No | 0 |
| `--report` | | Report output path | No | - |
| `--format` | | Report format | No | json |
| `--filter` | | Filter by tag/name | No | - |
| `--variables` | | Variables file | No | - |

## Configuration

### config.json

```json
{
  "version": "1.0.0",
  "timeout": 30000,
  "retry": {
    "count": 0,
    "delay": 1000,
    "backoff": "exponential"
  },
  "parallel": {
    "enabled": false,
    "workers": 1
  },
  "report": {
    "format": "json",
    "output": "./reports"
  },
  "logging": {
    "level": "info",
    "file": "./logs/api-test.log"
  },
  "proxy": {
    "enabled": false,
    "host": "",
    "port": 8080
  }
}
```

## Test Collection Format

### REST API Collection

```json
{
  "version": "1.0.0",
  "name": "User API Tests",
  "variables": {
    "baseUrl": "{{env.BASE_URL}}",
    "token": "{{env.AUTH_TOKEN}}"
  },
  "tests": [
    {
      "id": "test-001",
      "name": "Get User by ID",
      "enabled": true,
      "tags": ["smoke", "user"],
      "request": {
        "method": "GET",
        "url": "{{baseUrl}}/users/{{userId}}",
        "headers": {
          "Authorization": "Bearer {{token}}",
          "Accept": "application/json"
        },
        "timeout": 30000
      },
      "assertions": [
        {
          "type": "status",
          "expected": 200,
          "description": "Should return 200 OK"
        },
        {
          "type": "responseTime",
          "operator": "<",
          "expected": 500
        },
        {
          "type": "jsonPath",
          "path": "$.data.id",
          "expected": "{{userId}}"
        }
      ],
      "postRequest": {
        "extract": [
          {
            "var": "extractedId",
            "path": "$.data.id"
          }
        ]
      }
    }
  ]
}
```

### GraphQL Collection

```json
{
  "version": "1.0.0",
  "name": "GraphQL Tests",
  "tests": [
    {
      "name": "Get User Query",
      "request": {
        "method": "POST",
        "url": "{{baseUrl}}/graphql",
        "headers": {
          "Content-Type": "application/json"
        },
        "body": {
          "query": "query GetUser($id: ID!) { user(id: $id) { id name email } }",
          "variables": {
            "id": "{{userId}}"
          }
        }
      },
      "assertions": [
        {
          "type": "status",
          "expected": 200
        },
        {
          "type": "jsonPath",
          "path": "$.data.user.id",
          "expected": "{{userId}}"
        }
      ]
    }
  ]
}
```

## Assertion Types

### Status Code

```json
{"type": "status", "expected": 200}
{"type": "status", "operator": "in", "expected": [200, 201]}
```

### Response Time

```json
{"type": "responseTime", "operator": "<", "expected": 500}
```

### JSON Path

```json
{"type": "jsonPath", "path": "$.data.id", "expected": 123}
{"type": "jsonPath", "path": "$.items.length()", "operator": ">", "expected": 0}
```

### Header

```json
{"type": "header", "name": "Content-Type", "expected": "application/json"}
```

### Schema

```json
{"type": "schema", "schemaFile": "schemas/user.json"}
```

## Environment Configuration

### environment.json

```json
{
  "name": "staging",
  "variables": {
    "BASE_URL": "https://api.staging.example.com",
    "API_TOKEN": "{{env.STAGING_API_TOKEN}}",
    "userId": "12345"
  },
  "defaults": {
    "timeout": 30000,
    "headers": {
      "Content-Type": "application/json",
      "Accept": "application/json"
    }
  },
  "auth": {
    "type": "bearer",
    "token": "{{API_TOKEN}}"
  }
}
```

## Authentication

### Bearer Token

```json
{
  "auth": {
    "type": "bearer",
    "token": "{{env.API_TOKEN}}"
  }
}
```

### OAuth 2.0

```json
{
  "auth": {
    "type": "oauth2",
    "grantType": "client_credentials",
    "clientId": "{{env.CLIENT_ID}}",
    "clientSecret": "{{env.CLIENT_SECRET}}",
    "tokenUrl": "{{env.TOKEN_URL}}"
  }
}
```

## Performance Testing

### Load Testing

```bash
node main.js benchmark \
  --collection load-tests.json \
  --type load \
  --rps 100 \
  --duration 60s
```

### Stress Testing

```bash
node main.js benchmark \
  --collection stress-tests.json \
  --type stress \
  --users 500 \
  --ramp-up 30s
```

## Mock Server

### Mock Configuration

```json
{
  "port": 3000,
  "routes": [
    {
      "method": "GET",
      "path": "/users/:id",
      "response": {
        "status": 200,
        "headers": {
          "Content-Type": "application/json"
        },
        "body": {
          "id": "{{params.id}}",
          "name": "Mock User",
          "email": "mock@example.com"
        }
      }
    },
    {
      "method": "POST",
      "path": "/users",
      "response": {
        "status": 201,
        "body": {
          "id": 1,
          "created": true
        }
      }
    }
  ]
}
```

## API Reference

### TestRunner Class

```javascript
class TestRunner {
  constructor(config) {
    this.config = config;
  }

  async runCollection(collection, env) {
    /** Execute test collection */
  }

  async executeTest(test, context) {
    /** Execute single test */
  }

  async evaluateAssertions(response, assertions) {
    /** Evaluate assertions */
  }
}
```

### Validator Class

```javascript
class SchemaValidator {
  constructor(schema) {
    this.schema = schema;
  }

  validate(response) {
    /** Validate response against schema */
  }

  getErrors() {
    /** Return validation errors */
  }
}
```

## Output Format

### Test Result

```json
{
  "executionId": "exec-12345",
  "timestamp": "2024-01-15T10:30:00Z",
  "environment": "staging",
  "summary": {
    "total": 25,
    "passed": 23,
    "failed": 2,
    "duration": 12500
  },
  "results": [
    {
      "testId": "test-001",
      "name": "Get User by ID",
      "status": "passed",
      "duration": 245,
      "request": {
        "method": "GET",
        "url": "https://api.example.com/users/12345"
      },
      "response": {
        "status": 200,
        "size": 1024,
        "time": 245
      }
    }
  ]
}
```

## Error Handling

```json
{
  "status": "error",
  "error_code": "REQUEST_FAILED",
  "message": "Request timeout",
  "details": {
    "url": "https://api.example.com/users/12345",
    "timeout": 30000
  },
  "suggestion": "Increase timeout or check network connectivity"
}
```

## Examples

### Run tests with report

```bash
node main.js run \
  --collection api-tests.json \
  --env staging \
  --report report.html \
  --format html
```

### Filter tests by tag

```bash
node main.js run \
  --collection api-tests.json \
  --env staging \
  --filter "smoke"
```

### Parallel execution

```bash
node main.js run \
  --collection api-tests.json \
  --env staging \
  --parallel 5
```

## CI/CD Integration

### GitHub Actions

```yaml
- name: Run API Tests
  run: node main.js run --collection api-tests.json --env staging
  env:
    API_TOKEN: ${{ secrets.API_TOKEN }}
```

### GitLab CI

```yaml
api_test:
  script:
    - node main.js run --collection api-tests.json --report report.json
  artifacts:
    reports:
      junit: report.json
```

## Testing

```bash
# Run unit tests
npm test

# Run integration tests
npm run test:integration

# Run with coverage
npm run test:coverage
```

## Best Practices

1. Use environment variables for secrets
2. Organize tests by feature/endpoint
3. Include descriptive test names
4. Set appropriate timeouts
5. Handle authentication properly
6. Validate response schemas
7. Test error scenarios
