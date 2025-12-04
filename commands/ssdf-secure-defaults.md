---
description: Validate application configuration against secure defaults (PW.9)
arguments:
  - name: framework
    description: Framework to check - auto, express, django, spring, rails (default: auto)
    required: false
  - name: fix
    description: Generate secure configuration patches (default: false)
    required: false
---

# SSDF Secure Defaults Validation

Use the `ssdf-secure-defaults` skill to validate configuration security.

**Framework:** $ARGUMENTS.framework (or auto-detect if not specified)
**Fix:** $ARGUMENTS.fix (or false if not specified)

## Process

1. Dispatch agents:
   - `security-auditor`: Configuration analysis and baseline validation
   - Framework-specific agent: Framework-specific security checks

2. Auto-detect framework from project structure
3. Scan all configuration files
4. Validate against security baseline
5. Check Docker/Kubernetes security settings
6. Generate evidence file in `.claude/ssdf/evidence/config/`

## Output Requirements

- Configuration security report with severity ratings
- Remediation recommendations with code snippets
- --fix generates patches for insecure settings
- SSDF references (PW.9.1, PW.9.2) in commit messages
