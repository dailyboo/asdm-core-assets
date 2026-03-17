# ASDM Action: PR Security Review

## Overview

This action performs a security-focused code review on a Pull Request, specifically targeting security vulnerabilities, authentication issues, and data protection concerns.

## Purpose

- Identify security vulnerabilities and risks
- Verify authentication and authorization implementation
- Check for common attack vectors
- Ensure secure data handling practices
- Provide security-specific remediation guidance

## Steps

1. **Gather Security Context**
   - Understand the security requirements of the changed components
   - Identify sensitive data flows
   - Map authentication and authorization boundaries

2. **Input Validation Analysis**
   - Check all user inputs are validated
   - Verify sanitization of untrusted data
   - Identify potential injection points

3. **Authentication & Authorization Review**
   - Verify authentication mechanisms
   - Check authorization logic
   - Review session management
   - Assess privilege escalation risks

4. **Data Protection Analysis**
   - Review sensitive data handling
   - Check encryption implementations
   - Verify secure data storage
   - Assess data exposure risks

5. **Common Vulnerability Scan**
   - SQL Injection checks
   - XSS vulnerability detection
   - CSRF token validation
   - Command injection analysis
   - Path traversal detection

6. **Generate Security Report**
   - Document all security findings
   - Provide severity classification
   - Suggest remediation steps
   - Reference security best practices

## Input

- PR number or URL
- Target repository context
- Optional: specific security concerns to focus on

## Output

- Security-focused review report with:
  - Vulnerability summary
  - Detailed findings with CVSS-like severity
  - Remediation recommendations
  - Code examples for fixes
  - Security best practices references

## Language Detection

Before performing review, detect and use the current environment's response language:

1. **Detect Response Language**: Analyze the environment settings to determine the primary language
2. **Apply Language Consistency**: Ensure all generated content uses the detected language

**IMPORTANT**: The language detection is the FIRST step before any review content generation.

## Security Checklist

### Authentication
- [ ] Password handling is secure (hashing, no plaintext)
- [ ] Session tokens are properly generated and validated
- [ ] Multi-factor authentication implemented correctly
- [ ] Account lockout mechanism exists
- [ ] Password reset flow is secure

### Authorization
- [ ] Role-based access control implemented
- [ ] Permission checks are consistent
- [ ] No authorization bypass vulnerabilities
- [ ] Resource ownership verified
- [ ] Principle of least privilege followed

### Input Validation
- [ ] All user inputs are validated
- [ ] Input sanitization applied
- [ ] Type checking enforced
- [ ] Length limits applied
- [ ] Special characters handled

### Injection Prevention
- [ ] Parameterized queries used for database access
- [ ] Output encoding applied for XSS prevention
- [ ] Command injection prevented
- [ ] LDAP injection prevented
- [ ] No eval() or similar dangerous functions

### Data Protection
- [ ] Sensitive data encrypted at rest
- [ ] Sensitive data encrypted in transit
- [ ] No sensitive data in logs
- [ ] No sensitive data in URLs
- [ ] Proper data retention policies

### API Security
- [ ] API authentication required
- [ ] Rate limiting implemented
- [ ] Input validation on all endpoints
- [ ] Proper error handling without info leakage
- [ ] CORS configured correctly

## Severity Classification

| Severity | CVSS Range | Description |
|----------|------------|-------------|
| Critical | 9.0-10.0 | Exploitable without user interaction, full system compromise |
| High | 7.0-8.9 | Significant vulnerability, potential data breach |
| Medium | 4.0-6.9 | Moderate risk, requires specific conditions |
| Low | 0.1-3.9 | Minor security issue, limited impact |
| Info | N/A | Security best practice recommendation |

## Usage

This action is invoked when a user needs security-focused code review on a Pull Request.

### Example Invocation

```shell
# Security review of PR #123
Follow the instructions in .asdm/toolsets/code-review/actions/asdm-pr-security-review.md
```

## Output Format

```markdown
# Security Review Report

## Executive Summary
[Brief overview of security posture]

## Critical Vulnerabilities
[Immediate security risks requiring urgent fix]

## High Severity Issues
[Significant security concerns]

## Medium Severity Issues
[Moderate security risks]

## Low Severity Issues
[Minor security improvements]

## Security Recommendations
[Best practice suggestions]

## Remediation Summary
[Quick reference for all required fixes]
```

## Notes

- Focus on actionable security feedback
- Provide specific remediation code examples
- Reference OWASP Top 10 and CWE databases
- Consider threat modeling context
- Balance security with usability
