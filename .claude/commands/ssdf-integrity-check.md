---
description: Verify code repository and release integrity (PS.1/PS.2)
arguments:
  - name: scope
    description: Scope of check - repo, release, or all (default: all)
    required: false
---

# SSDF Integrity Check

Use the `ssdf-integrity-check` skill to verify code and release integrity.

**Scope:** $ARGUMENTS.scope (or all if not specified)

## Process

1. Dispatch agents:
   - `security-auditor`: Integrity verification, signing validation
   - `git-workflow-manager`: Git configuration, branch protection

2. Verify commit signing configuration and compliance
3. Check branch protection rules via GitHub/GitLab API
4. Validate release tag signatures and checksums
5. Generate evidence file in `.claude/ssdf/evidence/integrity/`

## Output Requirements

- Integrity verification evidence file
- Commit signing compliance percentage
- Branch protection status table
- Release integrity verification results
- SSDF references (PS.1.1, PS.2.1) in commit messages
