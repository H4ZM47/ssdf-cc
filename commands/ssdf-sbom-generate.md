---
description: Generate Software Bill of Materials (SBOM) per PS.3.2 and EO14028
arguments:
  - name: format
    description: Output format - spdx, cyclonedx, or both (default: both)
    required: false
  - name: sign
    description: Sign the SBOM with GPG or cosign (default: false)
    required: false
---

# SSDF SBOM Generation

Use the `ssdf-sbom-generate` skill to create Software Bill of Materials.

**Format:** $ARGUMENTS.format (or both if not specified)
**Sign:** $ARGUMENTS.sign (or false if not specified)

## Process

1. Dispatch agents:
   - `dependency-manager`: Dependency discovery and analysis
   - `security-auditor`: Vulnerability correlation and license compliance

2. Discover dependencies from all supported package managers
3. Resolve transitive dependency tree
4. Generate SBOM in specified format(s)
5. Optionally sign SBOM
6. Generate evidence file in `.claude/ssdf/evidence/sbom/`

## Output Requirements

- Valid SPDX 2.3 and/or CycloneDX 1.5 format
- SHA-256 checksums for all components
- Package URLs (purl) for each dependency
- License information where available
- SSDF references (PS.3.2) in commit messages
