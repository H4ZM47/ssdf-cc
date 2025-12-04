---
description: Run SSDF threat modeling (PW.1) for a feature or component
arguments:
  - name: feature
    description: The feature or component to threat model
    required: true
---

# SSDF Threat Modeling: $ARGUMENTS.feature

Use the `ssdf-threat-modeling` skill to perform threat modeling on: **$ARGUMENTS.feature**

## Process

1. Define scope and trust boundaries for this feature
2. Dispatch `security-auditor` agent for attack surface analysis
3. Complete STRIDE analysis
4. Document mitigations for identified threats
5. Generate evidence file in `.claude/ssdf/evidence/threat-models/`

## Output Requirements

- Create threat model evidence file
- Track with Beads: `bd create "Threat Model: $ARGUMENTS.feature" --type security`
- Include SSDF reference (PW.1.1) in any resulting commits
