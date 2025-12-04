---
description: Generate security metrics and compliance dashboard (PO.4)
arguments:
  - name: period
    description: Time period - week, month, quarter, or year (default: month)
    required: false
  - name: format
    description: Output format - summary, detailed, or json (default: summary)
    required: false
---

# SSDF Security Metrics

Use the `ssdf-metrics` skill to generate security metrics dashboard.

**Period:** $ARGUMENTS.period (or month if not specified)
**Format:** $ARGUMENTS.format (or summary if not specified)

## Process

1. Dispatch agents:
   - `data-analyst`: Metrics collection and trend analysis
   - `security-auditor`: Compliance scoring

2. Collect metrics from all SSDF evidence
3. Calculate compliance scores by practice
4. Identify trends and anomalies
5. Generate evidence file in `.claude/ssdf/evidence/metrics/`

## Output Requirements

- Overall compliance score
- Per-practice breakdown (PO, PS, PW, RV)
- Trend analysis over time
- SSDF references (PO.4.1, PO.4.2) in commit messages
