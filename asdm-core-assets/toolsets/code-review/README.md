# ASDM Toolset - Code Review

toolset-id: code-review
toolset-name: Code Review
version: 0.0.2
updated-date: 2026-03-16
toolset-description: A comprehensive toolset for AI-assisted code review based on Pull Requests.

## Overview

Code Review (toolset-id: `code-review`) is a comprehensive toolset for AI-assisted code review based on Pull Requests. It provides a rich set of tools for analyzing code changes, detecting potential issues, and generating review reports.

This toolset helps teams maintain code quality, identify security vulnerabilities, and catch issues early in the development process through automated code review capabilities.

## Features

Main features of Code Review:

- **PR Diff Analysis**: Retrieve and analyze pull request changes
- **PR Summary**: Generate comprehensive summaries of PR changes
- **Comprehensive Code Review**: Perform thorough code review across multiple dimensions
- **Security-Focused Review**: Identify security vulnerabilities and risks
- **Quick Review**: Provide fast feedback on common issues
- **Multi-dimensional Analysis**: Analyze code from security, performance, maintainability, and readability perspectives
- **Best Practices Checking**: Verify code against language-specific best practices and coding standards
- **Review Report Generation**: Generate structured review reports with severity levels

## Toolset Installation Process

`INSTALL.md` will setup the toolset with the following steps:

- Detect the current `Agentic Engine` provider (e.g., Claude Code, GitHub Copilot, Tencent CodeBuddy)
- Create shortcuts commands for `Code Review` in provider's entry point (e.g., `.claude/commands`, `.github/prompts`, `.codebuddy/commands`)

## Toolset Workflow

Once `Code Review` is installed, users can use the following commands:

- `/asdm-pr-review`: Perform comprehensive code review on a PR
- `/asdm-pr-security-review`: Focus on security vulnerabilities and issues
- `/asdm-pr-quick-review`: Quick review for common issues and quick feedback
- `/asdm-pr-diff`: Get PR change differences
- `/asdm-pr-summary`: Generate PR change summary

## Toolset Structure

The structure of the toolset is as follows:

```
.asdm/toolsets/code-review/
├── INSTALL.md                              ## Installation instructions for the toolset
├── README.md                               ## Current document
├── actions                                 ## Instructions for Code Review
│   ├── asdm-pr-review.md                   ## Instruction for performing comprehensive PR code review
│   ├── asdm-pr-security-review.md          ## Instruction for security-focused code review
│   ├── asdm-pr-quick-review.md             ## Instruction for quick lightweight code review
│   ├── asdm-pr-diff.md                     ## Instruction for getting PR diff
│   └── asdm-pr-summary.md                  ## Instruction for generating PR summary
└── specs                                   ## Templates and specifications
    ├── pr-review-spec.md                   ## Specification for PR review process
    ├── pr-analysis-spec.md                 ## Specification for PR analysis
    ├── security-review-spec.md             ## Specification for security review
    └── review-report-template.md           ## Template for generating review reports
```

## Available Actions

### 1. PR Code Review (`asdm-pr-review.md`)

Performs comprehensive code review on a pull request:

- Analyzes code changes for quality issues
- Checks for security vulnerabilities
- Identifies potential bugs and code smells
- Reviews performance considerations
- Generates structured review report

### 2. PR Security Review (`asdm-pr-security-review.md`)

Performs security-focused code review on a pull request:

- Identifies security vulnerabilities and risks
- Verifies authentication and authorization implementation
- Checks for common attack vectors
- Ensures secure data handling practices
- Provides security-specific remediation guidance

### 3. PR Quick Review (`asdm-pr-quick-review.md`)

Performs quick, lightweight code review for rapid feedback:

- Checks basic code quality and style
- Identifies obvious bugs and errors
- Verifies basic best practices
- Enables quick iteration cycles

### 4. PR Diff (`asdm-pr-diff.md`)

Retrieves and formats PR changes:

- Fetches PR diff
- Organizes changes by file
- Provides structured output for analysis

### 5. PR Summary (`asdm-pr-summary.md`)

Generates comprehensive PR summary:

- Summarizes all changes in the PR
- Identifies affected components
- Lists key modifications
- Provides high-level overview

## Review Dimensions

The Code Review toolset analyzes code across multiple dimensions:

1. **Code Quality**: Maintainability, readability, and code smell detection
2. **Security**: Vulnerability detection, injection risks, authentication issues
3. **Performance**: Efficiency issues, memory leaks, optimization opportunities
4. **Best Practices**: Language-specific conventions and design patterns
5. **Testing**: Test coverage, test quality, and missing tests
6. **Documentation**: Missing documentation, unclear comments

## Severity Levels

Review findings are categorized by severity:

| Level | Description | Action Required |
|-------|-------------|-----------------|
| **Critical** | Security vulnerabilities, breaking bugs, data loss risks | Must fix before merge |
| **High** | Significant issues that could cause problems | Should fix before merge |
| **Medium** | Code quality issues, maintainability concerns | Recommend fix |
| **Low** | Minor improvements, style preferences | Optional fix |
| **Info** | Informational notes, suggestions | No action required |

## Usage Examples

### Perform Comprehensive Code Review on PR

```shell
# Using slash command
/asdm-pr-review 123

# Or follow the instruction
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-review.md
```

### Perform Security-Focused Review

```shell
# Using slash command
/asdm-pr-security-review 123

# Or follow the instruction
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-security-review.md
```

### Perform Quick Review

```shell
# Using slash command
/asdm-pr-quick-review 123

# Or follow the instruction
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-quick-review.md
```

### Get PR Diff

```shell
# Using slash command
/asdm-pr-diff 123

# Or follow the instruction
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-diff.md
```

### Generate PR Summary

```shell
# Using slash command
/asdm-pr-summary 123

# Or follow the instruction
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-summary.md
```

## Prerequisites

- Access to the Pull Request system (GitHub, GitLab, Bitbucket, etc.)
- Git repository with proper commit history
- Understanding of the project's coding standards and conventions

## Integration with Other Toolsets

Code Review can be used independently or integrated with other toolsets:

- **Basic Tools**: Use together for commit workflows
- **Context Builder**: Leverage project context for better review insights
- **CI/CD Pipeline**: Automate review as part of pull request workflow

## Getting Help

For issues with Code Review toolset, refer to:

- [ASDM Documentation](https://asdm.ai/docs)
- Toolset README: `.asdm/toolsets/code-review/README.md`
- Spec documents in `.asdm/toolsets/code-review/specs/`

## Copyright & License

Copyright (c) 2026 LeansoftX.com & iSoftStone. All rights reserved.

Licensed under the PROPRIETARY SOFTWARE LICENSE. See [LICENSE](LICENSE) in the project root for license information.
