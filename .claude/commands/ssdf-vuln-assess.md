---
description: Run SSDF vulnerability assessment and response (RV.1-3)
arguments:
  - name: vulnerability
    description: Description of the vulnerability to assess
    required: true
---

# SSDF Vulnerability Assessment: $ARGUMENTS.vulnerability

Use the `ssdf-vulnerability-response` skill to assess and respond to this vulnerability.

## Process

### Phase 1: Identification (RV.1)
1. Create tracking issue: `bd create "VULN: $ARGUMENTS.vulnerability" --type security`
2. Dispatch `error-detective` agent for scope analysis
3. Document initial assessment

### Phase 2: Assessment (RV.2)
1. Complete risk scoring (Exploitability, Impact, Affected Users, Data Sensitivity)
2. Assign severity classification (Critical/High/Medium/Low)

### Phase 3: Remediation (RV.2.2)
1. Dispatch `debugger` agent for root cause and fix approach
2. Implement and verify fix
3. Use `ssdf-security-review` on the fix

### Phase 4: Root Cause Analysis (RV.3)
1. Determine why vulnerability existed
2. Search for similar patterns with `code-reviewer` agent
3. Document lessons learned and process improvements

## Output Requirements

- Evidence file in `.claude/ssdf/evidence/vulnerabilities/`
- Complete timeline (discovery → fix → deploy)
- Root cause analysis and process improvements
- Beads issue lifecycle tracking
