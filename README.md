# ssdf-cc

Claude Code plugin for NIST SP 800-218 Secure Software Development Framework (SSDF) v1.1 compliance.

## Features

- **Dual-mode operation**: Proactive security enforcement during development + on-demand guidance via slash commands
- **Sub-agent integration**: Leverages specialized agents (security-auditor, code-reviewer, dependency-manager, etc.) for security analysis
- **Evidence generation**: Creates audit-ready documentation for compliance verification
- **Beads integration**: Tracks security issues through the issue lifecycle

## Installation

1. Copy skills to your Claude Code config:
   ```bash
   cp -r skills/ssdf ~/.claude/skills/
   ```

2. Copy slash commands:
   ```bash
   cp -r commands/* ~/.claude/commands/
   ```

3. Copy project configuration to your target project:
   ```bash
   cp -r .claude/ssdf /path/to/your/project/.claude/
   ```

4. Edit `.claude/ssdf/ssdf-config.yaml` to customize settings for your project.

## Slash Commands

| Command | Description | SSDF Practice |
|---------|-------------|---------------|
| `/ssdf-threat-model [feature]` | Generate threat model for a feature or component | PW.1 |
| `/ssdf-security-review [scope]` | Security-focused code review | PW.7, PW.8 |
| `/ssdf-dependency-check [pkg]` | Check dependencies for vulnerabilities | PW.4, PS.3 |
| `/ssdf-vuln-assess [description]` | Assess and respond to a vulnerability | RV.1-3 |
| `/ssdf-evidence-export [format]` | Export compliance evidence (markdown, json) | All |
| `/ssdf-compliance-report [scope]` | Generate compliance status report | All |

## SSDF Practice Coverage

### PO - Prepare the Organization
- Security requirements defined in configuration
- Agent roles mapped to SSDF tasks
- Evidence output configured

### PS - Protect the Software
- **PS.3**: SBOM generation via dependency-check

### PW - Produce Well-Secured Software
- **PW.1**: Threat modeling for new features and APIs
- **PW.4**: Third-party component verification
- **PW.5**: Secure coding enforcement
- **PW.7**: Security-focused code review
- **PW.8**: Security testing guidance

### RV - Respond to Vulnerabilities
- **RV.1**: Vulnerability identification
- **RV.2**: Assessment and remediation
- **RV.3**: Root cause analysis

## Configuration

The `.claude/ssdf/ssdf-config.yaml` file controls:

- Which SSDF practices are enabled
- Proactive enforcement triggers
- Sub-agent mappings for security tasks
- Evidence output settings
- Beads integration preferences

## Evidence Output

All security activities generate evidence files in `.claude/ssdf/evidence/` with:
- Timestamps and SSDF practice references
- Agent analysis results
- Findings with severity, location, and remediation
- Sign-off checklists

## License

MIT
