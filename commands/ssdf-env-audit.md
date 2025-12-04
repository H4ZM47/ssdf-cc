---
description: Audit development/deployment environment security (PO.5)
arguments:
  - name: environment
    description: Environment to audit - dev, staging, prod, or all (default: all)
    required: false
---

# SSDF Environment Audit

Use the `ssdf-env-audit` skill to verify environment security.

**Environment:** $ARGUMENTS.environment (or all if not specified)

## Process

1. Dispatch agents:
   - `platform-engineer`: Infrastructure analysis
   - `security-engineer`: Access control review

2. Audit environment configurations
3. Check access controls and permissions
4. Verify network segmentation
5. Generate evidence file in `.claude/ssdf/evidence/environment/`

## Output Requirements

- Environment security assessment
- Access control matrix
- Network segmentation verification
- SSDF references (PO.5.1, PO.5.2) in commit messages
