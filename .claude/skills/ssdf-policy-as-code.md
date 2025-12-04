---
name: ssdf-policy-as-code
description: Use to manage security policies as code per SSDF PO.1. Enables declarative policy definitions with enforcement and exception management. Dispatches compliance-auditor and security-auditor agents.
---

# SSDF Policy as Code (PO.1)

## Overview

Define security policies declaratively in code, enabling version control, automated enforcement, and consistent compliance across teams.

**SSDF References:**
- PO.1.1 (Specify Security Requirements)
- PO.1.2 (Communicate Requirements)

## When to Use

**Required for:**
- Establishing security baseline
- Compliance documentation
- Audit preparation

**Recommended for:**
- New project setup
- Policy updates
- Exception management

## Policy as Code Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: compliance-auditor**
```
Validate security policy for:
[Repository path]

Tasks:
1. Parse policy file
2. Check against current state
3. Identify violations
4. Apply exceptions
5. Calculate compliance score
```

**Agent 2: security-auditor**
```
Assess policy effectiveness for:
[Repository path]

Tasks:
1. Review rule coverage
2. Identify gaps
3. Suggest improvements
4. Verify enforcement
```

### Step 2: Policy Schema

**Policy file location:**
```
.claude/ssdf/security-policy.yaml
```

**Policy structure:**
```yaml
version: "1.0"
organization: "Example Corp"

enforcement:
  mode: enforce  # enforce | warn | audit
  fail_on: high  # critical | high | medium | low

policies:
  PW.5.1:  # Secure Coding
    input_validation:
      required: true
    banned_functions:
      javascript: [eval, Function, innerHTML]
      python: [eval, exec, pickle.loads]

  PS.1.1:  # Code Protection
    commit_signing:
      required: true
    branch_protection:
      main:
        require_review: true
        require_ci: true

  PW.9.1:  # Secure Defaults
    debug_mode:
      production: false
    cookie_secure:
      required: true

exceptions:
  - id: "EXC-001"
    rule: "PW.5.1.banned_functions"
    scope: "src/legacy/"
    reason: "Legacy code, sandboxed"
    expires: "2024-12-31"
    approved_by: "security@example.com"
```

### Step 3: Initialize Policy

**Create default policy:**
```bash
# claude /ssdf-policy-as-code --action=init
```

Creates `.claude/ssdf/security-policy.yaml` with defaults.

### Step 4: Validate Policy

**Run validation:**
```bash
# claude /ssdf-policy-as-code --action=validate
```

**Validation output:**
```
Policy Validation Results:
- Rules evaluated: 25
- Violations found: 3
- Exceptions applied: 1
- Compliance: 92%

Violations:
1. PW.5.1: eval() usage in src/utils/parser.js:45
2. PS.1.1: Unsigned commits (last 5)
3. PW.9.1: Debug mode enabled in config/prod.yaml
```

### Step 5: Exception Management

**Adding exceptions:**
```yaml
exceptions:
  - id: "EXC-002"
    rule: "PS.1.1.commit_signing"
    scope: "bot/*"
    reason: "Automated bot commits verified via CI"
    expires: null  # No expiration
    approved_by: "security@example.com"
    approved_date: "2024-01-15"
```

### Step 6: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/policy/YYYY-MM-DD-policy-validation.md
```

**Report template:**
```markdown
# Policy Validation Report

**Date:** YYYY-MM-DD
**Policy Version:** [version]
**Project:** [project name]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PO.1.1, PO.1.2

## Summary

| Metric | Value |
|--------|-------|
| Rules Evaluated | X |
| Violations | Y |
| Exceptions Applied | Z |
| Compliance Score | XX% |

## Enforcement Mode

**Current:** [enforce/warn/audit]
**Fail Threshold:** [severity]

## Violations

| Rule | Location | Severity | Status |
|------|----------|----------|--------|
| PW.5.1 | file:line | High | Open |

## Active Exceptions

| ID | Rule | Scope | Expires | Approved By |
|----|------|-------|---------|-------------|
| EXC-001 | PW.5.1 | src/legacy/ | 2024-12-31 | security@ |

## Policy Coverage

| Practice | Rules | Pass | Fail |
|----------|-------|------|------|
| PO | X | Y | Z |
| PS | X | Y | Z |
| PW | X | Y | Z |
| RV | X | Y | Z |

## Recommendations

1. [Address violations]
2. [Update expiring exceptions]

## Sign-Off

- [ ] All critical violations addressed
- [ ] Exceptions reviewed and valid
- [ ] Policy up to date
```

## Commit Message Format

```
[SSDF:PO.1.1] Update security policy

- Added rule for [practice]
- Updated exception [ID]
- Evidence: .claude/ssdf/evidence/policy/YYYY-MM-DD.md
```

## Enforcement Modes

| Mode | Behavior |
|------|----------|
| enforce | Block on violations above threshold |
| warn | Log violations but don't block |
| audit | Silent logging only |
