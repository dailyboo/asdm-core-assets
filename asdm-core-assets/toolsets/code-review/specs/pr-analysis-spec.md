# PR Analysis Specification

## Overview

This specification defines the methodology for analyzing Pull Requests to understand their scope, impact, and characteristics.

## Analysis Objectives

1. **Scope Understanding**: Determine what the PR changes
2. **Impact Assessment**: Understand how changes affect the system
3. **Risk Evaluation**: Identify potential risks of the changes
4. **Context Building**: Provide context for code review

## Analysis Process

### Step 1: PR Metadata Analysis

Extract and analyze PR metadata:

```json
{
  "metadata": {
    "number": "PR number",
    "title": "PR title",
    "description": "PR description",
    "author": "Author username",
    "source_branch": "Branch name",
    "target_branch": "Branch name",
    "status": "open|merged|closed",
    "created_at": "ISO 8601 timestamp",
    "updated_at": "ISO 8601 timestamp",
    "labels": ["Label names"],
    "milestone": "Milestone name",
    "linked_issues": ["Issue numbers"]
  }
}
```

#### Title Analysis

Parse PR title for change type and scope:

| Prefix | Type | Example |
|--------|------|---------|
| `feat:` | Feature | `feat: add user authentication` |
| `fix:` | Bug Fix | `fix: resolve login timeout issue` |
| `refactor:` | Refactoring | `refactor: simplify payment logic` |
| `docs:` | Documentation | `docs: update API documentation` |
| `test:` | Testing | `test: add unit tests for UserService` |
| `chore:` | Maintenance | `chore: update dependencies` |
| `perf:` | Performance | `perf: optimize database queries` |
| `style:` | Code Style | `style: fix formatting issues` |

#### Description Analysis

Extract key information from description:

- **Purpose**: Why was this change made?
- **Changes**: What was changed?
- **Testing**: How was it tested?
- **Notes**: Any special considerations?

### Step 2: File Change Analysis

Analyze the list of changed files:

```json
{
  "files": [
    {
      "path": "relative/path/to/file",
      "extension": "file extension",
      "status": "added|modified|deleted|renamed",
      "additions": 0,
      "deletions": 0,
      "changes": 0,
      "category": "source|config|test|docs|asset|other"
    }
  ]
}
```

#### File Categorization

| Category | Extensions | Examples |
|----------|------------|----------|
| Source | `.js`, `.ts`, `.java`, `.py`, `.go` | Application code |
| Config | `.json`, `.yaml`, `.xml`, `.properties` | Configuration files |
| Test | `.test.js`, `.spec.ts`, `*Test.java` | Test files |
| Docs | `.md`, `.txt`, `.rst` | Documentation |
| Asset | `.png`, `.jpg`, `.svg`, `.ico` | Images, icons |
| Build | `Makefile`, `Dockerfile`, `.sh` | Build scripts |
| Style | `.css`, `.scss`, `.less` | Stylesheets |

#### Directory Analysis

Group files by directory to understand component impact:

```
src/
├── components/     # UI components
├── services/       # Business logic
├── models/         # Data models
├── utils/          # Utility functions
├── config/         # Configuration
└── tests/          # Test files
```

### Step 3: Change Statistics

Calculate and interpret change statistics:

```json
{
  "statistics": {
    "total_files": 0,
    "total_additions": 0,
    "total_deletions": 0,
    "net_change": 0,
    "by_category": {
      "source": 0,
      "config": 0,
      "test": 0,
      "docs": 0,
      "asset": 0,
      "other": 0
    },
    "by_status": {
      "added": 0,
      "modified": 0,
      "deleted": 0,
      "renamed": 0
    }
  }
}
```

#### Size Interpretation

| Size | Files | Lines Changed | Risk |
|------|-------|---------------|------|
| Tiny | 1-3 | < 50 | Low |
| Small | 4-10 | 50-200 | Low-Medium |
| Medium | 11-25 | 201-500 | Medium |
| Large | 26-50 | 501-1000 | Medium-High |
| Huge | > 50 | > 1000 | High |

### Step 4: Component Impact Analysis

Identify affected components and their relationships:

```json
{
  "components": [
    {
      "name": "Component name",
      "files": ["list of files"],
      "additions": 0,
      "deletions": 0,
      "impact_level": "high|medium|low",
      "change_type": "new|modified|removed",
      "dependencies": ["dependent components"]
    }
  ]
}
```

#### Impact Level Determination

| Level | Criteria |
|-------|----------|
| High | Core functionality, many files, significant lines |
| Medium | Important functionality, moderate changes |
| Low | Minor functionality, few changes |

### Step 5: Risk Assessment

Evaluate risk factors:

```json
{
  "risk_assessment": {
    "level": "low|medium|high|critical",
    "factors": [
      {
        "factor": "Large number of files changed",
        "impact": "high",
        "mitigation": "Consider splitting into smaller PRs"
      }
    ],
    "considerations": [
      "Items to consider during review"
    ]
  }
}
```

#### Risk Factors

| Factor | Weight | Description |
|--------|--------|-------------|
| PR Size | High | Large PRs are harder to review thoroughly |
| Core Components | High | Changes to core modules have wider impact |
| Database Changes | High | Schema changes can cause data issues |
| API Changes | High | Breaking changes affect consumers |
| Security Related | Critical | Authentication, authorization, data handling |
| Performance Sensitive | Medium | Queries, loops, algorithms |
| Configuration Changes | Medium | Environment settings, feature flags |
| Test Coverage | Medium | Missing tests increase risk |

### Step 6: Dependency Analysis

Analyze dependency changes:

```json
{
  "dependencies": {
    "added": [
      {
        "name": "package name",
        "version": "version",
        "type": "production|development"
      }
    ],
    "removed": [],
    "updated": [],
    "security_notes": []
  }
}
```

#### Dependency Check Points

- New dependencies: License compatibility, bundle size
- Updated dependencies: Breaking changes, deprecations
- Removed dependencies: Breaking functionality

### Step 7: Testing Requirements

Determine testing requirements:

```json
{
  "testing_requirements": {
    "unit_tests": {
      "required": true,
      "coverage_areas": ["list of areas"]
    },
    "integration_tests": {
      "required": false,
      "coverage_areas": []
    },
    "e2e_tests": {
      "required": false,
      "scenarios": []
    },
    "manual_testing": {
      "required": true,
      "checklist": ["items to manually verify"]
    }
  }
}
```

## Analysis Output

### Summary Report

```markdown
# PR Analysis Report

## Overview
- **PR Number**: #{number}
- **Title**: {title}
- **Type**: {type}
- **Author**: {author}
- **Branch**: {source} → {target}

## Statistics
- **Files Changed**: {count}
- **Additions**: +{count}
- **Deletions**: -{count}
- **Net Change**: {count}

## Affected Components
| Component | Files | Impact | Change Type |
|-----------|-------|--------|-------------|
| {name} | {count} | {level} | {type} |

## Risk Assessment
- **Level**: {level}
- **Factors**: {factors}

## Testing Requirements
- Unit Tests: {required/not required}
- Integration Tests: {required/not required}
- Manual Testing: {required/not required}

## Recommendations
1. {recommendation}
2. {recommendation}
```

## Integration with Code Review

This analysis provides:

1. **Context** for reviewers
2. **Priority guidance** for issue detection
3. **Risk awareness** for thorough review
4. **Testing requirements** for quality assurance

## Continuous Improvement

- Track prediction accuracy
- Update risk factors based on incidents
- Refine component identification
- Improve impact assessment accuracy
