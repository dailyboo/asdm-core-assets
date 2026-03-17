# Security Review Specification

**Document Version**: 1.0  
**Last Updated**: 2026-03-16

## Overview

This specification defines the security review checklist and guidelines for identifying and remediating security vulnerabilities in code changes.

## Security Review Framework

### 1. Authentication Security

#### Checklist Items

| Item | Description | Risk Level |
|------|-------------|------------|
| Password Storage | Passwords must be hashed with strong algorithms (bcrypt, Argon2) | Critical |
| Session Management | Sessions must have proper timeout and regeneration | Critical |
| Token Security | Auth tokens must be properly generated, stored, and invalidated | Critical |
| MFA Implementation | Multi-factor auth must be implemented correctly | High |
| Account Lockout | Failed login attempts must trigger lockout mechanism | High |
| Password Reset | Reset flow must use secure, time-limited tokens | Critical |

#### Common Vulnerabilities

```
❌ BAD: Storing passwords in plaintext
password = request.getParameter("password");
user.setPassword(password);

✅ GOOD: Using bcrypt hashing
String hashedPassword = passwordEncoder.encode(password);
user.setPassword(hashedPassword);
```

### 2. Authorization Security

#### Checklist Items

| Item | Description | Risk Level |
|------|-------------|------------|
| RBAC Implementation | Role-based access control properly enforced | Critical |
| Resource Ownership | Users can only access their own resources | Critical |
| Privilege Escalation | No paths to elevate privileges | Critical |
| API Authorization | All API endpoints have proper authorization | Critical |
| Insecure Direct Object Reference | IDs must be validated against ownership | Critical |

#### Common Vulnerabilities

```
❌ BAD: No authorization check
@GetMapping("/user/{id}/profile")
public UserProfile getProfile(@PathVariable Long id) {
    return userService.getProfile(id); // Anyone can access any user's profile
}

✅ GOOD: Authorization check
@GetMapping("/user/{id}/profile")
public UserProfile getProfile(@PathVariable Long id, Principal principal) {
    if (!userService.hasAccess(principal.getName(), id)) {
        throw new AccessDeniedException();
    }
    return userService.getProfile(id);
}
```

### 3. Input Validation

#### Checklist Items

| Item | Description | Risk Level |
|------|-------------|------------|
| Server-Side Validation | All inputs validated on server | Critical |
| Input Sanitization | Untrusted data is sanitized | Critical |
| Type Checking | Correct type enforcement | High |
| Length Limits | Input length restrictions applied | Medium |
| Special Characters | Dangerous characters handled | High |
| File Upload Validation | Uploaded files are validated | Critical |

#### Validation Patterns

```
❌ BAD: Direct use of user input
String query = "SELECT * FROM users WHERE id = " + userId;

✅ GOOD: Parameterized query
PreparedStatement stmt = conn.prepareStatement("SELECT * FROM users WHERE id = ?");
stmt.setString(1, userId);
```

### 4. Injection Prevention

#### SQL Injection

| Pattern | Risk | Prevention |
|---------|------|------------|
| String concatenation in SQL | Critical | Use parameterized queries |
| Dynamic table/column names | High | Whitelist allowed values |
| Raw SQL in ORM | High | Use ORM query builders |

```
❌ BAD: SQL Injection vulnerable
String sql = "SELECT * FROM users WHERE name = '" + userName + "'";

✅ GOOD: Parameterized query
String sql = "SELECT * FROM users WHERE name = ?";
PreparedStatement ps = connection.prepareStatement(sql);
ps.setString(1, userName);
```

#### Cross-Site Scripting (XSS)

| Pattern | Risk | Prevention |
|---------|------|------------|
| Unescaped output | Critical | Use output encoding |
| InnerHTML assignment | Critical | Use textContent or sanitize |
| URL parameters in HTML | High | Validate and encode |

```
❌ BAD: XSS vulnerable
element.innerHTML = userInput;

✅ GOOD: Safe text insertion
element.textContent = userInput;
// Or use sanitization library
element.innerHTML = DOMPurify.sanitize(userInput);
```

#### Command Injection

| Pattern | Risk | Prevention |
|---------|------|------------|
| Shell command with user input | Critical | Avoid shell execution |
| Dynamic command building | Critical | Use allowlist for commands |
| File path in commands | High | Validate and sanitize paths |

```
❌ BAD: Command Injection vulnerable
Runtime.getRuntime().exec("ping " + host);

✅ GOOD: Safe command execution
ProcessBuilder pb = new ProcessBuilder("ping", "-c", "1", validatedHost);
```

### 5. Data Protection

#### Checklist Items

| Item | Description | Risk Level |
|------|-------------|------------|
| Encryption at Rest | Sensitive data encrypted | Critical |
| Encryption in Transit | TLS for all communications | Critical |
| Key Management | Secure key storage and rotation | Critical |
| Data Masking | Sensitive data masked in logs | High |
| PII Handling | PII properly protected | Critical |

#### Sensitive Data Handling

```
❌ BAD: Logging sensitive data
logger.info("User login: " + username + ", password: " + password);

✅ GOOD: Masking sensitive data
logger.info("User login: " + username);

// Or mask PII
logger.info("Credit card: " + maskCardNumber(cardNumber));
// Output: Credit card: ****-****-****-1234
```

### 6. API Security

#### Checklist Items

| Item | Description | Risk Level |
|------|-------------|------------|
| Authentication Required | All APIs require auth (except public) | Critical |
| Rate Limiting | API rate limits enforced | High |
| Input Validation | All API inputs validated | Critical |
| Error Handling | No sensitive info in errors | High |
| CORS Configuration | Proper CORS headers | High |
| API Versioning | Proper versioning strategy | Medium |

#### Secure API Patterns

```
❌ BAD: Exposing internal errors
@ExceptionHandler(Exception.class)
public ResponseEntity<String> handleError(Exception e) {
    return ResponseEntity.status(500).body(e.getMessage()); // May expose stack trace
}

✅ GOOD: Safe error handling
@ExceptionHandler(Exception.class)
public ResponseEntity<ErrorResponse> handleError(Exception e) {
    log.error("Internal error", e);
    return ResponseEntity.status(500)
        .body(new ErrorResponse("INTERNAL_ERROR", "An unexpected error occurred"));
}
```

### 7. Dependency Security

#### Checklist Items

| Item | Description | Risk Level |
|------|-------------|------------|
| Dependency Audit | No known vulnerabilities in dependencies | Critical |
| Version Management | Dependencies up to date | High |
| Dependency Lock | Lock files used and committed | Medium |
| License Compliance | Dependencies have compatible licenses | Medium |

## Severity Classification

| Severity | CVSS Range | Description | Timeline |
|----------|------------|-------------|----------|
| Critical | 9.0-10.0 | Immediate exploitation possible, severe impact | Fix immediately |
| High | 7.0-8.9 | Significant vulnerability, high impact | Fix within 24h |
| Medium | 4.0-6.9 | Moderate risk, requires conditions | Fix within sprint |
| Low | 0.1-3.9 | Minor security issue | Fix when convenient |
| Info | N/A | Best practice recommendation | Consider fixing |

## Security Review Report Template

```markdown
# Security Review Report

## Executive Summary
[Brief summary of security posture]

## Critical Findings
### [Vulnerability Name]
- **CVSS Score**: X.X
- **Location**: `file:line`
- **Description**: [What's wrong]
- **Impact**: [Potential consequences]
- **Remediation**: [How to fix]
```suggestion
[code example]
```

## High Severity Findings
[Same format]

## Medium Severity Findings
[Same format]

## Low Severity Findings
[Same format]

## Security Recommendations
[Best practice suggestions]

## References
- [CWE-XXX](https://cwe.mitre.org/data/definitions/XXX.html)
- [OWASP Reference](https://owasp.org/...)

## Compliance Notes
[Any regulatory compliance considerations]
```

## References

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [OWASP API Security Top 10](https://owasp.org/www-project-api-security/)
- [SANS Top 25](https://www.sans.org/top25-software-errors/)
