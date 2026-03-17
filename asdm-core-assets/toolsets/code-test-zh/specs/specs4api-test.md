# API 测试规范

## 目的
本文档提供 API 测试动作的详细规范，定义 API 测试的测试执行、验证和报告标准。

## 架构

### API 测试框架
```
┌─────────────────────────────────────────────────────────┐
│                    API 测试运行器                         │
├─────────────────────────────────────────────────────────┤
│  集合加载器  │  环境管理器  │  执行器      │
├─────────────────────────────────────────────────────────┤
│  请求构建器    │  响应验证器   │  报告器     │
├─────────────────────────────────────────────────────────┤
│  认证处理器       │  模式验证器     │  日志器       │
└─────────────────────────────────────────────────────────┘
```

## 功能需求

### 测试集合格式

#### REST API 集合
```json
{
  "version": "1.0",
  "name": "API 测试集合",
  "variables": {
    "baseUrl": "{{env.BASE_URL}}",
    "token": "{{env.AUTH_TOKEN}}"
  },
  "tests": [
    {
      "id": "test-001",
      "name": "按 ID 获取用户",
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
          "description": "应该返回 200 OK"
        },
        {
          "type": "responseTime",
          "operator": "<",
          "expected": 500,
          "description": "响应时间在 500ms 以内"
        },
        {
          "type": "jsonPath",
          "path": "$.data.id",
          "expected": "{{userId}}",
          "description": "应该返回正确的用户 ID"
        }
      ],
      "postRequest": {
        "script": "pm.collectionVariables.set('extractedId', response.data.id);"
      }
    }
  ]
}
```

#### GraphQL 集合
```json
{
  "version": "1.0",
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

### 断言类型

#### 1. 状态码断言
```json
{
  "type": "status",
  "expected": 200,
  "description": "状态码应该是 200"
}
```

支持的运算符：
- `equals`（默认）
- `in` - 状态码在列表中
- `notIn` - 状态码不在列表中

#### 2. 响应时间断言
```json
{
  "type": "responseTime",
  "operator": "<",
  "expected": 500,
  "unit": "ms"
}
```

支持的运算符：
- `<` - 小于
- `<=` - 小于或等于
- `>` - 大于
- `>=` - 大于或等于

#### 3. JSON 路径断言
```json
{
  "type": "jsonPath",
  "path": "$.data.items.length()",
  "operator": ">",
  "expected": 0
}
```

JSON 路径语法：
- `$.property` - 根属性
- `$.[*]` - 所有数组项
- `$..property` - 递归下降

#### 4. 头部断言
```json
{
  "type": "header",
  "name": "Content-Type",
  "expected": "application/json"
}
```

#### 5. 正文断言
```json
{
  "type": "bodyContains",
  "expected": "success"
}
```

#### 6. 模式断言
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

### 认证

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

### 环境管理

#### 环境文件
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

### 请求执行

#### 执行流程
1. 加载测试集合
2. 解析环境变量
3. 应用认证
4. 构建请求
5. 执行请求
6. 捕获响应
7. 运行断言
8. 存储结果
9. 执行后请求脚本

#### 并行执行
```json
{
  "execution": {
    "parallel": true,
    "workers": 5,
    "strategy": "bySuite"
  }
}
```

#### 重试配置
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

### 性能测试

#### 负载测试配置
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

#### 压力测试配置
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

## 非功能需求

### 性能
- 单个测试执行：< 30 秒
- 集合执行：< 5 分钟
- 内存使用：< 每个工作进程 200MB
- CPU 使用：负载测试期间 < 50%

### 可靠性
- 优雅处理网络超时
- 支持离线测试设计
- 执行前验证请求
- 提供详细的错误消息

### 安全性
- 安全存储凭证
- 支持环境变量注入
- 在日志中屏蔽敏感数据
- 支持基于证书的认证

## 输出规范

### 测试结果格式
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
      "name": "按 ID 获取用户",
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

### 错误响应格式
```json
{
  "testId": "test-002",
  "name": "创建用户",
  "status": "failed",
  "error": {
    "code": "ASSERTION_FAILED",
    "message": "状态码断言失败",
    "details": {
      "expected": 201,
      "actual": 400,
      "response": {
        "error": "验证失败",
        "fields": ["email"]
      }
    }
  }
}
```

## 模拟服务器

### 模拟配置
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
            "name": "模拟用户"
          }
        }
      }
    ]
  }
}
```

## 测试需求

### 单元测试
- 测试断言评估器
- 测试变量解析器
- 测试认证处理器
- 测试请求构建器

### 集成测试
- 测试端到端执行
- 使用真实 API 测试
- 测试错误场景
- 测试并行执行

## 配置模式

### 主配置
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

## CI/CD 集成

### GitHub Actions 工作流
```yaml
name: API 测试
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: 运行 API 测试
        run: /api-test run --collection api-tests.json --env staging
        env:
          API_TOKEN: ${{ secrets.API_TOKEN }}
```

## 约束
- 最大请求大小：10MB
- 最大响应大小：100MB
- 最大并行工作进程：20
- 最大测试持续时间：1 小时
