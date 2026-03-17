# Code Review Toolset Installation

toolset-id: code-review
toolset-name: Code Review
version: 0.0.2
updated-date: 2026-03-16
toolset-description: A comprehensive toolset for AI-assisted code review based on Pull Requests.

## Overview

This document provides instructions for installing and setting up the Code Review toolset in a workspace. Code Review provides AI-assisted code review capabilities based on Pull Requests, including comprehensive review, security analysis, and quick feedback options.

## AI Guided Installation

To install this toolset using AI Guided Installation, copy and paste the following prompt into your AI Coding tool's chat window:

```shell
Follow instructions in .asdm/toolsets/code-review/INSTALL.md
```

## Installation Steps

### 1. Detect the current `Agentic Engine` provider

Detect the current AI coding assistant provider (e.g., Claude Code, GitHub Copilot, Tencent CodeBuddy). Using the following guidelines to detect the provider:

- If `.claude` directory exists, use `Claude Code`
- If `.github` directory exists, use `GitHub Copilot`
- If `.codebuddy` directory exists, use `Tencent CodeBuddy`
- If no such folder is found in the current workspace, give user a prompt to select a provider manually

### 2. Create shortcuts commands for Code Review (toolset ID: `code-review`) in provider's entry point

Create shortcut commands in the appropriate location based on the detected provider. The installation process is consistent across all providers - we use `cat` to concatenate provider-specific frontmatter with the actual instruction content:

#### For Claude Code (`.claude/commands/`):

Claude Code uses Markdown files with Frontmatter metadata for slash commands. Create commands by concatenating Claude-specific frontmatter with instruction content:

```bash
mkdir -p .claude/commands/

# PR Review command
cat > .claude/commands/asdm-pr-review.md << 'EOF'
---
description: "Perform comprehensive code review on a Pull Request"
argument-hint: "<PR number>"
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-review.md >> .claude/commands/asdm-pr-review.md

# PR Security Review command
cat > .claude/commands/asdm-pr-security-review.md << 'EOF'
---
description: "Perform security-focused code review on a Pull Request"
argument-hint: "<PR number>"
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-security-review.md >> .claude/commands/asdm-pr-security-review.md

# PR Quick Review command
cat > .claude/commands/asdm-pr-quick-review.md << 'EOF'
---
description: "Perform quick lightweight code review on a Pull Request"
argument-hint: "<PR number>"
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-quick-review.md >> .claude/commands/asdm-pr-quick-review.md

# PR Diff command
cat > .claude/commands/asdm-pr-diff.md << 'EOF'
---
description: "Get Pull Request diff and changes"
argument-hint: "<PR number>"
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-diff.md >> .claude/commands/asdm-pr-diff.md

# PR Summary command
cat > .claude/commands/asdm-pr-summary.md << 'EOF'
---
description: "Generate a summary of Pull Request changes"
argument-hint: "<PR number>"
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-summary.md >> .claude/commands/asdm-pr-summary.md
```

#### For GitHub Copilot (`.github/prompts/`):

GitHub Copilot uses `.prompt.md` files with YAML frontmatter. Create prompt files by concatenating GitHub-specific frontmatter with instruction content:

```bash
mkdir -p .github/prompts/

# PR Review prompt
cat > .github/prompts/asdm-pr-review.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Perform comprehensive code review on a Pull Request'
argument-hint: 'Enter PR number'
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-review.md >> .github/prompts/asdm-pr-review.prompt.md

# PR Security Review prompt
cat > .github/prompts/asdm-pr-security-review.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Perform security-focused code review on a Pull Request'
argument-hint: 'Enter PR number'
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-security-review.md >> .github/prompts/asdm-pr-security-review.prompt.md

# PR Quick Review prompt
cat > .github/prompts/asdm-pr-quick-review.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Perform quick lightweight code review on a Pull Request'
argument-hint: 'Enter PR number'
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-quick-review.md >> .github/prompts/asdm-pr-quick-review.prompt.md

# PR Diff prompt
cat > .github/prompts/asdm-pr-diff.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Get Pull Request diff and changes'
argument-hint: 'Enter PR number'
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-diff.md >> .github/prompts/asdm-pr-diff.prompt.md

# PR Summary prompt
cat > .github/prompts/asdm-pr-summary.prompt.md << 'EOF'
---
agent: 'agent'
description: 'Generate a summary of Pull Request changes'
argument-hint: 'Enter PR number'
---

EOF
cat .asdm/toolsets/code-review/actions/asdm-pr-summary.md >> .github/prompts/asdm-pr-summary.prompt.md
```

#### For Tencent CodeBuddy (`.codebuddy/commands/`):

CodeBuddy doesn't support frontmatter, so simply copy the instruction files as-is:

```bash
mkdir -p .codebuddy/commands/

# Copy instruction files directly (no frontmatter needed)
cp .asdm/toolsets/code-review/actions/asdm-pr-review.md .codebuddy/commands/
cp .asdm/toolsets/code-review/actions/asdm-pr-security-review.md .codebuddy/commands/
cp .asdm/toolsets/code-review/actions/asdm-pr-quick-review.md .codebuddy/commands/
cp .asdm/toolsets/code-review/actions/asdm-pr-diff.md .codebuddy/commands/
cp .asdm/toolsets/code-review/actions/asdm-pr-summary.md .codebuddy/commands/
```

### 3. Manual Usage for Other Providers

If your AI coding assistant provider is not detected by the automatic detection logic (Claude Code, GitHub Copilot, or Tencent CodeBuddy), you can still use Code Review manually. Follow these steps:

#### Direct Instruction Usage

You can directly use the instruction files by copying their relative paths and pasting them into your AI coding assistant's chat window:

1. **Navigate to the instruction files**:
   ```bash
   cd .asdm/toolsets/code-review/actions/
   ```

2. **Right-click on the desired instruction file** and copy its relative path:
   - For comprehensive review: `asdm-pr-review.md`
   - For security review: `asdm-pr-security-review.md`
   - For quick review: `asdm-pr-quick-review.md`
   - For PR diff: `asdm-pr-diff.md`
   - For PR summary: `asdm-pr-summary.md`

3. **Enter a prompt** in your AI coding assistant:
   ```
   Follow the instructions in {relative path to instruction file}
   ```

## Initializing Code Review

### Performing a Comprehensive Code Review

After installation, you can perform code review on a PR:

```shell
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-review.md
```

This will:
1. Analyze code changes for quality issues
2. Check for security vulnerabilities
3. Review performance considerations
4. Generate structured review report

### Performing a Security-Focused Review

For security-specific analysis:

```shell
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-security-review.md
```

This will:
1. Identify security vulnerabilities and risks
2. Verify authentication and authorization implementation
3. Check for common attack vectors
4. Provide remediation guidance

### Performing a Quick Review

For fast feedback:

```shell
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-quick-review.md
```

This will:
1. Check basic code quality and style
2. Identify obvious bugs and errors
3. Provide quick actionable feedback

### Getting PR Diff

When you need to see the changes in a PR:

```shell
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-diff.md
```

This will:
1. Fetch PR diff
2. Organize changes by file
3. Provide structured output for analysis

### Generating PR Summary

For a high-level overview of PR changes:

```shell
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-summary.md
```

This will:
1. Summarize all changes in the PR
2. Identify affected components
3. List key modifications
4. Provide high-level overview

### Available Commands

Once installed, you can use the following commands:

1. **`/asdm-pr-review`** - Perform comprehensive code review on a PR
2. **`/asdm-pr-security-review`** - Perform security-focused code review
3. **`/asdm-pr-quick-review`** - Perform quick lightweight code review
4. **`/asdm-pr-diff`** - Get PR change differences
5. **`/asdm-pr-summary`** - Generate PR change summary

## Verification

After installation, verify that:

1. Shortcut commands for Code Review (toolset ID: `code-review`) are created in the appropriate provider directory (if using Claude Code, GitHub Copilot, or Tencent CodeBuddy)
2. The Code Review toolset files are located in `.asdm/toolsets/code-review`

**For other providers**: Verify that you can access the instruction files at:
- `.asdm/toolsets/code-review/actions/asdm-pr-review.md`
- `.asdm/toolsets/code-review/actions/asdm-pr-security-review.md`
- `.asdm/toolsets/code-review/actions/asdm-pr-quick-review.md`
- `.asdm/toolsets/code-review/actions/asdm-pr-diff.md`
- `.asdm/toolsets/code-review/actions/asdm-pr-summary.md`

## Usage Examples

### Code Review Workflow

```shell
# First, install the toolset using AI Guided Installation
Follow instructions in .asdm/toolsets/code-review/INSTALL.md

# Get PR diff to see changes
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-diff.md

# Perform code review
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-review.md

# Or perform security review
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-security-review.md

# Generate PR summary
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-summary.md

# Example prompts when using slash commands:
/asdm-pr-diff 123
/asdm-pr-review 123
/asdm-pr-security-review 123
/asdm-pr-quick-review 123
/asdm-pr-summary 123
```

## Notes

- This installation process assumes you have the necessary permissions to create directories and files
- The actual implementation of the commands will be handled by the AI model using the templates and instructions provided in Code Review (toolset ID: `code-review`)
- Make sure to customize the provider-specific setup based on your actual AI coding assistant
- The toolset ID `code-review` should be used consistently when referring to Code Review in commands and documentation
- **For providers not in the detection logic**: Users can manually use the instruction files by copying their relative paths and entering prompts like "follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-review.md"

## Spec Documents

The toolset uses the following spec documents as templates:

1. **`pr-review-spec.md`** - Comprehensive PR review process specification
2. **`pr-analysis-spec.md`** - Specification for PR analysis methodology
3. **`security-review-spec.md`** - Security review checklist and guidelines
4. **`review-report-template.md`** - Template for generating structured review reports

## Integration with Other Toolsets

Code Review can be used independently or integrated with other toolsets to support the complete development workflow:

- **Basic Tools**: Use together for commit workflows
- **Context Builder**: Leverage project context for better review insights
- **CI/CD Pipeline**: Automate review as part of pull request workflow

### Getting Help

For issues with Code Review toolset, refer to:
- [ASDM Documentation](https://asdm.ai/docs)
- Toolset README: `.asdm/toolsets/code-review/README.md`
- Spec documents in `.asdm/toolsets/code-review/specs/`

## License

Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

Licensed under the PROPRIETARY SOFTWARE LICENSE. See [LICENSE](LICENSE) in the project root for license information.

---

*This installation document is part of the Code Review toolset. Use the actions to perform code review tasks on Pull Requests.*
