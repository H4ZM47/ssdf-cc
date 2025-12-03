---
description: Run SSDF security review (PW.7/PW.8) on code changes
arguments:
  - name: scope
    description: Files, directory, or PR to review (default: staged changes)
    required: false
---

# SSDF Security Review

Use the `ssdf-security-review` skill to perform comprehensive security review.

**Scope:** $ARGUMENTS.scope (or staged changes if not specified)

## Process

1. Dispatch agents in parallel:
   - `security-auditor`: OWASP Top 10, vulnerabilities, secrets
   - `code-reviewer`: Secure coding practices, input validation

2. Complete OWASP Top 10 checklist
3. Verify secure coding practices
4. Document findings with severity ratings
5. Generate evidence file in `.claude/ssdf/evidence/security-reviews/`

## Output Requirements

- Security review evidence file
- Issues table with severity, location, and remediation
- Sign-off checklist for High/Critical issues
- SSDF references (PW.7.2, PW.8.2) in commit messages
