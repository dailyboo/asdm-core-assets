# Review Report Template

**Document Version**: 1.0  
**Last Updated**: 2026-03-16

## Overview

This template defines the standard format for generating code review reports. It ensures consistency and completeness in review output.

---

# Code Review Report

## Metadata

| Field | Value |
|-------|-------|
| **PR Number** | #XXX |
| **Review Date** | YYYY-MM-DD |
| **Reviewer** | [AI Assistant] |
| **PR Author** | [Author Name] |
| **Target Branch** | [Branch Name] |
| **Review Type** | Comprehensive / Security / Quick |

## Executive Summary

[Brief 2-3 sentence overview of the PR changes and overall assessment. Include the general quality of the code and any standout positives or concerns.]

### Change Statistics

| Metric | Value |
|--------|-------|
| Files Changed | X |
| Lines Added | +X |
| Lines Removed | -X |
| Languages | [List] |
| Complexity Score | Low/Medium/High |

### Overall Assessment

| Aspect | Rating | Notes |
|--------|--------|-------|
| Code Quality | ⭐⭐⭐⭐☆ | [Brief comment] |
| Security | ⭐⭐⭐⭐⭐ | [Brief comment] |
| Performance | ⭐⭐⭐⭐☆ | [Brief comment] |
| Maintainability | ⭐⭐⭐⭐☆ | [Brief comment] |
| Test Coverage | ⭐⭐⭐☆☆ | [Brief comment] |

---

## Critical Issues 🔴

*Must be fixed before merge*

### CRIT-001: [Issue Title]

**Location**: `path/to/file.ext:123`

**Description**:
[Detailed description of the critical issue]

**Impact**:
[Why this is critical and potential consequences]

**Code Context**:
```language
// Current problematic code
```

**Suggested Fix**:
```language
// Recommended solution
```

**References**:
- [Link to relevant documentation or best practice]

---

### CRIT-002: [Issue Title]

[Same format as above]

---

## High Priority Issues 🟠

*Should be fixed before merge*

### HIGH-001: [Issue Title]

**Location**: `path/to/file.ext:456`

**Description**:
[Detailed description of the high priority issue]

**Impact**:
[Why this is important]

**Code Context**:
```language
// Current code
```

**Suggested Fix**:
```language
// Recommended solution
```

---

## Medium Priority Issues 🟡

*Recommend fixing*

### MED-001: [Issue Title]

**Location**: `path/to/file.ext:789`

**Description**:
[Detailed description]

**Suggestion**:
[How to improve]

---

## Low Priority Issues 🟢

*Optional improvements*

### LOW-001: [Issue Title]

**Location**: `path/to/file.ext:101`

**Description**:
[Minor issue description]

**Suggestion**:
[Minor improvement suggestion]

---

## Suggestions ℹ️

*Informational notes and best practice reminders*

### INFO-001: [Suggestion Title]

**Location**: `path/to/file.ext`

**Note**:
[Informational note or best practice reminder]

---

## File-by-File Review

### `path/to/file1.ext`

| Line | Severity | Issue | Suggestion |
|------|----------|-------|------------|
| 10 | Medium | Missing null check | Add null validation |
| 25 | Low | Magic number | Extract to constant |
| 42 | Info | Consider caching | Performance optimization |

### `path/to/file2.ext`

| Line | Severity | Issue | Suggestion |
|------|----------|-------|------------|
| ... | ... | ... | ... |

---

## Testing Assessment

### Test Coverage

| Area | Coverage | Notes |
|------|----------|-------|
| Unit Tests | ✅/⚠️/❌ | [Comments] |
| Integration Tests | ✅/⚠️/❌ | [Comments] |
| Edge Cases | ✅/⚠️/❌ | [Comments] |
| Error Paths | ✅/⚠️/❌ | [Comments] |

### Missing Tests

- [ ] Test case for [scenario]
- [ ] Test case for [scenario]

---

## Security Considerations

### Checked Items

| Item | Status | Notes |
|------|--------|-------|
| Input Validation | ✅/⚠️/❌ | |
| Authentication | ✅/⚠️/❌ | |
| Authorization | ✅/⚠️/❌ | |
| Data Protection | ✅/⚠️/❌ | |
| Injection Prevention | ✅/⚠️/❌ | |

### Security Notes
[Any additional security considerations]

---

## Performance Considerations

### Potential Issues

| Issue | Location | Impact | Suggestion |
|-------|----------|--------|------------|
| [Issue] | `file:line` | [Impact] | [Suggestion] |

### Performance Notes
[Any performance-related observations]

---

## Positive Highlights

- ✨ [Good practice or well-implemented feature]
- ✨ [Clean code or elegant solution]
- ✨ [Good test coverage in specific area]

---

## Review Checklist

- [x] Code logic reviewed
- [x] Security implications considered
- [x] Performance impact assessed
- [x] Test coverage adequate
- [x] Documentation updated
- [x] No breaking changes (or documented)
- [x] Follows coding standards
- [x] No sensitive data exposed

---

## Overall Recommendation

| Status | Condition |
|--------|-----------|
| ✅ **Approve** | No critical/high issues, good quality |
| ⚠️ **Request Changes** | Critical or high issues need fixing |
| 💬 **Need Discussion** | Design decisions need team input |

**Recommendation**: [Approve / Request Changes / Need Discussion]

**Reasoning**:
[Explanation of the recommendation]

---

## Next Steps

1. [ ] Address critical issues
2. [ ] Address high priority issues
3. [ ] Consider medium priority suggestions
4. [ ] Update tests as needed
5. [ ] Request re-review after changes

---

## Review Metadata

| Field | Value |
|-------|-------|
| Review Duration | X minutes |
| Issues Found | X Critical, X High, X Medium, X Low |
| Files Reviewed | X |
| Review Confidence | High/Medium/Low |

---

*Generated by ASDM Code Review Toolset*
