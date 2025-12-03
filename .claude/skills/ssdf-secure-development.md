---
name: ssdf-secure-development
description: Use proactively during all development work - enforces NIST SP 800-218 SSDF practices for secure software development. Triggers threat modeling, secure coding checks, security reviews, and dependency verification based on context.
---

# SSDF Secure Development

## Overview

This skill enforces the NIST SP 800-218 Secure Software Development Framework (SSDF) practices during development. It operates proactively based on development context.

**Framework:** NIST SP 800-218 Version 1.1
**Practice Groups:** PO (Prepare), PS (Protect), PW (Produce), RV (Respond)

## When This Skill Activates

**Automatically triggered by:**
- Creating new features or components → Threat Modeling (PW.1)
- Writing or modifying code → Secure Coding Practices (PW.5)
- Pre-commit or PR preparation → Security Review (PW.7)
- Adding/updating dependencies → Third-Party Verification (PW.4)
- Discovering bugs or vulnerabilities → Vulnerability Response (RV.1-3)

## Core Enforcement Rules

### 1. Threat Modeling Before Design (PW.1)

Before implementing any new feature or significant change:

```
STOP. Ask yourself:
- What are the security requirements for this feature?
- What threats could exploit this feature?
- What is the attack surface?
- How will sensitive data flow?
```

**Dispatch `security-auditor` agent** for threat modeling when:
- Creating new API endpoints
- Handling user input or authentication
- Processing sensitive data
- Integrating external services

**Evidence required:** Document threat model in `.claude/ssdf/evidence/threat-models/`

### 2. Secure Coding Practices (PW.5)

For ALL code changes, verify:

| Check | SSDF Reference | Required |
|-------|----------------|----------|
| Input validation | PW.5.1 | Yes |
| Output encoding | PW.5.1 | Yes |
| No unsafe functions | PW.5.1 | Yes |
| Proper error handling | PW.5.1 | Yes |
| Logging (no sensitive data) | PW.5.1 | Yes |
| Authentication/authorization | PW.1.1 | If applicable |

**Dispatch `code-reviewer` agent** for:
- Security-sensitive code sections
- Code handling credentials, tokens, or PII
- Cryptographic operations

### 3. Third-Party Component Verification (PW.4)

When adding or updating dependencies:

1. **Dispatch `dependency-manager` agent** to:
   - Check for known vulnerabilities (CVEs)
   - Verify component is actively maintained
   - Review license compatibility
   - Generate/update SBOM

2. **Document in evidence:**
   - Component name and version
   - Vulnerability scan results
   - Justification for inclusion

**SSDF References:** PW.4.1, PW.4.4, PS.3.2

### 4. Code Review Security Checks (PW.7)

Before any commit or PR:

**Dispatch `security-auditor` + `code-reviewer` agents in parallel** to verify:
- No hardcoded secrets or credentials
- No SQL injection vulnerabilities
- No XSS vulnerabilities
- No command injection
- Proper access control
- Secure session management

**Evidence required:** Security review checklist in commit message or PR description with SSDF task references.

### 5. Vulnerability Response (RV.1-3)

When a vulnerability is discovered:

1. **Create Beads issue:** `bd create "VULN: [description]" --type security`
2. **Dispatch `error-detective` agent** to trace the vulnerability
3. **Dispatch `debugger` agent** for root cause analysis
4. **Document:** Root cause, fix, and prevention measures

## Sub-Agent Dispatch Patterns

### Parallel Dispatch (Independent Tasks)
```
Use Task tool with multiple parallel calls:
- security-auditor: Threat analysis
- dependency-manager: Component verification
- code-reviewer: Code quality check
```

### Sequential Dispatch (Dependent Tasks)
```
1. error-detective: Identify vulnerability scope
2. debugger: Root cause analysis
3. code-reviewer: Verify fix
```

## Evidence Generation

All SSDF activities must generate evidence:

**Format:** Markdown reports in `.claude/ssdf/evidence/`

**Required fields:**
- SSDF Practice ID (e.g., PW.5.1)
- Date/timestamp
- Activity description
- Findings
- Actions taken
- Agent(s) used

**Commit message format:**
```
[SSDF:PW.5.1] Add input validation for user email

- Validates email format before processing
- Sanitizes input to prevent injection
- Evidence: .claude/ssdf/evidence/2024-12-03-email-validation.md
```

## Checklist Before Completion

Before marking any development task complete:

- [ ] Threat model documented (if new feature)
- [ ] Secure coding checklist passed
- [ ] Dependencies verified (if changed)
- [ ] Security review completed
- [ ] Evidence artifacts generated
- [ ] SSDF references in commit message

## Integration with Beads

Track security-related work:
```bash
bd create "Security: [task]" --type security
bd update [id] --status in_progress
bd close [id] --reason "SSDF:PW.x.x compliance verified"
```

## Red Flags - STOP Development

- Hardcoded credentials detected
- Known vulnerable dependency
- Missing input validation on user data
- Unencrypted sensitive data storage
- Missing authentication/authorization
- Disabled security features

**Action:** Stop, remediate, document, then continue.
