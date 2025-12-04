---
name: ssdf-coding-standards
description: Use to validate code against secure coding standards per SSDF PW.5. Checks OWASP, CERT, CWE guidelines. Dispatches code-reviewer and security-auditor agents.
---

# SSDF Coding Standards Validation (PW.5)

## Overview

Validate code against established secure coding standards including OWASP guidelines, CERT secure coding standards, and CWE weakness patterns.

**SSDF References:**
- PW.5.1 (Follow Secure Coding Practices)
- PW.5.2 (Adhere to Secure Design Practices)

## When to Use

**Required for:**
- Code reviews
- Pre-commit validation
- Security assessments

**Recommended for:**
- New feature development
- Legacy code audits
- Developer training

## Coding Standards Validation Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: code-reviewer**
```
Review code for secure coding practices:
[Files/directory to check]

Check for:
1. Input validation patterns
2. Output encoding
3. Error handling
4. Authentication/authorization patterns
5. Cryptographic usage
6. Resource management
```

**Agent 2: security-auditor**
```
Security vulnerability scan for:
[Files/directory to check]

Check for:
1. OWASP Top 10 violations
2. CWE weakness patterns
3. Language-specific vulnerabilities
4. Hardcoded secrets
5. Injection risks
```

### Step 2: Standards Reference

#### OWASP Secure Coding Practices

| Category | Check |
|----------|-------|
| Input Validation | All input validated, whitelisting preferred |
| Output Encoding | Context-appropriate encoding |
| Authentication | Strong password requirements, MFA support |
| Session Management | Secure session handling |
| Access Control | Least privilege, deny by default |
| Cryptographic Practices | Strong algorithms, proper key management |
| Error Handling | No sensitive data in errors |
| Data Protection | Encryption at rest and in transit |
| Communication Security | TLS 1.2+, certificate validation |
| System Configuration | Secure defaults, minimal footprint |

#### CERT Secure Coding Standards

**By Language:**

| Language | Standard |
|----------|----------|
| C | SEI CERT C Coding Standard |
| C++ | SEI CERT C++ Coding Standard |
| Java | SEI CERT Oracle Coding Standard for Java |
| Perl | SEI CERT Perl Coding Standard |

#### CWE Top 25 Weaknesses

| Rank | CWE | Name |
|------|-----|------|
| 1 | CWE-787 | Out-of-bounds Write |
| 2 | CWE-79 | Cross-site Scripting (XSS) |
| 3 | CWE-89 | SQL Injection |
| 4 | CWE-416 | Use After Free |
| 5 | CWE-78 | OS Command Injection |
| 6 | CWE-20 | Improper Input Validation |
| 7 | CWE-125 | Out-of-bounds Read |
| 8 | CWE-22 | Path Traversal |
| 9 | CWE-352 | Cross-Site Request Forgery |
| 10 | CWE-434 | Unrestricted Upload |

### Step 3: Language-Specific Checks

#### JavaScript/TypeScript
```javascript
// INSECURE patterns to detect:
eval(userInput)                    // CWE-95: Eval Injection
document.innerHTML = userInput     // CWE-79: XSS
new Function(userInput)            // CWE-95: Code Injection
$.html(userInput)                  // CWE-79: DOM XSS
child_process.exec(userInput)      // CWE-78: Command Injection
fs.readFile(userInput)             // CWE-22: Path Traversal

// SECURE patterns:
DOMPurify.sanitize(userInput)      // Sanitized output
parameterizedQuery(sql, params)    // Prepared statements
path.join(__dirname, validated)    // Safe path construction
```

#### Python
```python
# INSECURE patterns to detect:
eval(user_input)                   # CWE-95: Eval Injection
exec(user_input)                   # CWE-95: Code Injection
os.system(user_input)              # CWE-78: Command Injection
pickle.loads(user_input)           # CWE-502: Deserialization
query = f"SELECT * FROM {input}"   # CWE-89: SQL Injection
open(user_path)                    # CWE-22: Path Traversal

# SECURE patterns:
subprocess.run(cmd, shell=False)   # No shell injection
cursor.execute(query, params)      # Parameterized query
os.path.realpath(path)             # Path validation
```

#### Java
```java
// INSECURE patterns to detect:
Runtime.exec(userInput)            // CWE-78: Command Injection
stmt.executeQuery(userSql)         // CWE-89: SQL Injection
new ObjectInputStream(untrusted)   // CWE-502: Deserialization
response.getWriter().print(input)  // CWE-79: XSS

// SECURE patterns:
PreparedStatement with params      // Parameterized queries
ESAPI.encoder().encodeForHTML()    // Output encoding
ProcessBuilder with argument list  // Safe process execution
```

#### Go
```go
// INSECURE patterns to detect:
exec.Command("sh", "-c", userInput) // CWE-78: Command Injection
template.HTML(userInput)            // CWE-79: XSS
sql.Query(userQuery)                // CWE-89: SQL Injection

// SECURE patterns:
exec.Command(cmd, args...)          // Argument list
html/template (auto-escaping)       // Safe templating
db.Query(query, args...)            // Parameterized
```

### Step 4: Pattern Detection

**Search for dangerous patterns:**
```bash
# JavaScript/TypeScript
grep -rn "eval\s*(" --include="*.js" --include="*.ts"
grep -rn "innerHTML\s*=" --include="*.js" --include="*.ts"
grep -rn "dangerouslySetInnerHTML" --include="*.jsx" --include="*.tsx"

# Python
grep -rn "eval\s*(" --include="*.py"
grep -rn "exec\s*(" --include="*.py"
grep -rn "os\.system\s*(" --include="*.py"

# SQL Injection patterns (all languages)
grep -rn "SELECT.*\+.*\|SELECT.*%" --include="*.py" --include="*.js" --include="*.java"
```

### Step 5: Input Validation Check

**Verify input validation at boundaries:**

| Boundary | Validation Required |
|----------|---------------------|
| HTTP Parameters | Type, length, format |
| File Uploads | Type, size, content |
| Database Inputs | Parameterization |
| Command Arguments | Whitelisting |
| File Paths | Canonicalization |
| Deserialization | Type constraints |

### Step 6: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/coding-standards/YYYY-MM-DD-coding-standards.md
```

**Report template:**
```markdown
# Secure Coding Standards Report

**Date:** YYYY-MM-DD
**Project:** [project name]
**Scope:** [files/directories checked]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PW.5.1, PW.5.2

## Executive Summary

| Standard | Violations | Critical | High | Medium | Low |
|----------|------------|----------|------|--------|-----|
| OWASP | X | | | | |
| CERT | X | | | | |
| CWE | X | | | | |
| **Total** | X | Y | Z | A | B |

## Files Analyzed

| File | Lines | Violations |
|------|-------|------------|
| src/api/handler.js | 250 | 3 |
| src/db/queries.py | 180 | 1 |

## Violations by Category

### Input Validation (CWE-20)
| Location | Issue | Severity | Fix |
|----------|-------|----------|-----|
| file.js:45 | Unvalidated user input | High | Add validation |

### Injection (CWE-89, CWE-78)
| Location | Issue | Severity | Fix |
|----------|-------|----------|-----|

### XSS (CWE-79)
| Location | Issue | Severity | Fix |
|----------|-------|----------|-----|

### Authentication (CWE-287)
| Location | Issue | Severity | Fix |
|----------|-------|----------|-----|

### Cryptography (CWE-327)
| Location | Issue | Severity | Fix |
|----------|-------|----------|-----|

## OWASP Top 10 Coverage

| Category | Status | Findings |
|----------|--------|----------|
| A01:2021 Broken Access Control | PASS/FAIL | |
| A02:2021 Cryptographic Failures | PASS/FAIL | |
| A03:2021 Injection | PASS/FAIL | |
| A04:2021 Insecure Design | PASS/FAIL | |
| A05:2021 Security Misconfiguration | PASS/FAIL | |
| A06:2021 Vulnerable Components | PASS/FAIL | |
| A07:2021 Auth Failures | PASS/FAIL | |
| A08:2021 Data Integrity Failures | PASS/FAIL | |
| A09:2021 Logging Failures | PASS/FAIL | |
| A10:2021 SSRF | PASS/FAIL | |

## Detailed Findings

### Finding 1: [Title]
**Location:** file.js:45
**Severity:** High
**CWE:** CWE-89 (SQL Injection)
**Standard:** OWASP A03:2021

**Vulnerable Code:**
```javascript
const query = `SELECT * FROM users WHERE id = ${userId}`;
```

**Recommended Fix:**
```javascript
const query = 'SELECT * FROM users WHERE id = ?';
db.query(query, [userId]);
```

## Recommendations

### Immediate (Critical/High)
1. [Fix injection vulnerabilities]

### Short-term (Medium)
1. [Add input validation]

### Long-term (Low)
1. [Implement secure coding training]

## Compliance Checklist

- [ ] No SQL injection vulnerabilities
- [ ] No XSS vulnerabilities
- [ ] No command injection vulnerabilities
- [ ] Input validation at all boundaries
- [ ] Output encoding where required
- [ ] Secure error handling
- [ ] No hardcoded secrets

## Sign-Off

- [ ] All critical issues resolved
- [ ] Code meets secure coding baseline
- [ ] Evidence archived
```

## Integration

**Create issue for violations:**
```bash
bd create "Security: [Coding Standard Violation]" --type security -d "Found during SSDF PW.5 coding standards check. See evidence: .claude/ssdf/evidence/coding-standards/[date].md"
```

## Commit Message Format

```
[SSDF:PW.5.1] Fix secure coding violations

- Fixed SQL injection in user query
- Added input validation for API endpoints
- Evidence: .claude/ssdf/evidence/coding-standards/YYYY-MM-DD.md
```

## Severity Definitions

| Severity | Definition | SLA |
|----------|------------|-----|
| Critical | Exploitable, data breach risk | Immediate |
| High | Likely exploitable | 24 hours |
| Medium | Potentially exploitable | 1 week |
| Low | Defense in depth | 1 month |
