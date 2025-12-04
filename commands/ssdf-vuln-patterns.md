---
description: Analyze vulnerability patterns and root causes (RV.3)
arguments:
  - name: period
    description: Analysis period - month, quarter, year, or all (default: quarter)
    required: false
  - name: focus
    description: Focus area - type, component, author, or all (default: all)
    required: false
---

# SSDF Vulnerability Pattern Analysis

Use the `ssdf-vuln-patterns` skill to analyze recurring vulnerabilities.

**Period:** $ARGUMENTS.period (or quarter if not specified)
**Focus:** $ARGUMENTS.focus (or all if not specified)

## Process

1. Dispatch agents:
   - `error-detective`: Pattern detection and correlation
   - `security-auditor`: Root cause analysis

2. Collect vulnerability data from all sources
3. Identify recurring patterns and clusters
4. Perform root cause analysis
5. Generate evidence file in `.claude/ssdf/evidence/vuln-patterns/`

## Output Requirements

- Pattern analysis with clustering
- Root cause identification
- Preventive recommendations
- SSDF references (RV.3.3, RV.3.4) in commit messages
