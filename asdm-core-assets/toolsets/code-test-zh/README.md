# CodeTest 工具集

一个全面的测试工具集，用于自动化测试用例生成、API 测试、UI 测试和测试报告生成。

## 概述

CodeTest 工具集提供完整的测试解决方案，与 AI 编码工具集成，自动化测试生命周期。支持多种测试范式，包括单元测试、集成测试、API 测试和 UI 测试。

## 特性

### 1. 测试用例生成
基于代码分析和规范自动生成全面的测试用例：
- 函数和方法的单元测试生成
- 集成测试场景
- 边缘情况检测和覆盖
- 测试数据生成

### 2. API 测试
生成和执行 API 测试，提供完整的请求/响应验证：
- RESTful API 测试
- GraphQL API 支持
- 认证测试
- 性能基准测试
- 模式验证

### 3. UI 测试
Web 应用程序的自动化 UI 测试：
- 端到端测试工作流
- 视觉回归测试
- 跨浏览器兼容性
- 可访问性测试
- 响应式设计验证

### 4. 测试报告
全面的测试报告和分析：
- 测试执行摘要
- 覆盖率分析
- 趋势可视化
- CI/CD 集成
- 导出多种格式（HTML、JSON、JUnit XML）

## 组件

### 动作（Actions）
动作转换为 AI 编码工具的斜杠命令：

| 动作 | 命令 | 描述 |
|--------|---------|-------------|
| `generate-test-cases` | `/generate-test-cases` | 从代码生成测试用例 |
| `api-test` | `/api-test` | 创建并运行 API 测试 |
| `ui-test` | `/ui-test` | 执行 UI 测试 |
| `test-report` | `/test-report` | 生成测试报告 |

### 规范（Specifications）
每个动作的详细规范位于 `specs/` 目录：
- [测试用例生成规范](specs/specs4generate-test-cases.md)
- [API 测试规范](specs/specs4api-test.md)
- [UI 测试规范](specs/specs4ui-test.md)
- [测试报告规范](specs/specs4test-report.md)

### 工具（Tools）
CLI 环境的实用工具位于 `tools/` 目录：
- `test-generator/` - 测试用例生成引擎
- `api-tester/` - API 测试框架
- `ui-tester/` - UI 测试自动化
- `report-generator/` - 报告生成工具

## 快速开始

1. **为文件生成测试用例：**
   ```
   /generate-test-cases --file src/utils.js --type unit
   ```

2. **运行 API 测试：**
   ```
   /api-test run --collection api-tests.json --env staging
   ```

3. **执行 UI 测试：**
   ```
   /ui-test run --suite e2e --browser chrome
   ```

4. **生成测试报告：**
   ```
   /test-report generate --format html --output reports/
   ```

## 安装

详细安装说明请参阅 [INSTALL.md](INSTALL.md)。

## 支持的技术

### 测试框架
- Jest、Mocha、Vitest（JavaScript/TypeScript）
- PyTest、unittest（Python）
- JUnit、TestNG（Java）
- xUnit、NUnit（.NET）

### API 测试
- REST 客户端
- GraphQL
- OpenAPI/Swagger
- Postman 集合

### UI 测试
- Playwright
- Selenium
- Cypress
- Puppeteer

### 报告
- Allure
- Mochawesome
- HTML 报告
- JUnit XML

## 最佳实践

1. **测试覆盖率**：目标是至少 80% 的代码覆盖率
2. **测试隔离**：每个测试应该相互独立
3. **描述性名称**：使用清晰的测试用例命名
4. **断言**：包含有意义的断言
5. **清理**：正确清理测试资源

## 配置

配置文件位于 `~/.asdm/toolsets/codetest-toolset/`：
- `config.json` - 主配置
- `templates/` - 测试模板
- `environments/` - 环境配置

## CI/CD 集成

该工具集支持与流行的 CI/CD 平台集成：
- GitHub Actions
- GitLab CI
- Jenkins
- Azure DevOps
- CircleCI

## 贡献

添加新测试工具时：
1. 遵循现有的目录结构
2. 包含全面的文档
3. 为新功能添加单元测试
4. 更新相关规范

## 许可证

本工具集是 ASDM 生态系统的一部分。
