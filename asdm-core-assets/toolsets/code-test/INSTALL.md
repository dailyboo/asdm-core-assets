# Installation Guide for CodeTest Toolset

toolset-id: code-test
toolset-name: CodeTest
version: 0.0.2
updated-date: 2026-03-16
toolset-description: A comprehensive toolset for automated test case generation, API testing, UI testing, and test reporting.

## Overview

This document provides instructions for installing and setting up the CodeTest toolset in a workspace. CodeTest provides comprehensive testing capabilities including automated test case generation, API testing, UI testing, and test reporting.

## AI Guided Installation

To install this toolset using AI Guided Installation, copy and paste the following prompt into your AI Coding tool's chat window:

```shell
Follow instructions in .asdm/toolsets/code-test/INSTALL.md
```

## Installation Steps

### 1. Detect the current `Agentic Engine` provider

Detect the current AI coding assistant provider (e.g., Claude Code, GitHub Copilot, Tencent CodeBuddy). Using the following guidelines to detect the provider:

- If `.claude` directory exists, use `Claude Code`
- If `.github` directory exists, use `GitHub Copilot`
- If `.codebuddy` directory exists, use `Tencent CodeBuddy`
- If no such folder is found in the current workspace, give user a prompt to select a provider manually

### 2. Create shortcuts commands for CodeTest (toolset ID: `code-test`) in provider's entry point

Create shortcut commands in the appropriate location based on the detected provider. The installation process is consistent across all providers - we use `cat` to concatenate provider-specific frontmatter with the actual instruction content:

#### For Claude Code (`.claude/commands/`):

Claude Code uses Markdown files with Frontmatter metadata for slash commands. Create commands by concatenating Claude-specific frontmatter with instruction content:

```bash
mkdir -p .claude/commands/

# Generate Test Cases command
cat > .claude/commands/asdm-test-generate-cases.md << 'EOF'
---
description: "Generate test cases from source code"
argument-hint: "<file or directory>"
---

EOF
cat .asdm/toolsets/code-test/actions/asdm-test-generate-cases.md >> .claude/commands/asdm-test-generate-cases.md

# API Test command
cat > .claude/commands/asdm-test-api.md << 'EOF'
---
description: "Generate and execute API tests"
argument-hint: "<command> [options]"
---

EOF
cat .asdm/toolsets/code-test/actions/asdm-test-api.md >> .claude/commands/asdm-test-api.md

# UI Test command
cat > .claude/commands/asdm-test-ui.md << 'EOF'
---
description: "Execute automated UI tests"
argument-hint: "<command> [options]"
---

EOF
cat .asdm/toolsets/code-test/actions/asdm-test-ui.md >> .claude/commands/asdm-test-ui.md

# Test Report command
cat > .claude/commands/asdm-test-report.md << 'EOF'
---
description: "Generate test reports with analytics"
argument-hint: "<command> [options]"
---

EOF
cat .asdm/toolsets/code-test/actions/asdm-test-report.md >> .claude/commands/asdm-test-report.md
```

#### For GitHub Copilot (`.github/prompts/`):

GitHub Copilot uses `.prompt.md` files with YAML frontmatter. Create prompt files by concatenating GitHub-specific frontmatter with instruction content:

```bash
mkdir -p .github/prompts/

# Generate Test Cases prompt
cat > .github/prompts/asdm-test-generate-cases.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Generate test cases from source code'
argument-hint: 'Enter file or directory path'
---

EOF
cat .asdm/toolsets/code-test/actions/asdm-test-generate-cases.md >> .github/prompts/asdm-test-generate-cases.prompt.md

# API Test prompt
cat > .github/prompts/asdm-test-api.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Generate and execute API tests'
argument-hint: 'Enter command and options'
---

EOF
cat .asdm/toolsets/code-test/actions/asdm-test-api.md >> .github/prompts/asdm-test-api.prompt.md

# UI Test prompt
cat > .github/prompts/asdm-test-ui.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Execute automated UI tests'
argument-hint: 'Enter command and options'
---

EOF
cat .asdm/toolsets/code-test/actions/asdm-test-ui.md >> .github/prompts/asdm-test-ui.prompt.md

# Test Report prompt
cat > .github/prompts/asdm-test-report.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Generate test reports with analytics'
argument-hint: 'Enter command and options'
---

EOF
cat .asdm/toolsets/code-test/actions/asdm-test-report.md >> .github/prompts/asdm-test-report.prompt.md
```

#### For Tencent CodeBuddy (`.codebuddy/commands/`):

CodeBuddy doesn't support frontmatter, so simply copy the instruction files as-is:

```bash
mkdir -p .codebuddy/commands/

# Copy instruction files directly (no frontmatter needed)
cp .asdm/toolsets/code-test/actions/asdm-test-generate-cases.md .codebuddy/commands/
cp .asdm/toolsets/code-test/actions/asdm-test-api.md .codebuddy/commands/
cp .asdm/toolsets/code-test/actions/asdm-test-ui.md .codebuddy/commands/
cp .asdm/toolsets/code-test/actions/asdm-test-report.md .codebuddy/commands/
```

### 3. Manual Usage for Other Providers

If your AI coding assistant provider is not detected by the automatic detection logic (Claude Code, GitHub Copilot, or Tencent CodeBuddy), you can still use CodeTest manually. Follow these steps:

#### Direct Instruction Usage

You can directly use the instruction files by copying their relative paths and pasting them into your AI coding assistant's chat window:

1. **Navigate to the instruction files**:
   ```bash
   cd .asdm/toolsets/code-test/actions/
   ```

2. **Right-click on the desired instruction file** and copy its relative path:
   - For test case generation: `asdm-test-generate-cases.md`
   - For API testing: `asdm-test-api.md`
   - For UI testing: `asdm-test-ui.md`
   - For test reporting: `asdm-test-report.md`

3. **Enter a prompt** in your AI coding assistant:
   ```
   Follow the instructions in {relative path to instruction file}
   ```

## Initializing CodeTest

### Generate Test Cases

After installation, you can generate test cases for your code:

```shell
Follow the instructions in .asdm/toolsets/code-test/actions/asdm-test-generate-cases.md
```

This will:
1. Analyze source code structure
2. Generate comprehensive test cases
3. Create test files with proper structure
4. Estimate code coverage

### Run API Tests

For API testing:

```shell
Follow the instructions in .asdm/toolsets/code-test/actions/asdm-test-api.md
```

This will:
1. Execute API test suites
2. Validate responses against schemas
3. Run performance benchmarks
4. Generate test reports

### Run UI Tests

For UI testing:

```shell
Follow the instructions in .asdm/toolsets/code-test/actions/asdm-test-ui.md
```

This will:
1. Execute automated UI test suites
2. Perform visual regression testing
3. Test accessibility compliance
4. Capture screenshots and videos

### Generate Test Reports

For comprehensive test reporting:

```shell
Follow the instructions in .asdm/toolsets/code-test/actions/asdm-test-report.md
```

This will:
1. Aggregate test results from multiple sources
2. Generate interactive reports
3. Analyze test trends over time
4. Provide actionable insights

### Available Commands

Once installed, you can use the following commands:

1. **`/asdm-test-generate-cases`** - Generate test cases from source code
2. **`/asdm-test-api`** - Generate and execute API tests
3. **`/asdm-test-ui`** - Execute automated UI tests
4. **`/asdm-test-report`** - Generate test reports with analytics

## Verification

After installation, verify that:

1. Shortcut commands for CodeTest (toolset ID: `code-test`) are created in the appropriate provider directory (if using Claude Code, GitHub Copilot, or Tencent CodeBuddy)
2. The CodeTest toolset files are located in `.asdm/toolsets/code-test`

**For other providers**: Verify that you can access the instruction files at:
- `.asdm/toolsets/code-test/actions/asdm-test-generate-cases.md`
- `.asdm/toolsets/code-test/actions/asdm-test-api.md`
- `.asdm/toolsets/code-test/actions/asdm-test-ui.md`
- `.asdm/toolsets/code-test/actions/asdm-test-report.md`

## Usage Examples

### Test Generation Workflow

```shell
# First, install the toolset using AI Guided Installation
Follow instructions in .asdm/toolsets/code-test/INSTALL.md

# Generate test cases
Follow the instructions in .asdm/toolsets/code-test/actions/asdm-test-generate-cases.md

# Run API tests
Follow the instructions in .asdm/toolsets/code-test/actions/asdm-test-api.md

# Run UI tests
Follow the instructions in .asdm/toolsets/code-test/actions/asdm-test-ui.md

# Generate test report
Follow the instructions in .asdm/toolsets/code-test/actions/asdm-test-report.md

# Example prompts when using slash commands:
/asdm-test-generate-cases --file src/utils.js --type unit
/asdm-test-api run --collection api-tests.json --env staging
/asdm-test-ui run --suite tests/e2e/ --browser chrome
/asdm-test-report generate --format html --output reports/
```

## Notes

- This installation process assumes you have the necessary permissions to create directories and files
- The actual implementation of the commands will be handled by the AI model using the templates and instructions provided in CodeTest (toolset ID: `code-test`)
- Make sure to customize the provider-specific setup based on your actual AI coding assistant
- The toolset ID `code-test` should be used consistently when referring to CodeTest in commands and documentation
- **For providers not in the detection logic**: Users can manually use the instruction files by copying their relative paths and entering prompts like "follow the instructions in .asdm/toolsets/code-test/actions/asdm-test-generate-cases.md"

## Spec Documents

The toolset uses the following spec documents as templates:

1. **`specs4asdm-test-generate-cases.md`** - Specification for test case generation
2. **`specs4asdm-test-api.md`** - Specification for API testing methodology
3. **`specs4asdm-test-ui.md`** - Specification for UI testing procedures
4. **`specs4asdm-test-report.md`** - Template for generating test reports

## Integration with Other Toolsets

CodeTest can be used independently or integrated with other toolsets to support the complete development workflow:

- **Basic Tools**: Use together for commit workflows
- **Context Builder**: Leverage project context for better test generation
- **Code Review**: Integrate test results into code review process
- **CI/CD Pipeline**: Automate testing as part of deployment workflow

### Getting Help

For issues with CodeTest toolset, refer to:
- [ASDM Documentation](https://asdm.ai/docs)
- Toolset README: `.asdm/toolsets/code-test/README.md`
- Spec documents in `.asdm/toolsets/code-test/specs/`

## License

Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

Licensed under the PROPRIETARY SOFTWARE LICENSE. See [LICENSE](LICENSE) in the project root for license information.

---

*This installation document is part of the CodeTest toolset. Use the actions to perform comprehensive testing tasks including test case generation, API testing, UI testing, and test reporting.*
