# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Claude Code plugin implementing NIST SP 800-218 Secure Software Development Framework (SSDF) v1.1 compliance tooling. Provides dual-mode operation: proactive security enforcement during development and on-demand guidance via slash commands.

## Architecture

```
~/.claude/
├── skills/ssdf/                      # Proactive enforcement skills
│   ├── ssdf-secure-development.md    # Main orchestration
│   ├── ssdf-threat-modeling.md       # PW.1 - Threat models
│   ├── ssdf-security-review.md       # PW.7/PW.8 - Code review
│   ├── ssdf-dependency-check.md      # PW.4/PS.3 - Dependencies
│   └── ssdf-vulnerability-response.md # RV.1-3 - Vulnerability handling
│
└── commands/                         # On-demand slash commands
    ├── ssdf-threat-model.md
    ├── ssdf-security-review.md
    ├── ssdf-dependency-check.md
    ├── ssdf-vuln-assess.md
    ├── ssdf-evidence-export.md
    └── ssdf-compliance-report.md

.claude/ssdf/                         # Project-local configuration
├── ssdf-config.yaml                  # Project settings
├── templates/evidence-report.md      # Report template
└── evidence/                         # Generated evidence artifacts
```

## Slash Commands

| Command | Purpose | SSDF Reference |
|---------|---------|----------------|
| `/ssdf-threat-model [feature]` | Threat modeling | PW.1 |
| `/ssdf-security-review [scope]` | Security code review | PW.7, PW.8 |
| `/ssdf-dependency-check [pkg]` | Dependency verification | PW.4, PS.3 |
| `/ssdf-vuln-assess [vuln]` | Vulnerability response | RV.1-3 |
| `/ssdf-evidence-export [format]` | Export for audit | All |
| `/ssdf-compliance-report [scope]` | Compliance status | All |

## Sub-Agent Dispatch

| SSDF Activity | Primary Agent | Secondary Agent | Pattern |
|---------------|---------------|-----------------|---------|
| Threat Modeling (PW.1) | security-auditor | - | Sequential |
| Security Review (PW.7/8) | security-auditor | code-reviewer | Parallel |
| Dependency Check (PW.4) | dependency-manager | - | Sequential |
| Vulnerability ID (RV.1) | error-detective | - | Sequential |
| Root Cause (RV.3) | debugger | code-reviewer | Sequential |

## Configuration

Edit `.claude/ssdf/ssdf-config.yaml` to:
- Enable/disable specific SSDF practices
- Configure enforcement triggers
- Map agents to security tasks
- Set evidence output preferences

## Evidence Format

All evidence files use consistent structure with SSDF references, agent analysis results, findings tables, and sign-off checklists. Output to `.claude/ssdf/evidence/`.

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

Security issues use type `security` with `VULN` prefix.
