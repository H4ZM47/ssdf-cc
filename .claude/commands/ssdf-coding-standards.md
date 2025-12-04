---
description: Validate code against secure coding standards (PW.5)
arguments:
  - name: scope
    description: Files or directory to check (default: staged changes)
    required: false
  - name: standard
    description: Standard to check - owasp, cert, cwe, or all (default: all)
    required: false
---

# SSDF Coding Standards Validation

Use the `ssdf-coding-standards` skill to validate secure coding practices.

**Scope:** $ARGUMENTS.scope (or staged changes if not specified)
**Standard:** $ARGUMENTS.standard (or all if not specified)

## Process

1. Dispatch agents:
   - `code-reviewer`: Secure coding pattern analysis
   - `security-auditor`: Vulnerability pattern detection

2. Scan code for secure coding violations
3. Check against OWASP, CERT, and CWE guidelines
4. Identify language-specific anti-patterns
5. Generate evidence file in `.claude/ssdf/evidence/coding-standards/`

## Output Requirements

- Violations by severity and standard
- Code location with fix recommendations
- SSDF references (PW.5.1, PW.5.2) in commit messages
