---
description: Audit development toolchain security (PO.3)
arguments:
  - name: scope
    description: Scope - compilers, packages, build, ide, or all (default: all)
    required: false
---

# SSDF Toolchain Audit

Use the `ssdf-toolchain-audit` skill to verify development tool security.

**Scope:** $ARGUMENTS.scope (or all if not specified)

## Process

1. Dispatch agents:
   - `devops-engineer`: Tool version and configuration analysis
   - `security-auditor`: Security assessment of toolchain

2. Inventory all development tools
3. Check for known vulnerabilities in tools
4. Verify tool configurations are secure
5. Generate evidence file in `.claude/ssdf/evidence/toolchain/`

## Output Requirements

- Toolchain inventory with versions
- Vulnerability assessment per tool
- Configuration security findings
- SSDF references (PO.3.1, PO.3.2) in commit messages
