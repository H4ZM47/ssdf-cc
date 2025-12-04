---
name: ssdf-threat-model-evolution
description: Use to track threat model evolution over time per SSDF PW.1 (enhanced). Compares models, identifies new threats, tracks mitigation coverage. Dispatches security-auditor and research-analyst agents.
---

# SSDF Threat Model Evolution (PW.1 Enhanced)

## Overview

Track how threat models evolve over time, identifying new threats, changed attack vectors, and mitigation effectiveness.

**SSDF References:**
- PW.1.1 (Implement Threat Modeling)
- PW.1.2 (Track and Maintain Threat Models)

## When to Use

**Required for:**
- Major feature releases
- Architecture changes
- Annual threat reviews

**Recommended for:**
- Quarterly assessments
- After security incidents
- New threat disclosures

## Threat Model Evolution Process

### Step 1: Dispatch Agents

**Agent 1: security-auditor**
```
Analyze threat model evolution for:
[Repository path]

Tasks:
1. Load historical threat models
2. Compare current state
3. Identify new attack vectors
4. Assess mitigation coverage
5. Calculate risk changes
```

**Agent 2: research-analyst**
```
Research current threat landscape for:
[System type]

Tasks:
1. Survey recent CVEs
2. Identify emerging threats
3. Research attack trends
4. Map to system components
```

### Step 2: Version Comparison

**Compare threat models:**
```
.claude/ssdf/evidence/threat-models/
├── 2024-01-threat-model.md
├── 2024-04-threat-model.md
├── 2024-07-threat-model.md
└── current-threat-model.md
```

**Diff analysis:**
| Change Type | Count | Impact |
|-------------|-------|--------|
| New threats | X | High/Med/Low |
| Removed threats | X | - |
| Changed severity | X | High/Med/Low |
| New mitigations | X | - |

### Step 3: Generate Evolution Report

Create evidence file:
```
.claude/ssdf/evidence/threat-evolution/YYYY-MM-DD-evolution.md
```

**Report template:**
```markdown
# Threat Model Evolution Report

**Date:** YYYY-MM-DD
**Baseline:** [previous date]
**Project:** [project name]
**SSDF Reference:** PW.1.1, PW.1.2

## Summary

| Metric | Baseline | Current | Change |
|--------|----------|---------|--------|
| Total Threats | X | Y | +/-Z |
| Critical | X | Y | +/-Z |
| Mitigated | X% | Y% | +/-Z% |

## New Threats Identified

| Threat | Category | Severity | Mitigation Status |
|--------|----------|----------|-------------------|

## Changed Threats

| Threat | Previous | Current | Reason |
|--------|----------|---------|--------|

## Mitigation Coverage

| Category | Threats | Mitigated | Gap |
|----------|---------|-----------|-----|

## Trend Analysis

[Multi-period trend]

## Recommendations

1. [Address new threats]
2. [Update mitigations]
```

## Commit Message Format

```
[SSDF:PW.1.2] Update threat model

- Added X new threats
- Updated Y mitigations
- Evidence: .claude/ssdf/evidence/threat-evolution/YYYY-MM-DD.md
```
