---
description: Generate SLSA supply chain provenance (PS.3, EO14028)
arguments:
  - name: action
    description: Action - generate, verify, or status (default: status)
    required: false
  - name: level
    description: SLSA level - 1, 2, 3, or 4 (default: 2)
    required: false
---

# SSDF SLSA Provenance

Use the `ssdf-slsa-provenance` skill to manage supply chain attestations.

**Action:** $ARGUMENTS.action (or status if not specified)
**Level:** $ARGUMENTS.level (or 2 if not specified)

## Process

1. Dispatch agents:
   - `devops-engineer`: Build pipeline analysis
   - `security-engineer`: Attestation verification

2. Assess current SLSA level compliance
3. Generate provenance attestations
4. Verify artifact provenance
5. Generate evidence file in `.claude/ssdf/evidence/provenance/`

## Output Requirements

- SLSA level compliance status
- Provenance attestations
- Verification results
- SSDF references (PS.3) in commit messages
