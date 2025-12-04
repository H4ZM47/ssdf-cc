---
description: Define and validate security policies as code (PO.1)
arguments:
  - name: action
    description: Action - validate, init, or report (default: validate)
    required: false
---

# SSDF Policy as Code

Use the `ssdf-policy-as-code` skill to manage security policies declaratively.

**Action:** $ARGUMENTS.action (or validate if not specified)

## Process

1. Dispatch agents:
   - `compliance-auditor`: Policy validation
   - `security-auditor`: Rule assessment

2. Load security policy from .claude/ssdf/security-policy.yaml
3. Validate current state against policy
4. Report violations and exceptions
5. Generate evidence file in `.claude/ssdf/evidence/policy/`

## Output Requirements

- Policy validation results
- Violation report with locations
- Exception tracking
- SSDF references (PO.1.1, PO.1.2) in commit messages
