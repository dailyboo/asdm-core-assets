# ASDM Action: PR Diff

## Overview

This action retrieves and formats the diff of a Pull Request, providing a structured view of all changes for analysis.

## Purpose

- Retrieve PR changes from the repository
- Format diff output for easy analysis
- Support code review workflows
- Enable change analysis

## Prerequisites

- Access to the Pull Request system (GitHub, GitLab, Bitbucket, etc.)
- PR number or PR identifier
- Appropriate repository access permissions

## Input

- **PR Number**: The identifier of the Pull Request (required)
- **File Filter**: Optional file path pattern to filter changes
- **Context Lines**: Number of context lines to show (default: 3)

## Steps

### 1. Get PR Basic Information

First, retrieve the PR metadata using your repository's API:

This returns:
- PR title and description
- Source and target branches
- Author information
- Status (open, merged, closed)
- Number of changed files, additions, deletions

### 2. Fetch PR Diff

Retrieve the complete diff using your repository's API.

### 3. List Changed Files

Get the list of files changed in the PR using your repository's API.

### 4. Parse and Format Output

Organize the diff output into a structured format:

```json
{
  "pr_number": "{PR_NUMBER}",
  "pr_title": "Title of the PR",
  "source_branch": "feature/branch-name",
  "target_branch": "main",
  "author": "username",
  "stats": {
    "files_changed": 5,
    "additions": 150,
    "deletions": 30
  },
  "files": [
    {
      "path": "src/components/Button.tsx",
      "status": "modified|added|deleted|renamed",
      "additions": 20,
      "deletions": 5,
      "changes": [
        {
          "type": "addition|deletion|modification",
          "line_number": 42,
          "content": "Code content here",
          "context": "Surrounding context"
        }
      ]
    }
  ]
}
```

## Output Format

### Summary View

```
PR #{PR_NUMBER}: {PR_TITLE}
Author: {author}
Branch: {source_branch} → {target_branch}
Files: {files_changed} changed, +{additions} -{deletions}

Changed Files:
1. src/path/to/file1.ext (+20, -5) [modified]
2. src/path/to/file2.ext (+100, -0) [added]
3. src/path/to/file3.ext (+0, -10) [deleted]
```

### Detailed Diff View

For each file, provide:

```diff
--- a/src/path/to/file.ext
+++ b/src/path/to/file.ext
@@ -10,6 +10,8 @@ context line
-removed line
+added line
+another added line
 context line
```

## File Status Types

| Status | Description |
|--------|-------------|
| added | New file created |
| modified | Existing file changed |
| deleted | File removed |
| renamed | File renamed/moved |

## Usage Examples

### Basic Usage

```shell
# Using slash command
/asdm-pr-diff 123

# Or follow the instruction
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-diff.md
```

### Filter by File Pattern

```shell
# Get diff for specific files
/asdm-pr-diff 123 --filter "src/components/*.tsx"
```

### Get More Context

```shell
# Show more context lines
/asdm-pr-diff 123 --context 10
```

## Integration with Other Actions

This action is typically used as a prerequisite for:

- `asdm-pr-review`: Provides the diff for review analysis
- `asdm-pr-summary`: Uses diff data for summary generation

## Notes

- Large PRs may have truncated output; handle pagination if needed
- Binary files show only metadata, not content diff
- Consider file size limits for very large diffs
- Use file filtering for focused analysis

## Configuration

Refer to [pr-analysis-spec.md](../specs/pr-analysis-spec.md) for detailed analysis methodology.
