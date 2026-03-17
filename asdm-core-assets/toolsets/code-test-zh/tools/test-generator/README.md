# 测试生成器

自动测试用例生成引擎，分析源代码并生成全面的测试套件。

## 概述

测试生成器工具使用 AST（抽象语法树）解析分析源代码，并根据代码结构、函数签名和识别的模式生成测试用例。

## 特性

- **多语言支持**：JavaScript、TypeScript、Python、Java、C#
- **多框架支持**：Jest、Mocha、PyTest、JUnit、xUnit
- **智能分析**：基于 AST 的代码解析
- **边缘情况检测**：自动识别边界条件
- **模拟生成**：自动创建依赖的模拟
- **覆盖率优化**：生成测试以满足覆盖率目标

## 安装

```bash
# 安装依赖
pip install -r requirements.txt

# 或 Node.js 版本
npm install
```

## 用法

### 命令行

```bash
# 生成单元测试
python main.py --file src/utils.js --type unit --framework jest

# 生成集成测试
python main.py --file src/api/ --type integration --output tests/

# 仅分析代码
python main.py analyze --file src/utils.js --output analysis.json

# 带覆盖率目标生成
python main.py --file src/ --coverage 90 --output tests/
```

### 参数

| 参数 | 简写 | 描述 | 必需 | 默认值 |
|-----------|-------|-------------|----------|---------|
| `--file` | `-f` | 源文件或目录 | 是 | - |
| `--type` | `-t` | 测试类型（unit、integration、e2e） | 是 | - |
| `--framework` | | 测试框架 | 否 | jest |
| `--output` | `-o` | 输出目录 | 否 | ./tests |
| `--coverage` | | 覆盖率目标百分比 | 否 | 80 |
| `--overwrite` | | 覆盖现有测试 | 否 | false |
| `--template` | | 自定义模板文件 | 否 | default |
| `--exclude` | | 排除的模式 | 否 | - |
| `--include-private` | | 包含私有方法 | 否 | false |
| `--mock-external` | | 自动模拟外部依赖 | 否 | true |

## 配置

### config.json

```json
{
  "version": "1.0.0",
  "defaultFramework": "jest",
  "coverageTarget": 80,
  "generateMocks": true,
  "includePrivate": false,
  "namingConvention": {
    "pattern": "{函数名}_应该_{行为}_当_{条件}",
    "separator": "_"
  },
  "templates": {
    "unit": "templates/unit-test.hbs",
    "integration": "templates/integration-test.hbs",
    "e2e": "templates/e2e-test.hbs"
  },
  "exclusions": [
    "node_modules",
    "dist",
    "build",
    "*.config.js"
  ],
  "mockConfig": {
    "auto": true,
    "exclude": ["fs", "path", "crypto"]
  }
}
```

## 测试生成流程

### 1. 代码分析阶段

```python
def analyze_code(file_path: str) -> AnalysisResult:
    """
    分析源代码并提取：
    - 函数签名
    - 参数类型
    - 返回类型
    - 依赖关系
    - 控制流
    """
    pass
```

### 2. 测试计划阶段

```python
def generate_test_plan(analysis: AnalysisResult) -> TestPlan:
    """
    生成测试场景：
    - 正常路径测试
    - 边缘情况
    - 错误场景
    - 性能测试
    """
    pass
```

### 3. 测试生成阶段

```python
def generate_tests(plan: TestPlan, template: str) -> List[TestFile]:
    """
    从计划生成测试代码：
    - 应用模板
    - 生成测试数据
    - 创建模拟
    - 添加断言
    """
    pass
```

## 输出结构

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

## 测试模板

### 单元测试模板

```javascript
/**
 * 为 {{sourceFile}} 生成的测试
 * 生成时间：{{timestamp}}
 */

import { {{functions}} } from '{{sourcePath}}';
import { {{mocks}} } from '../helpers/mocks';

describe('{{moduleName}}', () => {
  beforeEach(() => {
    {{#each mockSetup}}
    {{this}}
    {{/each}}
  });

  afterEach(() => {
    {{#each mockTeardown}}
    {{this}}
    {{/each}}
  });

  {{#each testCases}}
  describe('{{functionName}}', () => {
    it('{{testName}}', () => {
      // 准备
      {{#each arrange}}
      {{this}}
      {{/each}}

      // 执行
      {{act}}

      // 断言
      {{#each assert}}
      {{this}}
      {{/each}}
    });
  });

  {{/each}}
});
```

## 测试用例类别

### 正常路径测试
- 正常输入
- 预期输出
- 标准工作流

### 边缘情况
- 空输入
- 边界值
- Null/undefined
- 最大值
- 特殊字符

### 错误场景
- 无效输入
- 缺少依赖
- 网络错误
- 权限被拒绝

### 性能测试
- 大数据集
- 并发访问
- 内存约束

## 模拟生成

### 函数模拟

```javascript
// 生成的模拟
jest.mock('../services/api', () => ({
  fetchUser: jest.fn().mockResolvedValue({ id: 1, name: '测试用户' }),
  createUser: jest.fn().mockResolvedValue({ id: 2 }),
}));
```

### 模块模拟

```javascript
jest.mock('axios', () => ({
  get: jest.fn(),
  post: jest.fn(),
}));
```

## 代码覆盖率

### 覆盖率分析

```python
def calculate_coverage(source_file: str, test_file: str) -> CoverageResult:
    """
    计算覆盖率指标：
    - 行覆盖率
    - 分支覆盖率
    - 函数覆盖率
    - 语句覆盖率
    """
    pass
```

### 覆盖率报告

```json
{
  "source": "src/utils.js",
  "test": "tests/utils.test.js",
  "coverage": {
    "lines": {"total": 50, "covered": 45, "percentage": 90},
    "branches": {"total": 20, "covered": 18, "percentage": 90},
    "functions": {"total": 10, "covered": 10, "percentage": 100},
    "statements": {"total": 55, "covered": 50, "percentage": 90.9}
  }
}
```

## 支持的语言

### JavaScript/TypeScript
- ES6+ 语法
- TypeScript 类型
- JSX/TSX 组件
- Async/await

### Python
- Python 3.8+
- 类型提示
- 异步函数
- 装饰器

### Java
- Java 8+
- 注解
- 泛型
- 流

### C#
- C# 8.0+
- LINQ
- Async/await
- 记录

## API 参考

### 分析器类

```python
class CodeAnalyzer:
    def __init__(self, language: str):
        self.language = language
    
    def parse(self, code: str) -> AST:
        """将代码解析为 AST"""
        pass
    
    def extract_functions(self, ast: AST) -> List[Function]:
        """提取函数定义"""
        pass
    
    def infer_types(self, function: Function) -> TypeSignature:
        """推断参数和返回类型"""
        pass
```

### 生成器类

```python
class TestGenerator:
    def __init__(self, framework: str, template: str):
        self.framework = framework
        self.template = template
    
    def generate(self, analysis: AnalysisResult) -> List[TestFile]:
        """生成测试文件"""
        pass
    
    def apply_template(self, test_case: TestCase) -> str:
        """将模板应用于测试用例"""
        pass
```

## 示例

### 为服务生成测试

```bash
python main.py \
  --file src/services/auth.js \
  --type unit \
  --framework jest \
  --coverage 90 \
  --output tests/unit/services/
```

### 生成集成测试

```bash
python main.py \
  --file src/api/ \
  --type integration \
  --mock-external \
  --output tests/integration/
```

### 分析代码结构

```bash
python main.py analyze \
  --file src/utils.js \
  --output analysis.json
```

## 错误处理

```json
{
  "status": "error",
  "error_code": "PARSE_ERROR",
  "message": "无法解析源文件",
  "details": {
    "file": "src/utils.js",
    "line": 42,
    "reason": "意外的标记"
  },
  "suggestion": "在生成测试之前修复语法错误"
}
```

## 测试

```bash
# 运行单元测试
pytest tests/unit/

# 运行集成测试
pytest tests/integration/

# 带覆盖率运行
pytest --cov=lib tests/
```

## 最佳实践

1. 生成前运行分析
2. 审查生成的测试
3. 根据需要自定义
4. 随时间维护测试
5. 设定现实的覆盖率目标
6. 使用有意义的测试名称
7. 包含边缘情况
