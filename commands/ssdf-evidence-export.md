---
description: Export SSDF compliance evidence for audit
arguments:
  - name: period
    description: Time period to export (e.g., "2024-Q4", "last-30-days", "all")
    required: false
  - name: format
    description: Export format (markdown, json)
    required: false
---

# SSDF Evidence Export

Generate structured compliance evidence for audit purposes.

**Period:** $ARGUMENTS.period (default: all)
**Format:** $ARGUMENTS.format (default: markdown)

## Process

1. Gather all evidence files from `.claude/ssdf/evidence/`:
   - threat-models/
   - security-reviews/
   - dependency-checks/
   - vulnerabilities/
   - sbom/

2. Compile into structured report organized by SSDF practice:
   - PO (Prepare Organization)
   - PS (Protect Software)
   - PW (Produce Well-Secured Software)
   - RV (Respond to Vulnerabilities)

3. Generate summary statistics:
   - Threat models completed
   - Security reviews performed
   - Vulnerabilities identified and remediated
   - Dependency audit coverage

## Output

Create consolidated export in `.claude/ssdf/exports/YYYY-MM-DD-export/`:
- `index.md` - Summary and navigation
- `practice-coverage.md` - SSDF practice compliance matrix
- `evidence-index.md` - Links to all evidence files
- `statistics.md` - Metrics and trends
