# ssdf-cc

Claude Code plugin for NIST SP 800-218 Secure Software Development Framework (SSDF) v1.1 compliance.

## Features

- **Comprehensive SSDF Coverage**: 15 automation commands covering all 4 SSDF practice groups
- **Dual-mode Operation**: Proactive security enforcement during development + on-demand guidance via slash commands
- **Sub-agent Integration**: Leverages specialized agents (security-auditor, code-reviewer, dependency-manager, etc.) for security analysis
- **Evidence Generation**: Creates audit-ready documentation for compliance verification
- **EO14028 Compliance**: Supports Executive Order 14028 requirements including SBOM and supply chain security
- **Beads Integration**: Tracks security issues through the issue lifecycle

## Installation

1. Copy skills to your Claude Code config:
   ```bash
   cp -r .claude/skills/ssdf-*.md ~/.claude/skills/
   ```

2. Copy slash commands:
   ```bash
   cp -r .claude/commands/ssdf-*.md ~/.claude/commands/
   ```

3. Copy project configuration to your target project:
   ```bash
   cp -r .claude/ssdf /path/to/your/project/.claude/
   ```

4. Edit `.claude/ssdf/ssdf-config.yaml` to customize settings for your project.

## Slash Commands

### Phase 1: Quick Wins (Low Effort, High Impact)

| Command | Description | SSDF Practice |
|---------|-------------|---------------|
| `/ssdf-integrity-check [scope]` | Verify commit signing, branch protection, release integrity | PS.1, PS.2 |
| `/ssdf-sbom-generate [format]` | Generate SBOM in SPDX/CycloneDX formats | PS.3.2, EO14028 |
| `/ssdf-secure-defaults [framework]` | Validate configuration against secure baselines | PW.9 |
| `/ssdf-build-hardening [language]` | Analyze compiler flags and build security | PW.6 |

### Phase 2: Core Automation (Medium Effort)

| Command | Description | SSDF Practice |
|---------|-------------|---------------|
| `/ssdf-toolchain-audit [scope]` | Audit compilers, package managers, IDE plugins | PO.3 |
| `/ssdf-coding-standards [scope]` | Validate against OWASP, CERT, CWE guidelines | PW.5 |
| `/ssdf-metrics [period]` | Generate security metrics and compliance dashboard | PO.4 |
| `/ssdf-vuln-patterns [period]` | Analyze vulnerability patterns and root causes | RV.3 |

### Phase 3: Advanced Features (High Effort)

| Command | Description | SSDF Practice |
|---------|-------------|---------------|
| `/ssdf-env-audit [environment]` | Audit environment security and access controls | PO.5 |
| `/ssdf-architecture-review [scope]` | Review architecture for trust boundaries, attack surface | PW.2 |
| `/ssdf-continuous-monitoring [action]` | Configure git hooks, CI/CD integration | All |
| `/ssdf-policy-as-code [action]` | Define and validate security policies declaratively | PO.1 |

### Phase 4: AI-Powered Features

| Command | Description | SSDF Practice |
|---------|-------------|---------------|
| `/ssdf-threat-model-evolution [action]` | Track threat model changes over time | PW.1 |
| `/ssdf-training-gaps [team]` | Identify security training needs from patterns | PO.2 |
| `/ssdf-slsa-provenance [action]` | Generate SLSA supply chain attestations | PS.3, EO14028 |

### Original Commands

| Command | Description | SSDF Practice |
|---------|-------------|---------------|
| `/ssdf-threat-model [feature]` | Generate threat model for a feature | PW.1 |
| `/ssdf-security-review [scope]` | Security-focused code review | PW.7, PW.8 |
| `/ssdf-dependency-check [pkg]` | Check dependencies for vulnerabilities | PW.4, PS.3 |
| `/ssdf-vuln-assess [description]` | Assess and respond to a vulnerability | RV.1-3 |
| `/ssdf-evidence-export [format]` | Export compliance evidence | All |
| `/ssdf-compliance-report [scope]` | Generate compliance status report | All |

## SSDF Practice Coverage

### PO - Prepare the Organization
| Practice | Commands |
|----------|----------|
| PO.1 | policy-as-code |
| PO.2 | training-gaps |
| PO.3 | toolchain-audit |
| PO.4 | metrics |
| PO.5 | env-audit |

### PS - Protect the Software
| Practice | Commands |
|----------|----------|
| PS.1 | integrity-check |
| PS.2 | integrity-check |
| PS.3 | sbom-generate, slsa-provenance, dependency-check |

### PW - Produce Well-Secured Software
| Practice | Commands |
|----------|----------|
| PW.1 | threat-model, threat-model-evolution |
| PW.2 | architecture-review |
| PW.4 | dependency-check |
| PW.5 | coding-standards |
| PW.6 | build-hardening |
| PW.7/8 | security-review |
| PW.9 | secure-defaults |

### RV - Respond to Vulnerabilities
| Practice | Commands |
|----------|----------|
| RV.1-3 | vuln-assess, vuln-patterns |

## Evidence Output

All security activities generate evidence files in `.claude/ssdf/evidence/` organized by type:

```
.claude/ssdf/evidence/
├── security-reviews/      # PW.7, PW.8
├── threat-models/         # PW.1
├── dependencies/          # PW.4, PS.3
├── integrity/             # PS.1, PS.2
├── config/                # PW.9
├── build/                 # PW.6
├── toolchain/             # PO.3
├── coding-standards/      # PW.5
├── sbom/                  # PS.3.2
├── environment/           # PO.5
├── architecture/          # PW.2
├── monitoring/            # All
├── policy/                # PO.1
├── threat-evolution/      # PW.1
├── training/              # PO.2
├── provenance/            # PS.3
├── metrics/               # PO.4
└── vuln-patterns/         # RV.3
```

Each evidence file includes:
- Timestamps and SSDF practice references
- Agent analysis results
- Findings with severity, location, and remediation
- Sign-off checklists

## Sub-Agent Dispatch

| SSDF Activity | Primary Agent | Secondary Agent |
|---------------|---------------|-----------------|
| Integrity Check | security-auditor | git-workflow-manager |
| SBOM Generation | dependency-manager | security-auditor |
| Secure Defaults | security-auditor | framework-specific |
| Build Hardening | build-engineer | security-engineer |
| Toolchain Audit | devops-engineer | security-auditor |
| Coding Standards | code-reviewer | security-auditor |
| Metrics | data-analyst | security-auditor |
| Vuln Patterns | error-detective | security-auditor |
| Env Audit | platform-engineer | security-engineer |
| Architecture Review | microservices-architect | security-auditor |
| Continuous Monitoring | devops-engineer | security-engineer |
| Policy as Code | compliance-auditor | security-auditor |
| Threat Evolution | security-auditor | research-analyst |
| Training Gaps | data-analyst | security-auditor |
| SLSA Provenance | devops-engineer | security-engineer |

## Configuration

The `.claude/ssdf/ssdf-config.yaml` file controls:

- Which SSDF practices are enabled
- Proactive enforcement triggers
- Sub-agent mappings for security tasks
- Evidence output settings
- Beads integration preferences
- Compliance thresholds

## Commit Message Format

```
[SSDF:PW.x.x] Brief description

- Detail 1
- Evidence: .claude/ssdf/evidence/[type]/YYYY-MM-DD-[context].md
```

## Issue Tracking

Uses **Beads** for issue tracking:

```bash
bd list                    # View all issues
bd create "Title"          # Create new issue
bd show <issue-id>         # View issue details
bd update <id> --status in_progress  # Update status
bd sync                    # Sync with git remote
```

Security issues use type `security`.

## References

- [NIST SP 800-218 SSDF v1.1](https://csrc.nist.gov/publications/detail/sp/800-218/final)
- [Executive Order 14028](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/)
- [SLSA Framework](https://slsa.dev/)
- [OWASP](https://owasp.org/)

## License

MIT
