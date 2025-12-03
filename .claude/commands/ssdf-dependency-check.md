---
description: Run SSDF dependency verification (PW.4/PS.3) and update SBOM
arguments:
  - name: package
    description: Specific package to check (default: all dependencies)
    required: false
---

# SSDF Dependency Verification

Use the `ssdf-dependency-check` skill to verify third-party components.

**Target:** $ARGUMENTS.package (or all dependencies if not specified)

## Process

1. Dispatch `dependency-manager` agent to:
   - Check for known CVEs (critical/high priority)
   - Verify maintenance status
   - Review license compatibility
   - Analyze transitive dependencies

2. Generate vulnerability assessment for each finding
3. Update SBOM in `.claude/ssdf/evidence/sbom/`
4. Document evidence in `.claude/ssdf/evidence/dependency-checks/`

## Output Requirements

- Vulnerability assessment per affected dependency
- Updated SBOM (sbom.json and sbom.md)
- Risk acceptance documentation for any deferred fixes
- Beads issue for critical/high vulnerabilities: `bd create "VULN: [CVE-ID] in [package]" --type security`
