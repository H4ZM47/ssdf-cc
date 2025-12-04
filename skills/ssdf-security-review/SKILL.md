---
name: ssdf-security-review
description: Use before commits, PRs, or releases - comprehensive security code review per SSDF PW.7 and PW.8. Dispatches security-auditor and code-reviewer agents in parallel.
---

# SSDF Security Review (PW.7, PW.8)

## Overview

Comprehensive security review of code changes before commit or release. Combines automated analysis with expert review via sub-agents.

**SSDF References:**
- PW.7.1 (Determine Review Methods)
- PW.7.2 (Perform Code Review)
- PW.8.1 (Determine Testing Methods)
- PW.8.2 (Perform Security Testing)

## When to Use

**Required before:**
- Any commit to main/production branches
- Pull request creation
- Release preparation
- Security-sensitive code changes

**Recommended for:**
- All code changes (proportional review depth)

## Review Process

### Step 1: Dispatch Agents in Parallel

Use Task tool with parallel agent dispatch:

**Agent 1: security-auditor**
```
Review the following code changes for security vulnerabilities:
[Include diff or file paths]

Check for:
1. OWASP Top 10 vulnerabilities
2. Hardcoded secrets or credentials
3. Injection vulnerabilities (SQL, command, XSS)
4. Authentication/authorization flaws
5. Cryptographic weaknesses
6. Sensitive data exposure
7. Security misconfiguration
```

**Agent 2: code-reviewer**
```
Review the following code for secure coding practices:
[Include diff or file paths]

Verify:
1. Input validation on all external data
2. Output encoding for all rendered data
3. Proper error handling (no sensitive data in errors)
4. Secure defaults
5. Principle of least privilege
6. Defense in depth
```

### Step 2: OWASP Top 10 Checklist

| # | Vulnerability | Check | Status |
|---|---------------|-------|--------|
| A01 | Broken Access Control | Authorization on all endpoints | |
| A02 | Cryptographic Failures | Proper encryption, no weak algorithms | |
| A03 | Injection | Parameterized queries, input sanitization | |
| A04 | Insecure Design | Threat model reviewed | |
| A05 | Security Misconfiguration | Secure defaults, no debug in prod | |
| A06 | Vulnerable Components | Dependencies scanned | |
| A07 | Auth Failures | Strong auth, session management | |
| A08 | Data Integrity Failures | Signed updates, integrity checks | |
| A09 | Logging Failures | Security events logged, no sensitive data | |
| A10 | SSRF | URL validation, allowlists | |

### Step 3: Secure Coding Verification

**Input Handling:**
- [ ] All user input validated
- [ ] Type checking enforced
- [ ] Length limits applied
- [ ] Allowlists preferred over denylists

**Output Handling:**
- [ ] HTML output encoded
- [ ] JSON properly serialized
- [ ] SQL uses parameterized queries
- [ ] Commands use safe APIs

**Error Handling:**
- [ ] Errors caught and handled
- [ ] No stack traces to users
- [ ] No sensitive data in error messages
- [ ] Logging captures errors securely

**Authentication/Authorization:**
- [ ] Auth required on protected endpoints
- [ ] Session tokens secure (httpOnly, secure, sameSite)
- [ ] Password requirements enforced
- [ ] Rate limiting on auth endpoints

### Step 4: Document Findings

Create evidence file:
```
.claude/ssdf/evidence/security-reviews/YYYY-MM-DD-[context].md
```

**Required sections:**
```markdown
# Security Review: [Context]

**Date:** YYYY-MM-DD
**Reviewer:** Claude Code (SSDF)
**Scope:** [files/features reviewed]

## Agent Analysis

### Security Auditor Findings
[Paste agent results]

### Code Reviewer Findings
[Paste agent results]

## Checklist Results
[OWASP Top 10 and secure coding checklists]

## Issues Found
| Severity | Issue | Location | Remediation |
|----------|-------|----------|-------------|
| [H/M/L]  | [desc]| [file:line] | [fix] |

## Sign-Off
- [ ] All High/Critical issues resolved
- [ ] Medium issues tracked or justified
- [ ] Evidence documented
```

### Step 5: Resolve or Track Issues

**Critical/High severity:** Must fix before merge
**Medium severity:** Fix or create tracking issue with justification
**Low severity:** Document and address in future iteration

## Pre-Commit Checklist

Before committing security-sensitive code:

- [ ] Security review completed
- [ ] No hardcoded secrets (use secrets scanner)
- [ ] Input validation in place
- [ ] Error handling doesn't leak info
- [ ] Dependencies checked
- [ ] Evidence documented
- [ ] SSDF references in commit message

## Commit Message Format

```
[SSDF:PW.7.2] Add user authentication endpoint

Security Review:
- Threat model: .claude/ssdf/evidence/threat-models/2024-12-03-auth.md
- Code review: .claude/ssdf/evidence/security-reviews/2024-12-03-auth.md
- No critical/high issues found
- OWASP checklist passed
```

## Integration

**Create issue for deferred fixes:**
```bash
bd create "Security: [Issue Description]" --type security -d "Found during SSDF PW.7 review. Severity: [level]"
```

## Secrets Scanning

Always verify no secrets in code:
```bash
# Check for common secret patterns
grep -rn "password\s*=" --include="*.py" --include="*.js"
grep -rn "api_key\s*=" --include="*.py" --include="*.js"
grep -rn "BEGIN.*PRIVATE KEY" .
```

**If secrets found:** Rotate immediately, remove from git history, update .gitignore
