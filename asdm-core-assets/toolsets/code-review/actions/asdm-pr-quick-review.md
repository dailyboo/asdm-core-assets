# ASDM Action: PR Quick Review

## Overview

This action performs a quick, lightweight code review on a Pull Request, focusing on common issues and providing rapid feedback.

## Purpose

- Provide fast feedback on common code issues
- Check basic code quality and style
- Identify obvious bugs and errors
- Verify basic best practices
- Enable quick iteration cycles

## Steps

1. **Quick Scan**
   - Get PR diff summary
   - Identify file types and changes
   - Assess scope of changes

2. **Basic Quality Check**
   - Check for syntax errors
   - Verify code formatting
   - Identify obvious logic errors
   - Check for missing error handling

3. **Common Issues Detection**
   - Detect common anti-patterns
   - Check for TODO/FIXME comments
   - Identify unused imports/variables
   - Check for debug code left in

4. **Style Verification**
   - Check naming conventions
   - Verify consistent formatting
   - Check line length limits
   - Verify proper indentation

5. **Quick Summary**
   - Summarize findings
   - Highlight quick wins
   - Provide actionable feedback

## Input

- PR number or URL
- Optional: specific focus areas

## Output

- Concise review summary with:
  - Quick assessment
  - Top issues to address
  - Style/formatting suggestions
  - Quick wins for improvement

## Language Detection

Before performing review, detect and use the current environment's response language:

1. **Detect Response Language**: Analyze the environment settings
2. **Apply Language Consistency**: Ensure all output uses the detected language

## Quick Check Items

### Code Quality Quick Checks
- [ ] No obvious syntax errors
- [ ] No hardcoded values that should be configurable
- [ ] No commented-out code
- [ ] No console.log/print statements in production code
- [ ] Proper error handling present

### Style Quick Checks
- [ ] Consistent naming conventions
- [ ] Proper indentation
- [ ] Reasonable line lengths
- [ ] No trailing whitespace
- [ ] Proper file encoding

### Common Anti-Patterns
- [ ] No deep nesting (more than 3-4 levels)
- [ ] No god functions (too long/too many responsibilities)
- [ ] No magic numbers without explanation
- [ ] No duplicate code blocks
- [ ] No unused variables or imports

### Documentation Quick Checks
- [ ] Public functions have comments
- [ ] Complex logic is explained
- [ ] No TODO without ticket reference
- [ ] No FIXME without explanation

## Quick Severity Guide

| Level | Description | Action |
|-------|-------------|--------|
| Must Fix | Errors or critical issues | Fix before merge |
| Should Fix | Style or quality issues | Recommend fixing |
| Consider | Minor improvements | Optional |

## Usage

This action is invoked when a user needs quick feedback on a Pull Request.

### Example Invocation

```shell
# Quick review of PR #123
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-quick-review.md
```

## Output Format

```markdown
# Quick Review Summary

## Overview
[1-2 sentence summary of the changes]

## Must Fix
[Critical issues that should be addressed]

## Should Fix
[Recommended improvements]

## Consider
[Nice-to-have suggestions]

## Quick Stats
- Files changed: X
- Lines added: Y
- Lines removed: Z
- Estimated review complexity: Low/Medium/High

## Verdict
[Ready for detailed review / Needs revision first]
```

## Notes

- Focus on speed and actionable feedback
- Not a replacement for comprehensive review
- Best used for early feedback in PR lifecycle
- Can be followed by detailed review if needed
