---
name: ssdf-sbom-generate
description: Use to generate Software Bill of Materials (SBOM) for supply chain transparency. Supports SPDX and CycloneDX formats per SSDF PS.3.2 and EO14028. Dispatches dependency-manager and security-auditor agents.
---

# SSDF SBOM Generation (PS.3.2)

## Overview

Generate comprehensive Software Bill of Materials documenting all dependencies, their versions, licenses, and provenance information.

**SSDF References:**
- PS.3.2 (Maintain Provenance of Software Components)

**EO14028 References:**
- 4e(vi) - Providing SBOM for each product
- 4e(vii) - SBOM minimum elements

## When to Use

**Required before:**
- Any release to production
- Software delivery to customers
- Compliance audits
- Security assessments

**Recommended for:**
- Regular dependency updates
- Pre-merge security checks
- Vulnerability triage

## SBOM Generation Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: dependency-manager**
```
Analyze dependencies for SBOM generation:
[Repository path]

Tasks:
1. Discover all package manager files
2. Parse direct dependencies
3. Resolve transitive dependency tree
4. Extract version constraints
5. Identify dependency conflicts
6. Calculate checksums for lock files
```

**Agent 2: security-auditor**
```
Perform security analysis for SBOM:
[Repository path]

Tasks:
1. Cross-reference dependencies with vulnerability databases
2. Check license compliance
3. Identify deprecated packages
4. Flag packages with known security issues
5. Verify package integrity (checksums match registry)
```

### Step 2: Dependency Discovery

**Supported package managers:**

| Ecosystem | Primary File | Lock File |
|-----------|-------------|-----------|
| JavaScript/Node | `package.json` | `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml` |
| Python | `requirements.txt`, `pyproject.toml` | `Pipfile.lock`, `poetry.lock` |
| Go | `go.mod` | `go.sum` |
| Rust | `Cargo.toml` | `Cargo.lock` |
| Java/Maven | `pom.xml` | - |
| Java/Gradle | `build.gradle` | `gradle.lockfile` |
| Ruby | `Gemfile` | `Gemfile.lock` |
| .NET | `*.csproj` | `packages.lock.json` |
| PHP | `composer.json` | `composer.lock` |

**Discovery commands:**
```bash
# Find all dependency files
find . -name "package.json" -o -name "requirements.txt" -o -name "go.mod" \
       -o -name "Cargo.toml" -o -name "pom.xml" -o -name "Gemfile" \
       -o -name "*.csproj" -o -name "composer.json" 2>/dev/null

# Check for lock files
ls -la package-lock.json yarn.lock pnpm-lock.yaml \
       Pipfile.lock poetry.lock go.sum Cargo.lock \
       Gemfile.lock composer.lock 2>/dev/null
```

### Step 3: Generate SPDX Format

**SPDX 2.3 (ISO/IEC 5962:2021):**

```json
{
  "spdxVersion": "SPDX-2.3",
  "dataLicense": "CC0-1.0",
  "SPDXID": "SPDXRef-DOCUMENT",
  "name": "[project-name]-sbom",
  "documentNamespace": "https://example.com/sbom/[project-name]/[uuid]",
  "creationInfo": {
    "created": "[ISO-8601-timestamp]",
    "creators": [
      "Tool: claude-code-ssdf-1.0",
      "Organization: [org-name]"
    ],
    "licenseListVersion": "3.19"
  },
  "packages": [
    {
      "SPDXID": "SPDXRef-Package-[name]-[version]",
      "name": "[package-name]",
      "versionInfo": "[version]",
      "downloadLocation": "[registry-url]",
      "filesAnalyzed": false,
      "licenseConcluded": "[SPDX-license-id]",
      "licenseDeclared": "[SPDX-license-id]",
      "copyrightText": "NOASSERTION",
      "externalRefs": [
        {
          "referenceCategory": "PACKAGE-MANAGER",
          "referenceType": "purl",
          "referenceLocator": "pkg:[ecosystem]/[name]@[version]"
        }
      ],
      "checksums": [
        {
          "algorithm": "SHA256",
          "checksumValue": "[hash]"
        }
      ],
      "supplier": "[supplier-info]"
    }
  ],
  "relationships": [
    {
      "spdxElementId": "SPDXRef-DOCUMENT",
      "relationshipType": "DESCRIBES",
      "relatedSpdxElement": "SPDXRef-Package-[root-package]"
    },
    {
      "spdxElementId": "SPDXRef-Package-[parent]",
      "relationshipType": "DEPENDS_ON",
      "relatedSpdxElement": "SPDXRef-Package-[child]"
    }
  ]
}
```

### Step 4: Generate CycloneDX Format

**CycloneDX 1.5:**

```json
{
  "bomFormat": "CycloneDX",
  "specVersion": "1.5",
  "serialNumber": "urn:uuid:[uuid]",
  "version": 1,
  "metadata": {
    "timestamp": "[ISO-8601-timestamp]",
    "tools": [
      {
        "vendor": "Claude Code",
        "name": "ssdf-sbom-generate",
        "version": "1.0"
      }
    ],
    "component": {
      "type": "application",
      "name": "[project-name]",
      "version": "[project-version]"
    }
  },
  "components": [
    {
      "type": "library",
      "bom-ref": "[name]@[version]",
      "name": "[package-name]",
      "version": "[version]",
      "purl": "pkg:[ecosystem]/[name]@[version]",
      "licenses": [
        {
          "license": {
            "id": "[SPDX-license-id]"
          }
        }
      ],
      "hashes": [
        {
          "alg": "SHA-256",
          "content": "[hash]"
        }
      ],
      "externalReferences": [
        {
          "type": "website",
          "url": "[homepage]"
        },
        {
          "type": "vcs",
          "url": "[repository]"
        }
      ]
    }
  ],
  "dependencies": [
    {
      "ref": "[parent]@[version]",
      "dependsOn": [
        "[child1]@[version]",
        "[child2]@[version]"
      ]
    }
  ],
  "vulnerabilities": []
}
```

### Step 5: Transitive Dependency Resolution

**Build complete dependency tree:**
```bash
# Node.js
npm ls --all --json

# Python
pip-compile --generate-hashes --output-file=-

# Go
go mod graph

# Rust
cargo tree --prefix=depth

# Java/Maven
mvn dependency:tree -DoutputType=json

# Ruby
bundle list --paths
```

**Dependency tree data to capture:**
- Direct vs transitive classification
- Dependency depth level
- Version constraints vs resolved versions
- Optional/dev dependencies flagged

### Step 6: SBOM Signing (Optional)

**GPG Signing:**
```bash
# Generate detached signature
gpg --armor --detach-sign --output sbom.json.asc sbom.json

# Verify signature
gpg --verify sbom.json.asc sbom.json
```

**Cosign (Sigstore) Signing:**
```bash
# Keyless signing (uses OIDC identity)
cosign sign-blob --yes sbom.json > sbom.json.sig

# Key-based signing
cosign sign-blob --key cosign.key sbom.json > sbom.json.sig

# Verify
cosign verify-blob --key cosign.pub --signature sbom.json.sig sbom.json
```

### Step 7: SBOM Diff (--diff flag)

Compare SBOMs between versions:

```bash
# Generate diff report
/ssdf-sbom-diff --base=v1.0.0 --target=HEAD
```

**Diff report includes:**
- Added dependencies
- Removed dependencies
- Version changes
- New vulnerabilities introduced
- License changes

### Step 8: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/sbom/YYYY-MM-DD-sbom.md
```

**Report template:**
```markdown
# SBOM Generation Report

**Date:** YYYY-MM-DD
**Project:** [project name]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PS.3.2
**EO14028 Reference:** 4e(vi), 4e(vii)

## Executive Summary

| Metric | Value |
|--------|-------|
| Total Components | X |
| Direct Dependencies | Y |
| Transitive Dependencies | Z |
| Formats Generated | SPDX, CycloneDX |
| Signed | Yes/No |

## Component Summary

### By Ecosystem
| Ecosystem | Count | Percentage |
|-----------|-------|------------|
| npm | X | Y% |
| PyPI | X | Y% |
| ... | | |

### By License
| License | Count | Compliance |
|---------|-------|------------|
| MIT | X | Compliant |
| Apache-2.0 | X | Compliant |
| GPL-3.0 | X | Review Required |
| Unknown | X | Action Required |

## Security Status

### Vulnerabilities Found
| Component | Version | CVE | Severity | Fixed In |
|-----------|---------|-----|----------|----------|

### Deprecated Packages
| Component | Version | Replacement |
|-----------|---------|-------------|

## SBOM Artifacts

| Format | File | Checksum (SHA-256) |
|--------|------|--------------------|
| SPDX 2.3 | sbom.spdx.json | [hash] |
| CycloneDX 1.5 | sbom.cdx.json | [hash] |
| Signature | sbom.spdx.json.sig | [hash] |

## Validation

- [ ] SPDX schema validation passed
- [ ] CycloneDX schema validation passed
- [ ] All components have purl identifiers
- [ ] All components have checksums
- [ ] No critical vulnerabilities
- [ ] All licenses identified

## Recommendations

1. [Prioritized recommendations]

## Sign-Off

- [ ] SBOM generated in required formats
- [ ] All dependencies accounted for
- [ ] Vulnerabilities documented
- [ ] License compliance verified
```

## Integration

**Create issue for SBOM problems:**
```bash
bd create "Security: [SBOM Issue]" --type security -d "Found during SSDF PS.3.2 SBOM generation. See evidence: .claude/ssdf/evidence/sbom/[date].md"
```

## Commit Message Format

```
[SSDF:PS.3.2] Generate SBOM for release vX.Y.Z

- Generated SPDX 2.3 and CycloneDX 1.5 formats
- X total components (Y direct, Z transitive)
- Evidence: .claude/ssdf/evidence/sbom/YYYY-MM-DD.md
```

## Validation Commands

**Validate SPDX:**
```bash
# Using pyspdxtools
pyspdxtools -i sbom.spdx.json

# Using spdx-tools
java -jar spdx-tools.jar Verify sbom.spdx.json
```

**Validate CycloneDX:**
```bash
# Using cyclonedx-cli
cyclonedx validate --input-file sbom.cdx.json --input-format json

# Using cdxgen
cdxgen validate sbom.cdx.json
```

## Common Issues and Fixes

**Issue: Missing checksums**
- Ensure lock files are present and up-to-date
- Run package manager install to populate cache
- Use `--generate-hashes` where supported

**Issue: Unknown licenses**
- Check package metadata manually
- Use license detection tools (licensee, scancode)
- Document as "NOASSERTION" with review note

**Issue: Missing transitive dependencies**
- Verify lock file is committed
- Run full dependency resolution
- Check for optional dependencies
