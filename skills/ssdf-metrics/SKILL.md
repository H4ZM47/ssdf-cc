---
name: ssdf-metrics
description: Use to generate security metrics and compliance scores per SSDF PO.4. Tracks vulnerability trends, coverage stats, and compliance over time. Dispatches data-analyst and security-auditor agents.
---

# SSDF Security Metrics (PO.4)

## Overview

Generate comprehensive security metrics dashboard including compliance scores, vulnerability trends, and coverage statistics across all SSDF practices.

**SSDF References:**
- PO.4.1 (Define Criteria for Security Checks)
- PO.4.2 (Implement Metrics and Reporting)

## When to Use

**Required for:**
- Management reporting
- Compliance audits
- Security reviews

**Recommended for:**
- Sprint retrospectives
- Quarterly planning
- Continuous improvement

## Metrics Collection Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: data-analyst**
```
Collect and analyze security metrics for:
[Repository path]

Tasks:
1. Parse all SSDF evidence files
2. Calculate compliance percentages
3. Identify trends over time
4. Generate statistical summary
5. Create visualizations
```

**Agent 2: security-auditor**
```
Assess compliance status for:
[Repository path]

Tasks:
1. Score each SSDF practice
2. Identify gaps in coverage
3. Assess risk levels
4. Compare against baselines
5. Prioritize improvements
```

### Step 2: Data Sources

**Evidence files to analyze:**
```
.claude/ssdf/evidence/
├── security-reviews/     # PW.7, PW.8
├── threat-models/        # PW.1
├── dependencies/         # PW.4, PS.3
├── integrity/            # PS.1, PS.2
├── config/               # PW.9
├── build/                # PW.6
├── toolchain/            # PO.3
├── coding-standards/     # PW.5
├── sbom/                 # PS.3.2
└── vulnerabilities/      # RV.1-3
```

**Git history analysis:**
```bash
# Count SSDF-tagged commits
git log --oneline --grep="SSDF" | wc -l

# Commits by practice
git log --oneline --grep="SSDF:PW" | wc -l
git log --oneline --grep="SSDF:PS" | wc -l
```

### Step 3: Compliance Scoring

**Practice-level scoring:**

| Practice | Weight | Metrics |
|----------|--------|---------|
| PO (Prepare Org) | 20% | Toolchain audits, training, policies |
| PS (Protect Software) | 25% | Integrity, SBOM, access control |
| PW (Produce Well-Secured) | 35% | Reviews, testing, secure coding |
| RV (Respond to Vulns) | 20% | Response time, remediation rate |

**Score calculation:**
```
Practice Score = (Completed Tasks / Total Tasks) × 100

Overall Score = Σ(Practice Score × Weight)
```

**Score thresholds:**

| Score | Rating | Color |
|-------|--------|-------|
| 90-100 | Excellent | Green |
| 75-89 | Good | Light Green |
| 60-74 | Fair | Yellow |
| 40-59 | Poor | Orange |
| 0-39 | Critical | Red |

### Step 4: Key Metrics

#### Vulnerability Metrics
- **MTTR** (Mean Time to Remediate)
- **Open vulnerabilities** by severity
- **Age of oldest** unresolved vulnerability
- **Vulnerability density** (vulns per KLOC)
- **Reintroduction rate**

#### Process Metrics
- **Review coverage** (% of changes reviewed)
- **SBOM freshness** (days since update)
- **Evidence completeness** (% of practices documented)
- **Audit readiness** score

#### Trend Metrics
- **Compliance trend** (month-over-month)
- **Vulnerability trend** (new vs resolved)
- **Risk trend** (aggregate risk score)

### Step 5: Trend Analysis

**Time-series data collection:**
```bash
# Parse evidence file dates
find .claude/ssdf/evidence -name "*.md" | while read f; do
  date=$(basename "$f" | grep -oE "[0-9]{4}-[0-9]{2}-[0-9]{2}")
  type=$(dirname "$f" | xargs basename)
  echo "$date,$type"
done | sort
```

**Trend indicators:**
- ↑ Improving (>5% increase)
- → Stable (±5%)
- ↓ Declining (>5% decrease)

### Step 6: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/metrics/YYYY-MM-DD-metrics.md
```

**Report template:**
```markdown
# SSDF Security Metrics Report

**Date:** YYYY-MM-DD
**Period:** [week/month/quarter/year]
**Project:** [project name]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PO.4.1, PO.4.2

## Executive Summary

### Overall Compliance Score: XX/100 [Rating]

| Practice | Score | Trend | Status |
|----------|-------|-------|--------|
| PO - Prepare Organization | XX% | ↑/→/↓ | [status] |
| PS - Protect Software | XX% | ↑/→/↓ | [status] |
| PW - Produce Well-Secured | XX% | ↑/→/↓ | [status] |
| RV - Respond to Vulnerabilities | XX% | ↑/→/↓ | [status] |

## Key Performance Indicators

### Vulnerability Management
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| Open Critical | X | 0 | PASS/FAIL |
| Open High | X | ≤5 | PASS/FAIL |
| MTTR (Critical) | X days | ≤7 days | PASS/FAIL |
| MTTR (High) | X days | ≤30 days | PASS/FAIL |
| Oldest Unresolved | X days | ≤90 days | PASS/FAIL |

### Process Compliance
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| Security Review Coverage | XX% | ≥90% | PASS/FAIL |
| SBOM Age | X days | ≤30 days | PASS/FAIL |
| Dependency Audit Age | X days | ≤7 days | PASS/FAIL |
| Threat Model Updated | X days | ≤90 days | PASS/FAIL |

### Supply Chain Security
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| Dependencies with CVEs | X | 0 critical | PASS/FAIL |
| Signed Commits | XX% | ≥95% | PASS/FAIL |
| Branch Protection | [status] | Full | PASS/FAIL |

## Practice Breakdown

### PO - Prepare the Organization (XX%)

| Task | Status | Last Evidence | Notes |
|------|--------|---------------|-------|
| PO.1 Security Requirements | | | |
| PO.2 Roles & Responsibilities | | | |
| PO.3 Toolchain Security | | | |
| PO.4 Metrics & Reporting | | | |
| PO.5 Environment Security | | | |

### PS - Protect the Software (XX%)

| Task | Status | Last Evidence | Notes |
|------|--------|---------------|-------|
| PS.1 Code Protection | | | |
| PS.2 Integrity Verification | | | |
| PS.3 Archive & Protect | | | |

### PW - Produce Well-Secured Software (XX%)

| Task | Status | Last Evidence | Notes |
|------|--------|---------------|-------|
| PW.1 Threat Modeling | | | |
| PW.2 Design Review | | | |
| PW.4 Dependency Security | | | |
| PW.5 Secure Coding | | | |
| PW.6 Build Security | | | |
| PW.7 Code Review | | | |
| PW.8 Security Testing | | | |
| PW.9 Configuration Security | | | |

### RV - Respond to Vulnerabilities (XX%)

| Task | Status | Last Evidence | Notes |
|------|--------|---------------|-------|
| RV.1 Identify Vulnerabilities | | | |
| RV.2 Assess Vulnerabilities | | | |
| RV.3 Remediate Vulnerabilities | | | |

## Trend Analysis

### Compliance Over Time
```
Month     | PO  | PS  | PW  | RV  | Overall
----------|-----|-----|-----|-----|--------
[M-3]     | XX% | XX% | XX% | XX% | XX%
[M-2]     | XX% | XX% | XX% | XX% | XX%
[M-1]     | XX% | XX% | XX% | XX% | XX%
Current   | XX% | XX% | XX% | XX% | XX%
```

### Vulnerability Trend
```
Month     | Critical | High | Medium | Low | Total
----------|----------|------|--------|-----|------
[M-3]     | X        | X    | X      | X   | X
[M-2]     | X        | X    | X      | X   | X
[M-1]     | X        | X    | X      | X   | X
Current   | X        | X    | X      | X   | X
```

## Risk Assessment

| Risk Area | Level | Trend | Action Required |
|-----------|-------|-------|-----------------|
| Vulnerability Backlog | H/M/L | ↑/→/↓ | [action] |
| Compliance Gaps | H/M/L | ↑/→/↓ | [action] |
| Supply Chain | H/M/L | ↑/→/↓ | [action] |
| Process Maturity | H/M/L | ↑/→/↓ | [action] |

## Recommendations

### Priority 1 (Immediate)
1. [Address critical gaps]

### Priority 2 (This Sprint)
1. [Improve coverage]

### Priority 3 (This Quarter)
1. [Strategic improvements]

## Evidence Summary

| Category | Files | Latest |
|----------|-------|--------|
| Security Reviews | X | YYYY-MM-DD |
| Threat Models | X | YYYY-MM-DD |
| Dependency Audits | X | YYYY-MM-DD |
| SBOM | X | YYYY-MM-DD |
| Build Hardening | X | YYYY-MM-DD |

## Sign-Off

- [ ] Metrics accurately reflect current state
- [ ] Trends identified and explained
- [ ] Recommendations prioritized
- [ ] Report archived for audit trail
```

## Integration

**Create issue for metric concerns:**
```bash
bd create "Security: [Metric Below Threshold]" --type task -d "Compliance score dropped. See metrics report: .claude/ssdf/evidence/metrics/[date].md"
```

## Commit Message Format

```
[SSDF:PO.4.2] Generate security metrics report

- Overall compliance: XX%
- Key findings: [summary]
- Evidence: .claude/ssdf/evidence/metrics/YYYY-MM-DD.md
```

## Automation

**Scheduled metrics generation:**
```yaml
# .github/workflows/ssdf-metrics.yml
name: SSDF Metrics
on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly
  workflow_dispatch:

jobs:
  metrics:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Generate Metrics
        run: claude /ssdf-metrics --period=week --format=detailed
```
