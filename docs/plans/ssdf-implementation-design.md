# SSDF Implementation Design Document

**Framework:** NIST SP 800-218 Secure Software Development Framework (SSDF) v1.1
**Date:** 2024-12-03
**Status:** Implemented

## Overview

This document describes the Claude Code integration with NIST SP 800-218 SSDF practices, enabling proactive security enforcement and on-demand compliance guidance throughout the software development lifecycle.

## Design Goals

1. **Dual-mode operation:** Proactive enforcement during development + on-demand guidance via slash commands
2. **Sub-agent integration:** Leverage specialized agents for security analysis, dependency checking, and code review
3. **Evidence generation:** Create audit-ready documentation for compliance verification
4. **Beads integration:** Track security issues and SSDF activities through issue lifecycle

## Architecture

### Components

```
~/.claude/
├── skills/ssdf/
│   ├── ssdf-secure-development.md    # Main orchestration (proactive)
│   ├── ssdf-threat-modeling.md       # PW.1 - Threat models
│   ├── ssdf-security-review.md       # PW.7/PW.8 - Code review
│   ├── ssdf-dependency-check.md      # PW.4/PS.3 - Dependencies
│   └── ssdf-vulnerability-response.md # RV.1-3 - Vulnerability handling
│
└── commands/
    ├── ssdf-threat-model.md          # On-demand threat modeling
    ├── ssdf-security-review.md       # On-demand security review
    ├── ssdf-dependency-check.md      # On-demand dependency check
    ├── ssdf-vuln-assess.md           # Vulnerability assessment
    ├── ssdf-evidence-export.md       # Export compliance evidence
    └── ssdf-compliance-report.md     # Generate compliance report

.claude/ssdf/                         # Project-local configuration
├── ssdf-config.yaml                  # Project settings
├── templates/
│   └── evidence-report.md            # Report template
└── evidence/                         # Generated evidence
    ├── threat-models/
    ├── security-reviews/
    ├── dependency-checks/
    ├── vulnerabilities/
    └── sbom/
```

### Sub-Agent Dispatch Patterns

| SSDF Activity | Primary Agent | Secondary Agent | Pattern |
|---------------|---------------|-----------------|---------|
| Threat Modeling (PW.1) | security-auditor | - | Sequential |
| Security Review (PW.7/8) | security-auditor | code-reviewer | Parallel |
| Dependency Check (PW.4) | dependency-manager | - | Sequential |
| Vulnerability ID (RV.1) | error-detective | - | Sequential |
| Root Cause (RV.3) | debugger | code-reviewer | Sequential |

## SSDF Practice Coverage

### PO - Prepare Organization
- Security requirements defined in ssdf-config.yaml
- Agent roles mapped to SSDF tasks
- Evidence output configured

### PS - Protect Software
- PS.3.2: SBOM generation via dependency-check skill
- Integrity verification through dependency-manager agent

### PW - Produce Well-Secured Software
| Task | Skill | Trigger |
|------|-------|---------|
| PW.1 Threat Modeling | ssdf-threat-modeling | New features, APIs, security-sensitive components |
| PW.4 Third-Party Components | ssdf-dependency-check | Adding/updating dependencies |
| PW.5 Secure Coding | ssdf-secure-development | All code changes (proactive) |
| PW.7 Code Review | ssdf-security-review | Pre-commit, PR creation |
| PW.8 Security Testing | ssdf-security-review | Release preparation |

### RV - Respond to Vulnerabilities
| Task | Skill | Trigger |
|------|-------|---------|
| RV.1 Identification | ssdf-vulnerability-response | Vulnerability discovered |
| RV.2 Assessment | ssdf-vulnerability-response | After identification |
| RV.3 Root Cause | ssdf-vulnerability-response | After remediation |

## Workflows

### Proactive Enforcement (ssdf-secure-development)

```
Developer writes code
        │
        ▼
┌───────────────────┐
│ Detect context:   │
│ - New feature?    │──▶ Trigger threat modeling
│ - Security code?  │──▶ Dispatch code-reviewer
│ - Dependencies?   │──▶ Run dependency check
│ - Pre-commit?     │──▶ Security review
└───────────────────┘
        │
        ▼
Generate evidence with SSDF references
        │
        ▼
Track in Beads if security issue found
```

### On-Demand Security Review

```
/ssdf-security-review [scope]
        │
        ▼
┌─────────────────────────────┐
│ Parallel agent dispatch:    │
│ - security-auditor          │
│ - code-reviewer             │
└─────────────────────────────┘
        │
        ▼
OWASP Top 10 checklist
        │
        ▼
Secure coding verification
        │
        ▼
Evidence file + findings table
```

### Vulnerability Response

```
/ssdf-vuln-assess [description]
        │
        ▼
Phase 1: Identification
├── Create Beads issue
├── Dispatch error-detective
└── Document initial assessment
        │
        ▼
Phase 2: Assessment
├── Risk scoring
└── Severity classification
        │
        ▼
Phase 3: Remediation
├── Dispatch debugger for root cause
├── Implement fix
└── Security review of fix
        │
        ▼
Phase 4: Root Cause Analysis
├── Pattern search with code-reviewer
├── Document lessons learned
└── Update processes
```

## Evidence Format

All evidence files follow consistent structure:

```markdown
# [Activity Type]: [Context]

**Date:** YYYY-MM-DD
**SSDF Reference:** [Practice IDs]
**Reviewer:** Claude Code (SSDF)

## Summary
[Brief description]

## Agent Analysis
[Results from dispatched agents]

## Findings
| Severity | Issue | Location | Remediation |
|----------|-------|----------|-------------|
| H/M/L    | desc  | file:line| fix         |

## Sign-Off
- [ ] Required actions completed
- [ ] Evidence documented
```

## Commit Message Format

```
[SSDF:PW.x.x] Brief description

- Detail 1
- Detail 2
- Evidence: .claude/ssdf/evidence/[type]/YYYY-MM-DD-[context].md
```

## Beads Integration

```bash
# Create security tracking issue
bd create "VULN: [description]" --type security -d "SSDF [reference]"

# Update status
bd update [id] --status in_progress

# Close with resolution
bd close [id] --reason "Fixed in vX.Y.Z. Evidence: [path]"
```

## Configuration

Project-level settings in `.claude/ssdf/ssdf-config.yaml`:

- Enable/disable specific SSDF practices
- Configure enforcement triggers
- Map agents to tasks
- Set evidence output preferences
- Enable Beads integration

## Slash Commands Reference

| Command | Purpose | SSDF Reference |
|---------|---------|----------------|
| `/ssdf-threat-model [feature]` | Threat modeling | PW.1 |
| `/ssdf-security-review [scope]` | Security code review | PW.7, PW.8 |
| `/ssdf-dependency-check [pkg]` | Dependency verification | PW.4, PS.3 |
| `/ssdf-vuln-assess [vuln]` | Vulnerability response | RV.1-3 |
| `/ssdf-evidence-export [period]` | Export for audit | All |
| `/ssdf-compliance-report [scope]` | Compliance status | All |

## Future Enhancements

1. **Automated CI/CD integration** - Pre-commit hooks and pipeline stages
2. **Metrics dashboard** - Track SSDF compliance over time
3. **Team training mode** - Educational prompts explaining SSDF requirements
4. **Custom rule sets** - Organization-specific security requirements
5. **Integration with security scanners** - Import results from external tools
