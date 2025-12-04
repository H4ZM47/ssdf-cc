# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Claude Code plugin implementing NIST SP 800-218 Secure Software Development Framework (SSDF) v1.1 compliance tooling. Provides 15 automation commands across 4 phases covering all SSDF practice groups (PO, PS, PW, RV) with EO14028 support.

## Architecture

```
.claude/
├── commands/                         # Slash command definitions
│   ├── ssdf-integrity-check.md       # PS.1, PS.2
│   ├── ssdf-sbom-generate.md         # PS.3.2, EO14028
│   ├── ssdf-secure-defaults.md       # PW.9
│   ├── ssdf-build-hardening.md       # PW.6
│   ├── ssdf-toolchain-audit.md       # PO.3
│   ├── ssdf-coding-standards.md      # PW.5
│   ├── ssdf-metrics.md               # PO.4
│   ├── ssdf-vuln-patterns.md         # RV.3
│   ├── ssdf-env-audit.md             # PO.5
│   ├── ssdf-architecture-review.md   # PW.2
│   ├── ssdf-continuous-monitoring.md # All
│   ├── ssdf-policy-as-code.md        # PO.1
│   ├── ssdf-threat-model-evolution.md # PW.1
│   ├── ssdf-training-gaps.md         # PO.2
│   ├── ssdf-slsa-provenance.md       # PS.3, EO14028
│   ├── ssdf-threat-model.md          # PW.1 (original)
│   ├── ssdf-security-review.md       # PW.7, PW.8
│   ├── ssdf-dependency-check.md      # PW.4, PS.3
│   ├── ssdf-vuln-assess.md           # RV.1-3
│   ├── ssdf-evidence-export.md       # All
│   └── ssdf-compliance-report.md     # All
│
├── skills/                           # Skill implementations
│   ├── ssdf-integrity-check.md
│   ├── ssdf-sbom-generate.md
│   ├── ssdf-secure-defaults.md
│   ├── ssdf-build-hardening.md
│   ├── ssdf-toolchain-audit.md
│   ├── ssdf-coding-standards.md
│   ├── ssdf-metrics.md
│   ├── ssdf-vuln-patterns.md
│   ├── ssdf-env-audit.md
│   ├── ssdf-architecture-review.md
│   ├── ssdf-continuous-monitoring.md
│   ├── ssdf-policy-as-code.md
│   ├── ssdf-threat-model-evolution.md
│   ├── ssdf-training-gaps.md
│   ├── ssdf-slsa-provenance.md
│   └── ssdf-*.md (original skills)
│
└── ssdf/                             # Project-local configuration
    ├── ssdf-config.yaml              # Project settings
    ├── templates/evidence-report.md  # Report template
    └── evidence/                     # Generated evidence artifacts
        ├── security-reviews/
        ├── threat-models/
        ├── dependencies/
        ├── integrity/
        ├── config/
        ├── build/
        ├── toolchain/
        ├── coding-standards/
        ├── sbom/
        ├── environment/
        ├── architecture/
        ├── monitoring/
        ├── policy/
        ├── threat-evolution/
        ├── training/
        ├── provenance/
        ├── metrics/
        └── vuln-patterns/
```

## Slash Commands by Phase

### Phase 1: Quick Wins
| Command | Purpose | SSDF Reference |
|---------|---------|----------------|
| `/ssdf-integrity-check` | Commit signing, branch protection | PS.1, PS.2 |
| `/ssdf-sbom-generate` | SBOM in SPDX/CycloneDX | PS.3.2, EO14028 |
| `/ssdf-secure-defaults` | Configuration security | PW.9 |
| `/ssdf-build-hardening` | Compiler flags, build security | PW.6 |

### Phase 2: Core Automation
| Command | Purpose | SSDF Reference |
|---------|---------|----------------|
| `/ssdf-toolchain-audit` | Development tool security | PO.3 |
| `/ssdf-coding-standards` | OWASP, CERT, CWE checks | PW.5 |
| `/ssdf-metrics` | Security metrics dashboard | PO.4 |
| `/ssdf-vuln-patterns` | Vulnerability pattern analysis | RV.3 |

### Phase 3: Advanced Features
| Command | Purpose | SSDF Reference |
|---------|---------|----------------|
| `/ssdf-env-audit` | Environment security | PO.5 |
| `/ssdf-architecture-review` | Architecture security review | PW.2 |
| `/ssdf-continuous-monitoring` | Git hooks, CI/CD integration | All |
| `/ssdf-policy-as-code` | Declarative security policies | PO.1 |

### Phase 4: AI-Powered
| Command | Purpose | SSDF Reference |
|---------|---------|----------------|
| `/ssdf-threat-model-evolution` | Threat model tracking | PW.1 |
| `/ssdf-training-gaps` | Training needs analysis | PO.2 |
| `/ssdf-slsa-provenance` | Supply chain attestations | PS.3, EO14028 |

### Original Commands
| Command | Purpose | SSDF Reference |
|---------|---------|----------------|
| `/ssdf-threat-model` | Threat modeling | PW.1 |
| `/ssdf-security-review` | Security code review | PW.7, PW.8 |
| `/ssdf-dependency-check` | Dependency verification | PW.4, PS.3 |
| `/ssdf-vuln-assess` | Vulnerability response | RV.1-3 |
| `/ssdf-evidence-export` | Export for audit | All |
| `/ssdf-compliance-report` | Compliance status | All |

## Sub-Agent Dispatch

| SSDF Activity | Primary Agent | Secondary Agent | Pattern |
|---------------|---------------|-----------------|---------|
| Integrity Check (PS.1/2) | security-auditor | git-workflow-manager | Parallel |
| SBOM Generation (PS.3) | dependency-manager | security-auditor | Parallel |
| Secure Defaults (PW.9) | security-auditor | framework-specific | Sequential |
| Build Hardening (PW.6) | build-engineer | security-engineer | Parallel |
| Toolchain Audit (PO.3) | devops-engineer | security-auditor | Parallel |
| Coding Standards (PW.5) | code-reviewer | security-auditor | Parallel |
| Metrics (PO.4) | data-analyst | security-auditor | Sequential |
| Vuln Patterns (RV.3) | error-detective | security-auditor | Parallel |
| Env Audit (PO.5) | platform-engineer | security-engineer | Parallel |
| Architecture Review (PW.2) | microservices-architect | security-auditor | Parallel |
| Continuous Monitoring | devops-engineer | security-engineer | Sequential |
| Policy as Code (PO.1) | compliance-auditor | security-auditor | Sequential |
| Threat Evolution (PW.1) | security-auditor | research-analyst | Parallel |
| Training Gaps (PO.2) | data-analyst | security-auditor | Sequential |
| SLSA Provenance (PS.3) | devops-engineer | security-engineer | Parallel |

## Configuration

Edit `.claude/ssdf/ssdf-config.yaml` to:
- Enable/disable specific SSDF practices
- Configure enforcement triggers
- Map agents to security tasks
- Set evidence output preferences
- Define compliance thresholds

## Evidence Format

All evidence files use consistent structure:
- SSDF practice references
- Agent analysis results
- Findings tables with severity, location, remediation
- Sign-off checklists
- Timestamps

Output to `.claude/ssdf/evidence/[type]/YYYY-MM-DD-[context].md`

## Commit Message Format

```
[SSDF:PW.x.x] Brief description

- Detail 1
- Evidence: .claude/ssdf/evidence/[type]/YYYY-MM-DD-[context].md
```

## Issue Tracking

Uses **Beads** for issue tracking (not GitHub Issues):

```bash
bd list                    # View all issues
bd create "Title"          # Create new issue
bd show <issue-id>         # View issue details
bd update <id> --status in_progress  # Update status
bd sync                    # Sync with git remote
```

Security issues use type `security`.

## Standards References

- NIST SP 800-218 SSDF v1.1
- Executive Order 14028
- SLSA Framework (Supply-chain Levels for Software Artifacts)
- OWASP Top 10 / OWASP Secure Coding Practices
- CERT Secure Coding Standards
- CWE Top 25
