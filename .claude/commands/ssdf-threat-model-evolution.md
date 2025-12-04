---
description: Track threat model changes over time (PW.1 enhanced)
arguments:
  - name: action
    description: Action - compare, history, or update (default: compare)
    required: false
---

# SSDF Threat Model Evolution

Use the `ssdf-threat-model-evolution` skill to track threat model changes.

**Action:** $ARGUMENTS.action (or compare if not specified)

## Process

1. Dispatch agents:
   - `security-auditor`: Threat model analysis
   - `research-analyst`: Threat landscape research

2. Compare current vs previous threat models
3. Identify new/changed threats
4. Assess mitigation coverage
5. Generate evidence file in `.claude/ssdf/evidence/threat-evolution/`

## Output Requirements

- Threat model diff analysis
- New threat identification
- Mitigation gap analysis
- SSDF references (PW.1) in commit messages
