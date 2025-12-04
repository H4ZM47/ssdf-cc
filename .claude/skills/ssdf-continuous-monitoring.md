---
name: ssdf-continuous-monitoring
description: Use to configure continuous security monitoring per SSDF. Sets up git hooks, CI/CD integration, and file watchers. Dispatches devops-engineer and security-engineer agents.
---

# SSDF Continuous Monitoring

## Overview

Configure continuous security monitoring through git hooks, CI/CD pipeline integration, and file watchers. Enables proactive security enforcement.

**SSDF Coverage:** All practices (continuous enforcement)

## When to Use

**Required for:**
- New project setup
- CI/CD pipeline configuration
- Compliance requirements

**Recommended for:**
- Team onboarding
- After process improvements
- Quarterly review

## Monitoring Configuration Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: devops-engineer**
```
Configure CI/CD security integration for:
[Repository path]

Tasks:
1. Set up git hooks
2. Configure CI/CD workflows
3. Implement file watchers
4. Configure notifications
```

**Agent 2: security-engineer**
```
Design monitoring strategy for:
[Repository path]

Tasks:
1. Define trigger conditions
2. Map checks to file patterns
3. Configure severity thresholds
4. Set up alerting
```

### Step 2: Git Hooks Setup

**Pre-commit hook:**
```bash
#!/bin/bash
# .git/hooks/pre-commit

# Check for secrets
if git diff --cached | grep -iE "(password|secret|api_key|token)\s*=" ; then
    echo "ERROR: Potential secret detected"
    exit 1
fi

# Run dependency check on changed package files
if git diff --cached --name-only | grep -qE "package.*json|requirements.*txt|go\.mod|Cargo\.toml" ; then
    echo "Running dependency security check..."
    # claude /ssdf-dependency-check --staged-only
fi

# Run secure defaults check on config changes
if git diff --cached --name-only | grep -qE "\.env|config.*\.(json|yaml|yml)" ; then
    echo "Running configuration security check..."
    # claude /ssdf-secure-defaults --staged-only
fi
```

**Pre-push hook:**
```bash
#!/bin/bash
# .git/hooks/pre-push

# Run full security review before push
echo "Running security review..."
# claude /ssdf-security-review --branch-diff
```

### Step 3: CI/CD Integration

**GitHub Actions workflow:**
```yaml
# .github/workflows/ssdf-security.yml
name: SSDF Security Checks

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  security-checks:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Dependency Check
        run: |
          # Run SSDF dependency check
          echo "Checking dependencies..."

      - name: Security Review
        run: |
          # Run SSDF security review
          echo "Running security review..."

      - name: Build Hardening Check
        run: |
          # Check build security
          echo "Checking build hardening..."
```

### Step 4: File Watchers

**Monitor configuration:**
```yaml
# .claude/ssdf/ssdf-monitors.yaml
monitors:
  dependency_change:
    patterns:
      - "package.json"
      - "package-lock.json"
      - "requirements.txt"
      - "go.mod"
      - "Cargo.toml"
    action: "/ssdf-dependency-check"

  config_change:
    patterns:
      - ".env*"
      - "config/**"
      - "*.config.js"
    action: "/ssdf-secure-defaults"

  architecture_change:
    patterns:
      - "src/api/**"
      - "src/auth/**"
    action: "/ssdf-threat-model --incremental"
```

### Step 5: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/monitoring/YYYY-MM-DD-monitoring-setup.md
```

**Report template:**
```markdown
# Continuous Monitoring Configuration

**Date:** YYYY-MM-DD
**Project:** [project name]
**Reviewer:** Claude Code (SSDF)

## Git Hooks

| Hook | Status | Checks |
|------|--------|--------|
| pre-commit | Installed | Secrets, deps |
| pre-push | Installed | Full review |

## CI/CD Integration

| Pipeline | Status | Triggers |
|----------|--------|----------|
| GitHub Actions | Active | push, PR |

## File Watchers

| Pattern | Monitor | Action |
|---------|---------|--------|
| package*.json | Dependencies | /ssdf-dependency-check |
| .env* | Config | /ssdf-secure-defaults |

## Verification

- [ ] Git hooks installed
- [ ] CI/CD workflows active
- [ ] File watchers configured
- [ ] Notifications working
```

## Commit Message Format

```
[SSDF] Configure continuous security monitoring

- Installed git hooks for pre-commit checks
- Added CI/CD security workflow
- Evidence: .claude/ssdf/evidence/monitoring/YYYY-MM-DD.md
```
