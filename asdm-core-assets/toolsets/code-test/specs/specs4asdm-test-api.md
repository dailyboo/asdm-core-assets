# Specifications for ASDM Test API

## Purpose
This document provides detailed specifications for the ASDM API Test action, defining test execution, validation, and reporting standards for API testing.

## Architecture

### API Testing Framework
```
┌─────────────────────────────────────────────────────────┐
│                    API Test Runner                       │
├─────────────────────────────────────────────────────────┤
│  Collection Loader  │  Environment Manager  │  Executor │
├─────────────────────────────────────────────────────────┤
│  Request Builder    │  Response Validator   │  Reporter │
├─────────────────────────────────────────────────────────┤
│  Auth Handler       │  Schema Validator     │  Logger   │
└─────────────────────────────────────────────────────────┘
```

## Functional Requirements

### Test Collection Format

#### REST API Collection
```json
{
  "version": "1.0",
  "name": "API Test Collection",
  "variables": {
    "baseUrl": "{{env.BASE_URL}}",
    "token": "{{env.AUTH_TOKEN}}"
  },
  "tests": [
    {
      "id": "test-001",
      "name": "Get User by ID",
      "enabled": true,
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
          "expected": 500,
          "description": "Response time under 500ms"
        },
        {
          "type": "jsonPath",
          "path": "$.data.id",
          "expected": "{{userId}}",
          "description": "Should return correct user ID"
        }
      ],
      "postRequest": {
        "script": "pm.collectionVariables.set('extractedId', response.data.id);"
      }
    }
  ]
}
```

#### GraphQL Collection
```json
{
  "version": "1.0",
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
          "type": "jsonPath",
          "path": "$.data.user.id",
          "expected": "{{userId}}"
        },
        {
          "type": "jsonPath",
          "path": "$.errors",
          "expected": null
        }
      ]
    }
  ]
}
```

### Assertion Types

#### 1. Status Code Assertion
```json
{
  "type": "status",
  "expected": 200,
  "description": "Status code should be 200"
}
```

Supported operators:
- `equals` (default)
- `in` - status code in list
- `notIn` - status code not in list

#### 2. Response Time Assertion
```json
{
  "type": "responseTime",
  "operator": "<",
  "expected": 500,
  "unit": "ms"
}
```

Supported operators:
- `<` - less than
- `<=` - less than or equal
- `>` - greater than
- `>=` - greater than or equal

#### 3. JSON Path Assertion
```json
{
  "type": "jsonPath",
  "path": "$.data.items.length()",
  "operator": ">",
  "expected": 0
}
```

JSON Path syntax:
- `$.property` - root property
- `$.[*]` - all array items
- `$..property` - recursive descent

#### 4. Header Assertion
```json
{
  "type": "header",
  "name": "Content-Type",
  "expected": "application/json"
}
```

#### 5. Body Assertion
```json
{
  "type": "bodyContains",
  "expected": "success"
}
```

#### 6. Schema Assertion
```json
{
  "type": "schema",
  "schema": {
    "type": "object",
    "properties": {
      "id": {"type": "number"},
      "name": {"type": "string"}
    },
    "required": ["id", "name"]
  }
}
```

### Authentication

#### Bearer Token
```json
{
  "auth": {
    "type": "bearer",
    "token": "{{env.API_TOKEN}}"
  }
}
```

#### Basic Auth
```json
{
  "auth": {
    "type": "basic",
    "username": "{{env.USERNAME}}",
    "password": "{{env.PASSWORD}}"
  }
}
```

#### OAuth 2.0
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

#### API Key
```json
{
  "auth": {
    "type": "apiKey",
    "key": "X-API-Key",
    "value": "{{env.API_KEY}}",
    "location": "header"
  }
}
```

### Environment Management

#### Environment File
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
  "proxy": {
    "host": "proxy.example.com",
    "port": 8080
  }
}
```

### Request Execution

#### Execution Flow
1. Load test collection
2. Resolve environment variables
3. Apply authentication
4. Build request
5. Execute request
6. Capture response
7. Run assertions
8. Store results
9. Execute post-request scripts

#### Parallel Execution
```json
{
  "execution": {
    "parallel": true,
    "workers": 5,
    "strategy": "bySuite"
  }
}
```

#### Retry Configuration
```json
{
  "retry": {
    "count": 3,
    "delay": 1000,
    "backoff": "exponential",
    "retryOn": [500, 502, 503, 504]
  }
}
```

### Performance Testing

#### Load Testing Configuration
```json
{
  "benchmark": {
    "type": "load",
    "config": {
      "rps": 100,
      "duration": "60s",
      "rampUp": "10s",
      "workers": 10
    }
  }
}
```

#### Stress Testing Configuration
```json
{
  "benchmark": {
    "type": "stress",
    "config": {
      "users": 500,
      "rampUp": "30s",
      "duration": "120s",
      "threshold": {
        "errorRate": 0.01,
        "p95Latency": 1000
      }
    }
  }
}
```

## Non-Functional Requirements

### Performance
- Single test execution: < 30 seconds
- Collection execution: < 5 minutes
- Memory usage: < 200MB per worker
- CPU usage: < 50% during load test

### Reliability
- Handle network timeouts gracefully
- Support offline test design
- Validate requests before execution
- Provide detailed error messages

### Security
- Store credentials securely
- Support environment variable injection
- Mask sensitive data in logs
- Support certificate-based auth

## Output Specifications

### Test Result Format
```json
{
  "executionId": "exec-12345",
  "timestamp": "2024-01-15T10:30:00Z",
  "environment": "staging",
  "summary": {
    "total": 25,
    "passed": 23,
    "failed": 2,
    "skipped": 0,
    "duration": 12500,
    "avgResponseTime": 245
  },
  "results": [
    {
      "testId": "test-001",
      "name": "Get User by ID",
      "status": "passed",
      "duration": 245,
      "request": {
        "method": "GET",
        "url": "https://api.staging.example.com/users/12345"
      },
      "response": {
        "status": 200,
        "size": 1024,
        "time": 245
      },
      "assertions": [
        {
          "type": "status",
          "expected": 200,
          "actual": 200,
          "passed": true
        }
      ]
    }
  ]
}
```

### Error Response Format
```json
{
  "testId": "test-002",
  "name": "Create User",
  "status": "failed",
  "error": {
    "code": "ASSERTION_FAILED",
    "message": "Status code assertion failed",
    "details": {
      "expected": 201,
      "actual": 400,
      "response": {
        "error": "Validation failed",
        "fields": ["email"]
      }
    }
  }
}
```

## Mock Server

### Mock Configuration
```json
{
  "mock": {
    "port": 3000,
    "routes": [
      {
        "method": "GET",
        "path": "/users/:id",
        "response": {
          "status": 200,
          "body": {
            "id": "{{params.id}}",
            "name": "Mock User"
          }
        }
      }
    ]
  }
}
```

## Testing Requirements

### Unit Tests
- Test assertion evaluators
- Test variable resolver
- Test auth handlers
- Test request builder

### Integration Tests
- Test end-to-end execution
- Test with real APIs
- Test error scenarios
- Test parallel execution

## Configuration Schema

### Main Configuration
```json
{
  "timeout": 30000,
  "retry": {
    "count": 0,
    "delay": 1000
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
  }
}
```

## CI/CD Integration

### GitHub Actions Workflow
```yaml
name: API Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run API Tests
        run: /asdm-test-api run --collection api-tests.json --env staging
        env:
          API_TOKEN: ${{ secrets.API_TOKEN }}
```

## Constraints
- Maximum request size: 10MB
- Maximum response size: 100MB
- Maximum parallel workers: 20
- Maximum test duration: 1 hour
