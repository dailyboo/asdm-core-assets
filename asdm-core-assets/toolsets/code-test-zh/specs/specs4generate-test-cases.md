# 生成测试用例规范

## 目的
本文档提供生成测试用例动作的详细规范，定义测试生成策略、覆盖率需求和输出标准。

## 架构

### 测试生成流水线
1. **代码分析阶段**
   - 解析源代码（AST 分析）
   - 提取函数/方法签名
   - 识别依赖关系
   - 检测代码模式

2. **测试计划阶段**
   - 生成测试场景
   - 识别边缘情况
   - 映射覆盖率需求
   - 优先排序测试用例

3. **测试生成阶段**
   - 生成测试代码
   - 创建测试数据
   - 设置模拟/桩
   - 添加断言

4. **优化阶段**
   - 移除冗余测试
   - 优化测试执行
   - 验证覆盖率
   - 生成报告

## 功能需求

### 代码分析

#### 支持的语言
| 语言 | 文件扩展名 | 框架 |
|----------|----------------|------------|
| JavaScript | .js, .jsx | Jest, Mocha, Vitest |
| TypeScript | .ts, .tsx | Jest, Vitest |
| Python | .py | PyTest, unittest |
| Java | .java | JUnit, TestNG |
| C# | .cs | xUnit, NUnit |

#### 分析能力
- 函数/方法提取
- 参数类型推断
- 返回类型分析
- 依赖映射
- 控制流分析
- 异常处理检测

### 测试用例类别

#### 1. 单元测试
**目的**：测试单个函数/方法

**生成规则**：
- 每个源文件一个测试文件
- 测试所有公共方法
- 包含正向和负向用例
- 模拟外部依赖
- 关键路径的目标分支覆盖率为 100%

**命名约定**：
```
{函数名}_应该_{预期行为}_当_{条件}
```

**示例**：
```javascript
describe('calculateTotal', () => {
  it('应该返回正确的总和_当给定有效项目', () => {
    const items = [{ price: 10 }, { price: 20 }];
    expect(calculateTotal(items)).toBe(30);
  });
});
```

#### 2. 集成测试
**目的**：测试组件交互

**生成规则**：
- 测试数据库操作
- 测试 API 交互
- 测试服务集成
- 包含事务场景
- 验证数据一致性

**命名约定**：
```
{组件}_{集成类型}_应该_{预期行为}
```

#### 3. 边缘情况测试
**目的**：测试边界条件和异常输入

**边缘情况类型**：
- 空输入：`[]`、`''`、`null`、`undefined`
- 边界值：最小、最大值
- 无效类型：错误的数据类型
- 特殊字符：unicode、转义序列
- 大数据：性能场景
- 并发访问：竞态条件

### 测试数据生成

#### 原始类型
| 类型 | 测试值 |
|------|-------------|
| Number | 0, 1, -1, MAX_VALUE, MIN_VALUE, NaN, Infinity |
| String | '', 'a', 'valid string', '<script>alert(1)</script>' |
| Boolean | true, false |
| Array | [], [1], [1,2,3], new Array(1000) |
| Object | {}, {key: 'value'}, nested objects |
| Null | null, undefined |

#### 自定义类型
- 根据类型定义生成
- 遵守验证规则
- 包含约束违规

### 覆盖率需求

#### 覆盖率目标
| 指标 | 最低 | 目标 |
|--------|---------|--------|
| 行覆盖率 | 70% | 85% |
| 分支覆盖率 | 60% | 80% |
| 函数覆盖率 | 80% | 95% |
| 语句覆盖率 | 70% | 85% |

#### 覆盖率排除
- 生成的代码
- 第三方库
- 配置文件
- 类型定义

### 模拟生成

#### 模拟类型
1. **函数模拟**：替换函数实现
2. **模块模拟**：模拟整个模块
3. **API 模拟**：模拟 HTTP 请求
4. **数据库模拟**：模拟数据库操作
5. **时间模拟**：模拟定时器和日期

#### 模拟配置
```javascript
const mockConfig = {
  functionName: {
    returns: '模拟值',
    throws: null,
    calls: 1
  }
};
```

## 输出规范

### 测试文件结构
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

### 测试文件模板
```javascript
/**
 * 为 {源文件} 生成的测试
 * 生成时间：{时间戳}
 * 覆盖率目标：{覆盖率}%
 */

import { describe, it, expect, beforeEach, afterEach } from '{框架}';
import { {函数} } from '{源路径}';
import { mockSetup, mockTeardown } from '../helpers/mocks';

describe('{模块名}', () => {
  beforeEach(() => {
    mockSetup();
  });

  afterEach(() => {
    mockTeardown();
  });

  describe('{函数名}', () => {
    it('应该为有效输入返回预期结果', () => {
      // 准备
      const input = {valid: 'data'};
      const expected = {result: 'expected'};

      // 执行
      const result = {函数名}(input);

      // 断言
      expect(result).toEqual(expected);
    });

    // ... 其他测试
  });
});
```

### 测试元数据
```json
{
  "sourceFile": "src/services/auth.js",
  "testFile": "tests/unit/services/auth.test.js",
  "generatedAt": "2024-01-15T10:30:00Z",
  "functions": ["login", "logout", "validateToken"],
  "coverage": {
    "target": 85,
    "current": 0
  },
  "dependencies": {
    "mocked": ["database", "logger"],
    "tested": ["crypto"]
  }
}
```

## 非功能需求

### 性能
- 分析：每个文件 < 5 秒
- 生成：每个文件 < 10 秒
- 内存：每个进程 < 500MB

### 质量
- 生成的测试应该可执行
- 输出中无语法错误
- 遵循代码风格指南
- 包含有意义的断言

### 可扩展性
- 支持自定义模板
- 允许框架扩展
- 新语言的插件架构

## 错误处理

### 错误代码
| 代码 | 描述 | 恢复 |
|------|-------------|----------|
| PARSE_ERROR | 无法解析源文件 | 修复语法错误 |
| TYPE_ERROR | 无法推断类型 | 添加类型注解 |
| DEPENDENCY_ERROR | 缺少依赖 | 安装依赖 |
| GENERATION_ERROR | 测试生成失败 | 检查日志 |
| VALIDATION_ERROR | 生成的测试无效 | 报告问题 |

### 错误响应
```json
{
  "status": "error",
  "error_code": "PARSE_ERROR",
  "message": "无法解析源文件",
  "details": {
    "file": "src/utils.js",
    "line": 42,
    "column": 15,
    "reason": "意外的标记"
  },
  "suggestion": "修复第 42 行的语法错误"
}
```

## 测试需求

### 单元测试
- 测试代码解析器
- 测试 AST 分析器
- 测试模板引擎
- 测试模拟生成器

### 集成测试
- 测试端到端生成
- 使用真实代码库测试
- 测试覆盖率计算
- 测试报告生成

## 配置

### 配置文件
```json
{
  "language": "javascript",
  "framework": "jest",
  "coverage": {
    "target": 85,
    "exclude": ["node_modules", "dist"]
  },
  "templates": {
    "unit": "templates/unit-test.hbs",
    "integration": "templates/integration-test.hbs"
  },
  "mocks": {
    "auto": true,
    "exclude": ["crypto", "fs"]
  }
}
```

## 约束
- 必须支持增量生成
- 必须保留手动测试修改
- 必须处理循环依赖
- 必须支持 monorepo 结构
