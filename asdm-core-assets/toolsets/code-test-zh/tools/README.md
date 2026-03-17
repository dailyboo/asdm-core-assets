# 工具目录

本目录包含支持 CodeTest 工具集功能的实用工具。这些工具设计用于在 CLI 环境中由 AI 代理调用。

## 可用工具

### 1. test-generator
自动测试用例生成引擎，分析源代码并生成全面的测试套件。

**功能：**
- 基于 AST 的代码分析
- 多框架支持（Jest、Mocha、PyTest、JUnit）
- 边缘情况检测
- 模拟生成
- 覆盖率优化

[查看文档](./test-generator/README.md)

### 2. api-tester
用于 REST、GraphQL 和 WebSocket 端点的 API 测试框架。

**功能：**
- 请求执行和验证
- 模式验证（OpenAPI、GraphQL）
- 性能基准测试
- 模拟服务器生成
- 环境管理

[查看文档](./api-tester/README.md)

### 3. ui-tester
Web 应用程序的 UI 测试自动化框架。

**功能：**
- 多浏览器测试
- 视觉回归测试
- 可访问性测试
- 移动设备模拟
- 录制和回放

[查看文档](./ui-tester/README.md)

### 4. report-generator
测试报告生成和分析引擎。

**功能：**
- 多格式输出（HTML、JSON、JUnit、Allure）
- 趋势分析
- 不稳定测试检测
- 覆盖率集成
- 通知分发

[查看文档](./report-generator/README.md)

## 工具结构

每个工具遵循一致的结构：

```
{工具名称}/
├── README.md           # 工具文档
├── main.py            # Python 入口点（或 Node.js 的 main.js）
├── config.json        # 默认配置
├── lib/               # 核心库代码
├── templates/         # 模板文件
└── tests/             # 工具测试套件
    ├── unit/
    └── integration/
```

## 用法

### 独立执行
每个工具可以独立执行：

```bash
# Python 工具
python test-generator/main.py --file src/utils.js --framework jest

# Node.js 工具
node api-tester/main.js run --collection api-tests.json
```

### 与动作集成
工具通常通过动作命令调用：

```
/generate-test-cases --file src/utils.js --type unit
/api-test run --collection api-tests.json --env staging
/ui-test run --suite tests/e2e/
/test-report generate --input results/ --format html
```

## 配置

### 全局配置
工具在 `~/.asdm/toolsets/codetest-toolset/config.json` 共享全局配置：

```json
{
  "version": "1.0.0",
  "logLevel": "info",
  "outputDir": "./test-output",
  "cacheDir": "./.cache",
  "templatesDir": "./templates"
}
```

### 工具特定配置
每个工具都有自己的配置文件用于专用设置：

```json
// test-generator/config.json
{
  "defaultFramework": "jest",
  "coverageTarget": 85,
  "generateMocks": true,
  "includePrivate": false
}
```

## 开发

### 添加新工具

1. 在 `tools/` 下创建新目录
2. 实现所需接口：
   ```python
   class Tool:
       def __init__(self, config: dict):
           self.config = config
       
       def execute(self, params: dict) -> dict:
           """使用给定参数执行工具"""
           pass
       
       def validate(self, params: dict) -> bool:
           """验证输入参数"""
           pass
   ```
3. 创建包含文档的 README.md
4. 添加单元测试和集成测试
5. 用新工具更新此 README.md

### 测试工具
```bash
# 运行所有工具测试
pytest tools/*/tests/

# 运行特定工具测试
pytest tools/test-generator/tests/

# 带覆盖率运行
pytest --cov=tools tools/*/tests/
```

## 依赖

### Python 工具
- Python 3.8+
- `requirements.txt` 中的必需包

### Node.js 工具
- Node.js 16+
- `package.json` 中的必需包

## 错误处理

所有工具遵循一致的错误处理模式：

```json
{
  "status": "error",
  "error_code": "ERROR_CODE",
  "message": "人类可读的错误消息",
  "details": {
    "additional": "context"
  },
  "suggestion": "建议的修复或操作"
}
```

## 日志记录

工具使用结构化日志：

```python
import logging

logger = logging.getLogger('codetest.tool')
logger.info("操作已开始", extra={"tool": "test-generator", "operation": "analyze"})
```

## 输出格式

所有工具返回一致的输出：

```json
{
  "status": "success",
  "data": {
    // 工具特定的结果
  },
  "metadata": {
    "duration": 1234,
    "timestamp": "2024-01-15T10:30:00Z"
  }
}
```

## 最佳实践

1. **自包含**：每个工具应该可独立执行
2. **单一职责**：每个工具有一个明确的目的
3. **可配置**：外部化配置
4. **可测试**：全面的测试覆盖
5. **有文档**：清晰的 README 和内联文档
6. **健壮**：优雅地处理错误
7. **高效**：优化性能

## 维护

### 更新工具
1. 对工具代码进行更改
2. 如需要更新测试
3. 更新文档
4. 运行完整的测试套件
5. 在 config.json 中更新版本

### 弃用工具
1. 在 README.md 中标记为已弃用
2. 在代码中添加弃用警告
3. 提供迁移路径
4. 设置移除日期
