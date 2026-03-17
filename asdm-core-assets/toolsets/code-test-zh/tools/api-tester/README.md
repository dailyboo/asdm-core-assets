# API 测试器

用于 REST、GraphQL 和 WebSocket 端点的 API 测试框架，提供全面的验证和性能测试功能。

## 概述

API 测试器工具为 API 测试提供完整的解决方案，包括请求执行、响应验证、模式合规性和性能基准测试。

## 特性

- **多协议支持**：REST、GraphQL、WebSocket
- **模式验证**：OpenAPI 3.0、Swagger 2.0、GraphQL 模式
- **认证**：Bearer、Basic、OAuth 2.0、API Key
- **性能测试**：负载、压力、峰值测试
- **模拟服务器**：内置模拟服务器用于测试
- **环境管理**：多环境配置
- **并行执行**：并发测试执行

## 安装

```bash
# 安装依赖
npm install

# 或 Python 版本
pip install -r requirements.txt
```

## 用法

### 命令行

```bash
# 运行 API 测试
node main.js run --collection api-tests.json --env staging

# 从 OpenAPI 规范生成测试
node main.js generate --schema api-spec.yaml --output tests/

# 验证响应
node main.js validate --collection api-tests.json --schema api-spec.yaml

# 运行性能基准测试
node main.js benchmark --collection load-tests.json --iterations 100

# 启动模拟服务器
node main.js mock --port 3000 --responses mock-data.json
```

### 参数

| 参数 | 简写 | 描述 | 必需 | 默认值 |
|-----------|-------|-------------|----------|---------|
| `--collection` | `-c` | 测试集合文件 | 是 | - |
| `--env` | `-e` | 环境名称 | 是 | - |
| `--parallel` | | 并行工作进程数 | 否 | 1 |
| `--timeout` | | 请求超时（毫秒） | 否 | 30000 |
| `--retry` | | 重试次数 | 否 | 0 |
| `--report` | | 报告输出路径 | 否 | - |
| `--format` | | 报告格式 | 否 | json |
| `--filter` | | 按标签/名称过滤 | 否 | - |
| `--variables` | | 变量文件 | 否 | - |

## 配置

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

## 测试集合格式

### REST API 集合

```json
{
  "version": "1.0.0",
  "name": "用户 API 测试",
  "variables": {
    "baseUrl": "{{env.BASE_URL}}",
    "token": "{{env.AUTH_TOKEN}}"
  },
  "tests": [
    {
      "id": "test-001",
      "name": "按 ID 获取用户",
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
          "description": "应该返回 200 OK"
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

### GraphQL 集合

```json
{
  "version": "1.0.0",
  "name": "GraphQL 测试",
  "tests": [
    {
      "name": "获取用户查询",
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

## 断言类型

### 状态码

```json
{"type": "status", "expected": 200}
{"type": "status", "operator": "in", "expected": [200, 201]}
```

### 响应时间

```json
{"type": "responseTime", "operator": "<", "expected": 500}
```

### JSON 路径

```json
{"type": "jsonPath", "path": "$.data.id", "expected": 123}
{"type": "jsonPath", "path": "$.items.length()", "operator": ">", "expected": 0}
```

### 头部

```json
{"type": "header", "name": "Content-Type", "expected": "application/json"}
```

### 模式

```json
{"type": "schema", "schemaFile": "schemas/user.json"}
```

## 环境配置

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

## 认证

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

## 性能测试

### 负载测试

```bash
node main.js benchmark \
  --collection load-tests.json \
  --type load \
  --rps 100 \
  --duration 60s
```

### 压力测试

```bash
node main.js benchmark \
  --collection stress-tests.json \
  --type stress \
  --users 500 \
  --ramp-up 30s
```

## 模拟服务器

### 模拟配置

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
          "name": "模拟用户",
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

## API 参考

### TestRunner 类

```javascript
class TestRunner {
  constructor(config) {
    this.config = config;
  }

  async runCollection(collection, env) {
    /** 执行测试集合 */
  }

  async executeTest(test, context) {
    /** 执行单个测试 */
  }

  async evaluateAssertions(response, assertions) {
    /** 评估断言 */
  }
}
```

### Validator 类

```javascript
class SchemaValidator {
  constructor(schema) {
    this.schema = schema;
  }

  validate(response) {
    /** 根据模式验证响应 */
  }

  getErrors() {
    /** 返回验证错误 */
  }
}
```

## 输出格式

### 测试结果

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
      "name": "按 ID 获取用户",
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

## 错误处理

```json
{
  "status": "error",
  "error_code": "REQUEST_FAILED",
  "message": "请求超时",
  "details": {
    "url": "https://api.example.com/users/12345",
    "timeout": 30000
  },
  "suggestion": "增加超时时间或检查网络连接"
}
```

## 示例

### 带报告运行测试

```bash
node main.js run \
  --collection api-tests.json \
  --env staging \
  --report report.html \
  --format html
```

### 按标签过滤测试

```bash
node main.js run \
  --collection api-tests.json \
  --env staging \
  --filter "smoke"
```

### 并行执行

```bash
node main.js run \
  --collection api-tests.json \
  --env staging \
  --parallel 5
```

## CI/CD 集成

### GitHub Actions

```yaml
- name: 运行 API 测试
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

## 测试

```bash
# 运行单元测试
npm test

# 运行集成测试
npm run test:integration

# 带覆盖率运行
npm run test:coverage
```

## 最佳实践

1. 对机密使用环境变量
2. 按功能/端点组织测试
3. 包含描述性的测试名称
4. 设置适当的超时时间
5. 正确处理认证
6. 验证响应模式
7. 测试错误场景
