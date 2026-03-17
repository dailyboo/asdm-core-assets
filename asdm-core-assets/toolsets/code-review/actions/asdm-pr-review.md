# ASDM Action: PR Code Review

## Overview

This action performs a comprehensive code review on a Pull Request, analyzing code quality, security, performance, and best practices.

## Purpose

- Identify code quality issues and potential bugs
- Detect security vulnerabilities
- Check adherence to coding standards and best practices
- Provide actionable improvement suggestions
- Generate structured review report

## Steps

1. **Gather PR Information**
   - Get PR diff and changed files
   - Identify the programming languages involved
   - Understand the context and purpose of changes

2. **Analyze Code Changes**
   - Review code structure and organization
   - Check for code smells and anti-patterns
   - Verify logic correctness and edge cases
   - Assess test coverage

3. **Security Analysis**
   - Check for common security vulnerabilities
   - Review authentication and authorization logic
   - Identify potential injection risks
   - Verify sensitive data handling

4. **Performance Review**
   - Identify potential performance bottlenecks
   - Check for inefficient algorithms
   - Review database query patterns
   - Assess memory usage patterns

5. **Best Practices Check**
   - Verify coding style consistency
   - Check naming conventions
   - Review error handling patterns
   - Assess code maintainability

6. **Generate Review Report**
   - Summarize findings by severity
   - Provide specific line-level feedback
   - Suggest improvements with code examples
   - Assign severity levels to each finding

## Input

- PR number or URL
- Target repository context
- Optional: specific areas to focus on

## Output

- Structured review report with:
  - Executive summary
  - Detailed findings by category
  - Severity levels (Critical/High/Medium/Low/Info)
  - Line-level comments with suggestions
  - Overall recommendation

## Language Detection

Before performing review, detect and use the current environment's response language:

1. **Detect Response Language**: Analyze the environment settings to determine the primary language:
   - Check system/user language settings or environment configuration
   - Identify the primary language used in project documentation and comments
   - Determine the language preference based on workspace context

2. **Apply Language Consistency**: Ensure all generated content uses the detected language:
   - Use the same language for all review comments, explanations, and documentation
   - Maintain language consistency across all generated content
   - Follow the detected language's writing conventions and formatting

**IMPORTANT**: The language detection is the FIRST step before any review content generation. All output must consistently use the detected language throughout the entire process.

## Review Dimensions

### 1. Code Quality
- Readability and maintainability
- Code duplication
- Complexity analysis
- Dead code detection

### 2. Security
- Injection vulnerabilities (SQL, XSS, Command)
- Authentication and authorization issues
- Sensitive data exposure
- Input validation

### 3. Performance
- Algorithm efficiency
- Resource management
- Caching opportunities
- Database query optimization

### 4. Best Practices
- Design patterns usage
- Error handling
- Logging and monitoring
- Documentation completeness

### 5. Testing
- Test coverage
- Test quality
- Edge case coverage
- Mock usage appropriateness

## Severity Definitions

| Severity | Description | Action Required |
|----------|-------------|-----------------|
| Critical | Security vulnerabilities, breaking bugs, data loss risks | Must fix before merge |
| High | Significant issues that could cause problems | Should fix before merge |
| Medium | Code quality issues, maintainability concerns | Recommend fix |
| Low | Minor improvements, style preferences | Optional fix |
| Info | Informational notes, suggestions | No action required |

## Usage

This action is invoked when a user needs comprehensive code review on a Pull Request.

### Example Invocation

```shell
# Review PR #123
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-review.md
```

## Review Process Flow

```
┌─────────────────┐
│  Get PR Info    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Analyze Diff   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐     ┌─────────────────┐
│ Security Check  │────▶│ Quality Check   │
└────────┬────────┘     └────────┬────────┘
         │                       │
         ▼                       ▼
┌─────────────────┐     ┌─────────────────┐
│ Perf Check      │────▶│ Best Practices  │
└────────┬────────┘     └────────┬────────┘
         │                       │
         └───────────┬───────────┘
                     │
                     ▼
         ┌─────────────────┐
         │ Generate Report │
         └─────────────────┘
```

## Output Format

The review report follows this structure:

```markdown
# Code Review Report

## Summary
[Brief overview of the PR and overall assessment]

## Critical Issues
[List of critical issues that must be fixed]

## High Priority Issues
[List of high priority issues]

## Medium Priority Issues
[List of medium priority issues]

## Low Priority Issues
[List of low priority issues]

## Suggestions
[List of informational suggestions]

## Overall Recommendation
[Approve / Request Changes / Need Discussion]
```

## Notes

- Focus on constructive feedback with actionable suggestions
- Prioritize issues by impact and likelihood
- Provide code examples for suggested improvements
- Consider the project's specific context and constraints
