---
description: Identify security training gaps from vulnerability patterns (PO.2 enhanced)
arguments:
  - name: team
    description: Team or role to analyze (default: all)
    required: false
---

# SSDF Training Gap Analysis

Use the `ssdf-training-gaps` skill to identify security training needs.

**Team:** $ARGUMENTS.team (or all if not specified)

## Process

1. Dispatch agents:
   - `data-analyst`: Pattern correlation
   - `security-auditor`: Training needs assessment

2. Analyze vulnerability patterns by contributor
3. Correlate with code review findings
4. Identify knowledge gaps
5. Generate evidence file in `.claude/ssdf/evidence/training/`

## Output Requirements

- Training gap analysis by topic
- Priority recommendations
- Suggested training resources
- SSDF references (PO.2.2) in commit messages
