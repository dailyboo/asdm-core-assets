# API 测试

## 描述
生成并执行全面的 API 测试，包括 REST、GraphQL 和 WebSocket 端点，提供完整的请求/响应验证。

## 用法
```
/api-test [command] [options]
```

## 命令
- `run`：执行 API 测试套件
- `generate`：生成 API 测试用例
- `validate`：根据模式验证 API 响应
- `benchmark`：运行性能基准测试
- `mock`：启动模拟 API 服务器

## 参数

### 必需参数
- `--collection`, `-c`：测试集合文件或目录
- `--env`, `-e`：环境配置（dev、staging、prod）

### 可选参数
- `--parallel`：并行请求数（默认：1）
- `--timeout`：请求超时时间（毫秒）（默认：30000）
- `--retry`：失败请求的重试次数（默认：0）
- `--report`：输出报告文件路径
- `--format`：报告格式（json、html、junit）
- `--filter`：按标签或名称过滤测试
- `--variables`：环境变量 JSON 文件
- `--delay`：请求之间的延迟（毫秒）
- `--stop-on-failure`：首次失败时停止执行
- `--save-response`：将响应体保存到文件
- `--schema`：用于验证的 OpenAPI/Swagger 模式文件

## 示例

### 运行 API 测试
```
/api-test run --collection api-tests.json --env staging
```

### 从 OpenAPI 规范生成测试
```
/api-test generate --schema api-spec.yaml --output tests/api/
```

### 并行执行测试
```
/api-test run --collection api-tests/ --parallel 5 --env prod
```

### 根据模式验证响应
```
/api-test validate --collection api-tests.json --schema api-spec.yaml
```

### 运行性能基准测试
```
/api-test benchmark --collection load-tests.json --iterations 100
```

### 启动模拟服务器
```
/api-test mock --port 3000 --responses mock-data.json
```

## 测试集合格式

### REST API 测试
```json
{
  "name": "用户 API 测试",
  "tests": [
    {
      "name": "按 ID 获取用户",
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

### GraphQL 测试
```json
{
  "name": "GraphQL 查询测试",
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

## 断言类型

### 状态码
```json
{"type": "status", "value": 200}
{"type": "status", "operator": "in", "value": [200, 201]}
```

### 响应时间
```json
{"type": "responseTime", "operator": "<", "value": 500}
```

### JSON 路径
```json
{"type": "jsonPath", "path": "$.data.id", "value": 123}
{"type": "jsonPath", "path": "$.items.length()", "operator": ">", "value": 0}
```

### 头部
```json
{"type": "header", "name": "Content-Type", "value": "application/json"}
```

### 正文包含
```json
{"type": "bodyContains", "value": "success"}
```

### 模式验证
```json
{"type": "schema", "schemaFile": "schemas/user.json"}
```

## 环境配置

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

## 认证支持

### Bearer Token
```
/api-test run --auth bearer --token $API_TOKEN
```

### Basic Auth
```
/api-test run --auth basic --username user --password pass
```

### OAuth 2.0
```
/api-test run --auth oauth2 --client-id $CLIENT_ID --client-secret $SECRET
```

### API Key
```
/api-test run --auth api-key --key X-API-Key --value $API_KEY
```

## 相关规范
详细规范请参阅 [specs4api-test.md](../specs/specs4api-test.md)。

## 输出格式

### 测试执行报告
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
      "name": "按 ID 获取用户",
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

## 性能测试

### 负载测试
```
/api-test benchmark --type load --rps 100 --duration 60s
```

### 压力测试
```
/api-test benchmark --type stress --users 500 --ramp-up 30s
```

### 峰值测试
```
/api-test benchmark --type spike --peak-rps 1000 --duration 10s
```

## 最佳实践

1. **环境变量**：对敏感数据使用环境变量
2. **测试隔离**：每个测试应该相互独立
3. **清理**：执行后清理测试数据
4. **断言**：具体明确的断言
5. **错误消息**：包含有意义的描述
6. **重试逻辑**：为不稳定的端点实现重试

## CI/CD 集成

### GitHub Actions
```yaml
- name: 运行 API 测试
  run: /api-test run --collection api-tests.json --env ${{ matrix.env }}
```

### GitLab CI
```yaml
api_test:
  script:
    - /api-test run --collection api-tests.json --report report.json
  artifacts:
    reports:
      junit: report.json
```
