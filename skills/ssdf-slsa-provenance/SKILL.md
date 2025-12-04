---
name: ssdf-slsa-provenance
description: Use to manage SLSA supply chain provenance per SSDF PS.3 and EO14028. Generates and verifies attestations. Dispatches devops-engineer and security-engineer agents.
---

# SSDF SLSA Provenance (PS.3, EO14028)

## Overview

Implement SLSA (Supply-chain Levels for Software Artifacts) framework for supply chain security, including provenance generation and verification.

**SSDF References:**
- PS.3.1 (Protect All Forms of Code)
- PS.3.2 (Maintain Provenance of Components)

**EO14028 References:**
- 4e(vi) - Providing SBOM for each product

## When to Use

**Required for:**
- Release builds
- Artifact publishing
- Supply chain audits

**Recommended for:**
- CI/CD setup
- Security assessments
- Compliance verification

## SLSA Provenance Process

### Step 1: Dispatch Agents

**Agent 1: devops-engineer**
```
Analyze build pipeline for SLSA compliance:
[Repository path]

Tasks:
1. Assess current build process
2. Identify SLSA requirements gaps
3. Generate provenance metadata
4. Configure attestation signing
```

**Agent 2: security-engineer**
```
Verify provenance for:
[Artifact path]

Tasks:
1. Validate attestation signatures
2. Check builder identity
3. Verify source references
4. Assess SLSA level
```

### Step 2: SLSA Levels

| Level | Requirements |
|-------|--------------|
| 1 | Build process documented, provenance available |
| 2 | Hosted build service, authenticated provenance |
| 3 | Hardened builds, non-falsifiable provenance |
| 4 | Hermetic builds, reproducible, two-person review |

### Step 3: Provenance Generation

**SLSA Provenance v1.0 format:**
```json
{
  "_type": "https://in-toto.io/Statement/v1",
  "subject": [{
    "name": "artifact-name",
    "digest": {"sha256": "..."}
  }],
  "predicateType": "https://slsa.dev/provenance/v1",
  "predicate": {
    "buildDefinition": {
      "buildType": "...",
      "externalParameters": {"repository": "...", "ref": "..."},
      "resolvedDependencies": [...]
    },
    "runDetails": {
      "builder": {"id": "..."},
      "metadata": {"invocationId": "...", "startedOn": "..."}
    }
  }
}
```

### Step 4: Attestation Signing

**Sigstore (keyless):**
```bash
cosign sign-blob --yes artifact.tar.gz > artifact.sig
```

**GPG signing:**
```bash
gpg --armor --detach-sign provenance.json
```

### Step 5: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/provenance/YYYY-MM-DD-provenance.md
```

**Report template:**
```markdown
# SLSA Provenance Report

**Date:** YYYY-MM-DD
**Artifact:** [name]
**SLSA Level:** [1-4]
**Project:** [project name]
**SSDF Reference:** PS.3.1, PS.3.2

## SLSA Level Assessment

| Requirement | Status | Notes |
|-------------|--------|-------|
| Build documented | Pass/Fail | |
| Provenance generated | Pass/Fail | |
| Hosted build | Pass/Fail | |
| Authenticated | Pass/Fail | |
| Hardened | Pass/Fail | |
| Hermetic | Pass/Fail | |

## Current Level: X

### Gap to Level [X+1]
- [ ] [Missing requirement]

## Provenance Details

### Builder
- ID: [builder]
- Version: [version]

### Source
- Repository: [url]
- Commit: [sha]
- Ref: [branch/tag]

### Dependencies
| Dependency | Version | Digest |
|------------|---------|--------|

## Verification Status

| Check | Result |
|-------|--------|
| Signature valid | Pass/Fail |
| Builder trusted | Pass/Fail |
| Source matches | Pass/Fail |

## Recommendations

1. [Steps to improve SLSA level]

## Sign-Off

- [ ] Provenance generated
- [ ] Attestation signed
- [ ] Verification passed
```

## CI/CD Integration

**GitHub Actions (SLSA Level 3):**
```yaml
name: SLSA Build
on:
  push:
    tags: ['v*']

jobs:
  build:
    uses: slsa-framework/slsa-github-generator/.github/workflows/generator_generic_slsa3.yml@v1
    with:
      base64-subjects: ${{ needs.build.outputs.hash }}
```

## Commit Message Format

```
[SSDF:PS.3.2] Generate SLSA provenance

- SLSA Level: X
- Artifact: [name]
- Evidence: .claude/ssdf/evidence/provenance/YYYY-MM-DD.md
```
