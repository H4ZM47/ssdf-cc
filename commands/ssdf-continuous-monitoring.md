---
description: Configure continuous security monitoring (all practices)
arguments:
  - name: action
    description: Action - setup, status, or disable (default: status)
    required: false
---

# SSDF Continuous Monitoring

Use the `ssdf-continuous-monitoring` skill to configure automated security checks.

**Action:** $ARGUMENTS.action (or status if not specified)

## Process

1. Dispatch agents:
   - `devops-engineer`: CI/CD integration
   - `security-engineer`: Monitoring configuration

2. Configure git hooks for pre-commit checks
3. Set up CI/CD pipeline integration
4. Configure file watchers for relevant changes
5. Generate evidence file in `.claude/ssdf/evidence/monitoring/`

## Output Requirements

- Monitoring configuration status
- Git hooks installation report
- CI/CD integration status
- SSDF references in commit messages
