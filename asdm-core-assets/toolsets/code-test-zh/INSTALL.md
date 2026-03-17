# CodeTest 工具集安装指南

toolset-id: code-test
toolset-name: CodeTest
version: 0.0.2
updated-date: 2026-03-16
toolset-description: 一个全面的工具集，用于自动化测试用例生成、API测试、UI测试和测试报告。

## 概述

本文档提供在工作空间中安装和设置 CodeTest 工具集的说明。CodeTest 提供全面的测试能力，包括自动化测试用例生成、API测试、UI测试和测试报告。

## AI 引导安装

要使用 AI 引导安装此工具集，请将以下提示复制并粘贴到您的 AI 编码工具的聊天窗口中：

```shell
按照 .asdm/toolsets/code-test-zh/INSTALL.md 中的说明操作
```

## 安装步骤

### 1. 检测当前的 `Agentic Engine` 提供商

检测当前的 AI 编码助手提供商（例如 Claude Code、GitHub Copilot、Tencent CodeBuddy）。使用以下准则检测提供商：

- 如果存在 `.claude` 目录，使用 `Claude Code`
- 如果存在 `.github` 目录，使用 `GitHub Copilot`
- 如果存在 `.codebuddy` 目录，使用 `Tencent CodeBuddy`
- 如果在当前工作空间中未找到此类文件夹，提示用户手动选择提供商

### 2. 在提供商的入口点为 CodeTest（工具集 ID：`code-test`）创建快捷命令

根据检测到的提供商在适当位置创建快捷命令。安装过程在所有提供商之间保持一致 - 我们使用 `cat` 将提供商特定的前置内容与实际指令内容连接起来：

#### 对于 Claude Code（`.claude/commands/`）：

Claude Code 使用带有 Frontmatter 元数据的 Markdown 文件作为斜杠命令。通过连接 Claude 特定的前置内容与指令内容来创建命令：

```bash
mkdir -p .claude/commands/

# 生成测试用例命令
cat > .claude/commands/generate-test-cases.md << 'EOF'
---
description: "从源代码生成测试用例"
argument-hint: "<文件或目录>"
---

EOF
cat .asdm/toolsets/code-test-zh/actions/generate-test-cases.md >> .claude/commands/generate-test-cases.md

# API 测试命令
cat > .claude/commands/api-test.md << 'EOF'
---
description: "生成并执行 API 测试"
argument-hint: "<命令> [选项]"
---

EOF
cat .asdm/toolsets/code-test-zh/actions/api-test.md >> .claude/commands/api-test.md

# UI 测试命令
cat > .claude/commands/ui-test.md << 'EOF'
---
description: "执行自动化 UI 测试"
argument-hint: "<命令> [选项]"
---

EOF
cat .asdm/toolsets/code-test-zh/actions/ui-test.md >> .claude/commands/ui-test.md

# 测试报告命令
cat > .claude/commands/test-report.md << 'EOF'
---
description: "生成带有分析的测试报告"
argument-hint: "<命令> [选项]"
---

EOF
cat .asdm/toolsets/code-test-zh/actions/test-report.md >> .claude/commands/test-report.md
```

#### 对于 GitHub Copilot（`.github/prompts/`）：

GitHub Copilot 使用带有 YAML 前置内容的 `.prompt.md` 文件。通过连接 GitHub 特定的前置内容与指令内容来创建提示文件：

```bash
mkdir -p .github/prompts/

# 生成测试用例提示
cat > .github/prompts/generate-test-cases.prompt.md << 'EOF'
---
agent: 'agent'
description: '从源代码生成测试用例'
argument-hint: '输入文件或目录路径'
---

EOF
cat .asdm/toolsets/code-test-zh/actions/generate-test-cases.md >> .github/prompts/generate-test-cases.prompt.md

# API 测试提示
cat > .github/prompts/api-test.prompt.md << 'EOF'
---
agent: 'agent'
description: '生成并执行 API 测试'
argument-hint: '输入命令和选项'
---

EOF
cat .asdm/toolsets/code-test-zh/actions/api-test.md >> .github/prompts/api-test.prompt.md

# UI 测试提示
cat > .github/prompts/ui-test.prompt.md << 'EOF'
---
agent: 'agent'
description: '执行自动化 UI 测试'
argument-hint: '输入命令和选项'
---

EOF
cat .asdm/toolsets/code-test-zh/actions/ui-test.md >> .github/prompts/ui-test.prompt.md

# 测试报告提示
cat > .github/prompts/test-report.prompt.md << 'EOF'
---
agent: 'agent'
description: '生成带有分析的测试报告'
argument-hint: '输入命令和选项'
---

EOF
cat .asdm/toolsets/code-test-zh/actions/test-report.md >> .github/prompts/test-report.prompt.md
```

#### 对于 Tencent CodeBuddy（`.codebuddy/commands/`）：

CodeBuddy 不支持前置内容，因此直接复制指令文件：

```bash
mkdir -p .codebuddy/commands/

# 直接复制指令文件（无需前置内容）
cp .asdm/toolsets/code-test-zh/actions/generate-test-cases.md .codebuddy/commands/
cp .asdm/toolsets/code-test-zh/actions/api-test.md .codebuddy/commands/
cp .asdm/toolsets/code-test-zh/actions/ui-test.md .codebuddy/commands/
cp .asdm/toolsets/code-test-zh/actions/test-report.md .codebuddy/commands/
```

### 3. 其他提供商的手动使用

如果您的 AI 编码助手提供商未被自动检测逻辑检测到（Claude Code、GitHub Copilot 或 Tencent CodeBuddy），您仍然可以手动使用 CodeTest。请按照以下步骤操作：

#### 直接使用指令文件

您可以通过复制指令文件的相对路径并将其粘贴到 AI 编码助手的聊天窗口中来直接使用它们：

1. **导航到指令文件**：
   ```bash
   cd .asdm/toolsets/code-test-zh/actions/
   ```

2. **右键点击所需的指令文件**并复制其相对路径：
   - 用于测试用例生成：`generate-test-cases.md`
   - 用于 API 测试：`api-test.md`
   - 用于 UI 测试：`ui-test.md`
   - 用于测试报告：`test-report.md`

3. **在 AI 编码助手中输入提示**：
   ```
   按照以下指令文件中的说明操作 {指令文件的相对路径}
   ```

## 初始化 CodeTest

### 生成测试用例

安装完成后，您可以为代码生成测试用例：

```shell
按照 .asdm/toolsets/code-test-zh/actions/generate-test-cases.md 中的说明操作
```

这将：
1. 分析源代码结构
2. 生成全面的测试用例
3. 创建具有适当结构的测试文件
4. 估算代码覆盖率

### 运行 API 测试

用于 API 测试：

```shell
按照 .asdm/toolsets/code-test-zh/actions/api-test.md 中的说明操作
```

这将：
1. 执行 API 测试套件
2. 根据 schema 验证响应
3. 运行性能基准测试
4. 生成测试报告

### 运行 UI 测试

用于 UI 测试：

```shell
按照 .asdm/toolsets/code-test-zh/actions/ui-test.md 中的说明操作
```

这将：
1. 执行自动化 UI 测试套件
2. 执行视觉回归测试
3. 测试无障碍合规性
4. 截取屏幕截图和视频

### 生成测试报告

用于全面的测试报告：

```shell
按照 .asdm/toolsets/code-test-zh/actions/test-report.md 中的说明操作
```

这将：
1. 从多个来源汇总测试结果
2. 生成交互式报告
3. 分析测试趋势随时间的变化
4. 提供可操作的见解

### 可用命令

安装完成后，您可以使用以下命令：

1. **`/generate-test-cases`** - 从源代码生成测试用例
2. **`/api-test`** - 生成并执行 API 测试
3. **`/ui-test`** - 执行自动化 UI 测试
4. **`/test-report`** - 生成带有分析的测试报告

## 验证

安装完成后，请验证：

1. CodeTest（工具集 ID：`code-test`）的快捷命令已在适当的提供商目录中创建（如果使用 Claude Code、GitHub Copilot 或 Tencent CodeBuddy）
2. CodeTest 工具集文件位于 `.asdm/toolsets/code-test-zh`

**对于其他提供商**：验证您可以访问以下指令文件：
- `.asdm/toolsets/code-test-zh/actions/generate-test-cases.md`
- `.asdm/toolsets/code-test-zh/actions/api-test.md`
- `.asdm/toolsets/code-test-zh/actions/ui-test.md`
- `.asdm/toolsets/code-test-zh/actions/test-report.md`

## 使用示例

### 测试生成工作流

```shell
# 首先，使用 AI 引导安装安装工具集
按照 .asdm/toolsets/code-test-zh/INSTALL.md 中的说明操作

# 生成测试用例
按照 .asdm/toolsets/code-test-zh/actions/generate-test-cases.md 中的说明操作

# 运行 API 测试
按照 .asdm/toolsets/code-test-zh/actions/api-test.md 中的说明操作

# 运行 UI 测试
按照 .asdm/toolsets/code-test-zh/actions/ui-test.md 中的说明操作

# 生成测试报告
按照 .asdm/toolsets/code-test-zh/actions/test-report.md 中的说明操作

# 使用斜杠命令时的示例提示：
/generate-test-cases --file src/utils.js --type unit
/api-test run --collection api-tests.json --env staging
/ui-test run --suite tests/e2e/ --browser chrome
/test-report generate --format html --output reports/
```

## 注意事项

- 此安装过程假设您拥有创建目录和文件的必要权限
- 命令的实际实现将由 AI 模型使用 CodeTest（工具集 ID：`code-test`）中提供的模板和说明来处理
- 确保根据您的实际 AI 编码助手自定义提供商特定的设置
- 工具集 ID `code-test` 应在命令和文档中引用 CodeTest 时一致使用
- **对于检测逻辑中未包含的提供商**：用户可以通过复制指令文件的相对路径并输入类似"按照 .asdm/toolsets/code-test-zh/actions/generate-test-cases.md 中的说明操作"的提示来手动使用指令文件

## 规范文档

工具集使用以下规范文档作为模板：

1. **`specs4generate-test-cases.md`** - 测试用例生成规范
2. **`specs4api-test.md`** - API 测试方法论规范
3. **`specs4ui-test.md`** - UI 测试流程规范
4. **`specs4test-report.md`** - 生成测试报告的模板

## 与其他工具集的集成

CodeTest 可以独立使用，也可以与其他工具集集成以支持完整的开发工作流：

- **基础工具**：一起用于提交工作流
- **上下文构建器**：利用项目上下文进行更好的测试生成
- **代码审查**：将测试结果集成到代码审查流程中
- **CI/CD 流水线**：作为部署工作流的一部分自动化测试

### 获取帮助

对于 CodeTest 工具集的问题，请参考：
- [ASDM 文档](https://asdm.ai/docs)
- 工具集 README：`.asdm/toolsets/code-test-zh/README.md`
- `.asdm/toolsets/code-test-zh/specs/` 中的规范文档

## 许可证

Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

根据专有软件许可证授权。有关许可证信息，请参阅项目根目录中的 [LICENSE](LICENSE)。

---

*此安装文档是 CodeTest 工具集的一部分。使用这些操作可以执行全面的测试任务，包括测试用例生成、API 测试、UI 测试和测试报告。*
