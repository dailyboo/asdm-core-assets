# ASDM Action: PR Summary

## Overview

This action generates a comprehensive summary of Pull Request changes, providing a high-level overview of modifications and their impact.

## Purpose

- Provide quick overview of PR changes
- Summarize affected components and modules
- Identify key modifications and their purposes
- Help reviewers understand PR scope before detailed review

## Prerequisites

- Access to the Pull Request system (GitHub, GitLab, Bitbucket, etc.)
- PR number or PR identifier
- Appropriate repository access permissions

## Input

- **PR Number**: The identifier of the Pull Request (required)
- **Detail Level**: Optional (brief/standard/detailed), default: standard
- **Focus Areas**: Optional areas to highlight (e.g., API, database, UI)

## Steps

### 1. Retrieve PR Information

Get the PR metadata and description using your repository's API.

### 2. Fetch Changed Files List

Get the list of changed files using your repository's API.

### 3. Analyze Changes

Categorize and analyze the changes:

#### By File Type
- Source code files (by language)
- Configuration files
- Documentation files
- Test files
- Asset files

#### By Component/Module
- Group files by logical component
- Identify affected modules
- Map changes to architectural layers

#### By Change Type
- New features
- Bug fixes
- Refactoring
- Documentation updates
- Configuration changes
- Test additions/modifications

### 4. Generate Summary

Create a structured summary:

```json
{
  "pr_number": "{PR_NUMBER}",
  "pr_title": "Title of the PR",
  "pr_description": "PR description text",
  "author": "username",
  "source_branch": "feature/branch-name",
  "target_branch": "main",
  "status": "open",
  "created_at": "2026-03-13T12:00:00Z",
  "stats": {
    "files_changed": 10,
    "additions": 500,
    "deletions": 100,
    "net_change": 400
  },
  "change_categories": {
    "features": ["New feature X implemented"],
    "fixes": ["Bug Y fixed in module Z"],
    "refactoring": ["Code cleanup in service layer"],
    "tests": ["Added tests for feature X"],
    "docs": ["Updated README with new instructions"]
  },
  "affected_components": [
    {
      "name": "Authentication Module",
      "files": ["src/auth/login.ts", "src/auth/session.ts"],
      "change_type": "enhancement",
      "impact": "medium"
    }
  ],
  "risk_assessment": {
    "level": "low|medium|high",
    "reasons": ["Large number of files changed", "Core module modified"]
  },
  "testing_requirements": [
    "Unit tests needed for new authentication flow",
    "Integration tests recommended for API changes"
  ]
}
```

## Output Format

### Brief Summary

```
PR #{PR_NUMBER}: {PR_TITLE}

📊 Statistics:
- Files changed: {count}
- Additions: +{additions}
- Deletions: -{deletions}

📝 Summary:
{Brief description of changes}

🏷️ Type: feature|fix|refactor|docs|test
⚠️ Risk: low|medium|high
```

### Standard Summary

```
PR #{PR_NUMBER}: {PR_TITLE}

Author: {author}
Branch: {source_branch} → {target_branch}

## 📊 Statistics
- Files changed: {count}
- Additions: +{additions}
- Deletions: -{deletions}

## 📝 Change Summary
{Detailed summary of what was changed and why}

## 📁 Affected Components
- **{Component Name}**: {change description}
- **{Component Name}**: {change description}

## 🏷️ Change Categories
- ✨ Features: {count}
- 🐛 Fixes: {count}
- ♻️ Refactoring: {count}
- 📚 Documentation: {count}
- 🧪 Tests: {count}

## ⚠️ Risk Assessment
Level: {low/medium/high}
Reasons: {reasons}

## ✅ Testing Recommendations
- {recommendation 1}
- {recommendation 2}
```

### Detailed Summary

Includes all of standard summary plus:

```
## 📄 Changed Files

### Added Files
- `path/to/file1.ext` - {description}
- `path/to/file2.ext` - {description}

### Modified Files
- `path/to/file3.ext` - {description of changes}

### Deleted Files
- `path/to/file4.ext` - {reason for deletion}

## 🔗 Dependencies
- New dependencies added: {list}
- Dependencies removed: {list}
- Dependencies updated: {list}

## 🔍 Key Code Changes
### {Section Name}
{Highlighted important code changes with context}
```

## Usage Examples

### Basic Usage

```shell
# Using slash command
/asdm-pr-summary 123

# Or follow the instruction
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-summary.md
```

### Brief Summary

```shell
# Get brief summary
/asdm-pr-summary 123 --brief
```

### Detailed Summary

```shell
# Get detailed summary
/asdm-pr-summary 123 --detailed
```

## Integration with Other Actions

This action complements:

- `asdm-pr-diff`: Uses diff data for detailed analysis
- `asdm-pr-review`: Provides context for review decisions

## Notes

- Summary quality depends on PR description quality
- Consider generating summaries after major commits
- Use in combination with diff for comprehensive understanding
- Update summary if PR changes significantly

## Configuration

Refer to [pr-analysis-spec.md](../specs/pr-analysis-spec.md) for detailed analysis methodology.
