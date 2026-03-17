# 生成测试用例

## 描述
自动为源代码生成全面的测试用例，包括单元测试、集成测试和边缘情况场景。

## 用法
```
/generate-test-cases [options]
```

## 命令
- `analyze`：分析代码并生成测试计划
- `generate`：生成测试用例
- `update`：更新现有测试用例

## 参数

### 必需参数
- `--file`, `-f`：要生成测试的目标文件或目录
- `--type`, `-t`：要生成的测试类型（unit、integration、e2e）

### 可选参数
- `--framework`：要使用的测试框架（jest、mocha、pytest、junit）
- `--output`, `-o`：生成测试的输出目录
- `--coverage`：目标覆盖率百分比（默认：80）
- `--overwrite`：覆盖现有测试文件
- `--template`：测试生成的自定义模板文件
- `--exclude`：从测试生成中排除的模式
- `--include-private`：在测试生成中包含私有方法
- `--mock-external`：自动模拟外部依赖

## 示例

### 基本单元测试生成
```
/generate-test-cases --file src/utils.js --type unit
```

### 使用特定框架生成集成测试
```
/generate-test-cases --file src/api/ --type integration --framework jest
```

### 带覆盖率目标生成测试
```
/generate-test-cases --file src/services/ --type unit --coverage 90 --output tests/
```

### 分析并生成测试计划
```
/generate-test-cases analyze --file src/ --output test-plan.json
```

### 更新现有测试
```
/generate-test-cases update --file src/utils.js --test-file tests/utils.test.js
```

## 测试生成策略

### 单元测试
- 函数/方法级别测试
- 输入验证场景
- 边界值分析
- 错误处理路径
- 模拟外部依赖

### 集成测试
- 模块交互测试
- 数据库操作
- API 端点
- 服务集成
- 事件处理

### E2E 测试
- 用户工作流场景
- 关键业务路径
- 跨组件交互
- 系统级验证

## 输出结构

生成的测试遵循标准结构：

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

## 测试用例类别

### 1. 正常路径测试
- 正常输入场景
- 预期的成功结果
- 标准工作流完成

### 2. 边缘情况
- 边界值
- 空输入
- 最大限制
- Null/undefined 处理

### 3. 错误场景
- 无效输入
- 缺少依赖
- 网络故障
- 权限错误

### 4. 性能测试
- 大数据集
- 并发操作
- 内存约束
- 超时场景

## 相关规范
详细规范请参阅 [specs4generate-test-cases.md](../specs/specs4generate-test-cases.md)。

## 输出格式

### 成功响应
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
    "考虑为 null 输入添加边缘情况测试",
    "增加错误处理路径的覆盖率"
  ]
}
```

### 错误响应
```json
{
  "status": "error",
  "error_code": "ANALYSIS_FAILED",
  "message": "无法分析源文件",
  "details": {
    "file": "src/utils.js",
    "reason": "源文件中有语法错误"
  },
  "suggestion": "在生成测试之前修复语法错误"
}
```

## 最佳实践

1. **先运行分析**：使用 `analyze` 命令了解代码结构
2. **审查生成的测试**：始终审查和自定义生成的测试
3. **增量生成**：从关键组件开始
4. **维护测试**：源代码更改时更新测试
5. **覆盖率目标**：设定现实的覆盖率目标

## 与 IDE 集成

生成的测试可以：
- 在 IDE 中直接执行
- 与测试运行器集成
- 添加到 CI/CD 流水线
- 用于调试

## 限制

- 复杂的动态代码可能需要手动测试设计
- 外部 API 依赖需要模拟配置
- UI 组件可能需要额外设置
- 性能测试需要环境特定的调优
