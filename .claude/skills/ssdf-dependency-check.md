---
name: ssdf-dependency-check
description: Use when adding, updating, or auditing third-party components - verifies security, maintenance status, and generates SBOM per SSDF PW.4 and PS.3. Dispatches dependency-manager agent.
---

# SSDF Dependency Verification (PW.4, PS.3)

## Overview

Verify third-party components for security vulnerabilities, maintenance status, and license compliance. Generate and maintain Software Bill of Materials (SBOM).

**SSDF References:**
- PW.4.1 (Acquire Well-Secured Components)
- PW.4.4 (Verify Compliance Throughout Lifecycle)
- PS.3.2 (Provenance Data / SBOM)

## When to Use

**Required when:**
- Adding new dependencies
- Updating existing dependencies
- Periodic security audits (monthly recommended)
- Before releases
- After vulnerability disclosures

## Verification Process

### Step 1: Dispatch Dependency-Manager Agent

Use the Task tool to dispatch `dependency-manager` agent:

```
Analyze the following dependencies for security and compliance:
[List dependencies or package file path]

Check for:
1. Known vulnerabilities (CVEs) - critical/high priority
2. Maintenance status (last update, open issues)
3. License compatibility with [project license]
4. Transitive dependency risks
5. Available security patches
```

### Step 2: Vulnerability Assessment

For each dependency with findings:

```markdown
### Dependency: [name@version]
**Vulnerabilities Found:** [count]
**Severity:** [Critical/High/Medium/Low]
**CVEs:** [list]
**Fix Available:** [Yes/No - target version]
**Action Required:** [Update/Replace/Accept Risk]
**Justification:** [if accepting risk]
```

### Step 3: Maintenance Status Check

Verify each component is actively maintained:

| Criteria | Acceptable | Flag for Review |
|----------|------------|-----------------|
| Last commit | < 6 months | > 1 year |
| Open security issues | Addressed promptly | Ignored |
| Maintainer response | Active | Abandoned |
| Deprecation notice | None | Deprecated |

### Step 4: Generate SBOM

Create/update SBOM in `.claude/ssdf/evidence/sbom/`:

**Formats:**
- `sbom.json` (CycloneDX or SPDX)
- `sbom.md` (Human-readable summary)

**Required fields per component:**
- Name and version
- License
- Source/registry
- Integrity hash
- Known vulnerabilities (at time of check)
- Last verification date

### Step 5: Document Evidence

Create evidence file:
```
.claude/ssdf/evidence/dependency-checks/YYYY-MM-DD-[context].md
```

**Include:**
1. Dependencies checked
2. Vulnerabilities found and actions
3. Agent analysis results
4. SBOM update confirmation
5. Sign-off on accepted risks

## Dependency Addition Checklist

Before adding a new dependency:

- [ ] Vulnerability scan completed (no critical/high unpatched)
- [ ] Actively maintained (commit in last 6 months)
- [ ] License compatible
- [ ] Justified need (no existing solution)
- [ ] Minimal transitive dependencies
- [ ] SBOM updated
- [ ] Evidence documented

## Periodic Audit Checklist

Monthly or before releases:

- [ ] All dependencies scanned for new CVEs
- [ ] Outdated dependencies identified
- [ ] Update plan for vulnerable components
- [ ] SBOM refreshed
- [ ] Evidence report generated

## Automated Checks

Integrate into CI/CD:
```bash
# Example: npm audit
npm audit --audit-level=high

# Example: pip-audit
pip-audit --strict

# Example: go vuln check
govulncheck ./...
```

## Integration

**Create tracking issue for vulnerabilities:**
```bash
bd create "VULN: [CVE-ID] in [package]" --type security -d "SSDF PW.4.4 - vulnerability in third-party component"
```

**Reference in commits:**
```
[SSDF:PW.4.4] Update lodash to 4.17.21 (CVE-2021-23337)

Evidence: .claude/ssdf/evidence/dependency-checks/2024-12-03-lodash-update.md
SBOM updated: .claude/ssdf/evidence/sbom/sbom.json
```

## Risk Acceptance

If a vulnerability cannot be immediately fixed:

1. Document justification
2. Identify mitigating controls
3. Set review date
4. Create Beads issue for tracking
5. Get explicit approval (if team policy requires)

```markdown
### Risk Acceptance: [CVE-ID]
**Component:** [name@version]
**Severity:** [level]
**Justification:** [why not fixing now]
**Mitigating Controls:** [what reduces risk]
**Review Date:** [when to reassess]
**Approved By:** [name/role]
```
