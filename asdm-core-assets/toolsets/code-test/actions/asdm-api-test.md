# ASDM API Test

## Description
Generate and execute comprehensive API tests including REST, GraphQL, and WebSocket endpoints with full request/response validation.

## Usage
```
/asdm-api-test [command] [options]
```

## Commands
- `run`: Execute API test suite
- `generate`: Generate API test cases
- `validate`: Validate API responses against schema
- `benchmark`: Run performance benchmarks
- `mock`: Start mock API server

## Parameters

### Required Parameters
- `--collection`, `-c`: Test collection file or directory
- `--env`, `-e`: Environment configuration (dev, staging, prod)

### Optional Parameters
- `--parallel`: Number of parallel requests (default: 1)
- `--timeout`: Request timeout in milliseconds (default: 30000)
- `--retry`: Number of retries for failed requests (default: 0)
- `--report`: Output report file path
- `--format`: Report format (json, html, junit)
- `--filter`: Filter tests by tag or name
- `--variables`: Environment variables JSON file
- `--delay`: Delay between requests in ms
- `--stop-on-failure`: Stop execution on first failure
- `--save-response`: Save response bodies to files
- `--schema`: OpenAPI/Swagger schema file for validation

## Examples

### Run API tests
```
/asdm-api-test run --collection api-tests.json --env staging
```

### Generate tests from OpenAPI spec
```
/asdm-api-test generate --schema api-spec.yaml --output tests/api/
```

### Run with parallel execution
```
/asdm-api-test run --collection api-tests/ --parallel 5 --env prod
```

### Validate responses against schema
```
/asdm-api-test validate --collection api-tests.json --schema api-spec.yaml
```

### Run performance benchmark
```
/asdm-api-test benchmark --collection load-tests.json --iterations 100
```

### Start mock server
```
/asdm-api-test mock --port 3000 --responses mock-data.json
```

## Test Collection Format

### REST API Test
```json
{
  "name": "User API Tests",
  "tests": [
    {
      "name": "Get User by ID",
      "request": {
        "method": "GET",
        "url": "{{baseUrl}}/users/{{userId}}",
        "headers": {
          "Authorization": "Bearer {{token}}"
        }
      },
      "assertions": [
        {"type": "status", "value": 200},
        {"type": "responseTime", "operator": "<", "value": 500},
        {"type": "jsonPath", "path": "$.id", "value": "{{userId}}"}
      ]
    }
  ]
}
```

### GraphQL Test
```json
{
  "name": "GraphQL Query Tests",
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
          "variables": {"id": "{{userId}}"}
        }
      },
      "assertions": [
        {"type": "status", "value": 200},
        {"type": "jsonPath", "path": "$.data.user.id", "value": "{{userId}}"}
      ]
    }
  ]
}
```

## Assertion Types

### Status Code
```json
{"type": "status", "value": 200}
{"type": "status", "operator": "in", "value": [200, 201]}
```

### Response Time
```json
{"type": "responseTime", "operator": "<", "value": 500}
```

### JSON Path
```json
{"type": "jsonPath", "path": "$.data.id", "value": 123}
{"type": "jsonPath", "path": "$.items.length()", "operator": ">", "value": 0}
```

### Header
```json
{"type": "header", "name": "Content-Type", "value": "application/json"}
```

### Body Contains
```json
{"type": "bodyContains", "value": "success"}
```

### Schema Validation
```json
{"type": "schema", "schemaFile": "schemas/user.json"}
```

## Environment Configuration

```json
{
  "name": "staging",
  "variables": {
    "baseUrl": "https://api.staging.example.com",
    "token": "{{env.API_TOKEN}}",
    "userId": "12345"
  },
  "defaults": {
    "timeout": 30000,
    "headers": {
      "Content-Type": "application/json"
    }
  }
}
```

## Authentication Support

### Bearer Token
```
/asdm-api-test run --auth bearer --token $API_TOKEN
```

### Basic Auth
```
/asdm-api-test run --auth basic --username user --password pass
```

### OAuth 2.0
```
/asdm-api-test run --auth oauth2 --client-id $CLIENT_ID --client-secret $SECRET
```

### API Key
```
/asdm-api-test run --auth api-key --key X-API-Key --value $API_KEY
```

## Related Specifications
See [specs4asdm-api-test.md](../specs/specs4asdm-api-test.md) for detailed specifications.

## Output Format

### Test Execution Report
```json
{
  "status": "completed",
  "summary": {
    "total": 25,
    "passed": 23,
    "failed": 2,
    "skipped": 0,
    "duration": "12.5s"
  },
  "results": [
    {
      "name": "Get User by ID",
      "status": "passed",
      "duration": "245ms",
      "response": {
        "status": 200,
        "size": "1.2KB"
      }
    }
  ]
}
```

## Performance Testing

### Load Testing
```
/asdm-api-test benchmark --type load --rps 100 --duration 60s
```

### Stress Testing
```
/asdm-api-test benchmark --type stress --users 500 --ramp-up 30s
```

### Spike Testing
```
/asdm-api-test benchmark --type spike --peak-rps 1000 --duration 10s
```

## Best Practices

1. **Environment Variables**: Use environment variables for sensitive data
2. **Test Isolation**: Each test should be independent
3. **Cleanup**: Clean up test data after execution
4. **Assertions**: Be specific with assertions
5. **Error Messages**: Include meaningful descriptions
6. **Retry Logic**: Implement retry for flaky endpoints

## CI/CD Integration

### GitHub Actions
```yaml
- name: Run API Tests
  run: /asdm-api-test run --collection api-tests.json --env ${{ matrix.env }}
```

### GitLab CI
```yaml
api_test:
  script:
    - /asdm-api-test run --collection api-tests.json --report report.json
  artifacts:
    reports:
      junit: report.json
```
