# SSDF for Claude Code

A Claude Code plugin for secure software development and compliance with NIST SP 800-218.

## Why This Exists

If you've ever had to fill out a security questionnaire, prepare for an audit, or prove to a customer that you actually follow secure development practices, you know how painful it is. You're either scrambling to document things after the fact, or you're maintaining a pile of security docs that nobody reads.

This plugin runs security checks as you work and generates the evidence automatically. When audit time comes, you've already got the paperwork.

## Who Should Use This

- Teams selling to enterprises or government who need to demonstrate security practices
- Anyone dealing with NIST SP 800-218 (SSDF) or Executive Order 14028 requirements
- Security folks who want to scale reviews across multiple projects
- Developers who'd rather have a tool catch issues than find out in a pen test

## What's NIST SSDF?

The Secure Software Development Framework is a NIST publication (SP 800-218) that lists practices for reducing software vulnerabilities. It covers four areas:

| Area | What It Means |
|------|---------------|
| Prepare the Organization | Security policies, training, secure tooling |
| Protect the Software | Signed commits, SBOMs, verified dependencies |
| Produce Well-Secured Software | Threat models, code review, secure configs |
| Respond to Vulnerabilities | Triage, root cause analysis, pattern tracking |

This plugin has commands for each area.

## Installation

### Prerequisites

Before installing the plugin, ensure you have:

1. **Claude Code** - The official CLI for Claude. See [claude.ai/code](https://claude.ai/code) for installation.

### Option 1: From GitHub (Recommended)

Within Claude Code, execute these plugin commands:

```bash
/plugin marketplace add H4ZM47/ssdf-cc
/plugin install ssdf-cc
```

### Option 2: Local Development

For developers working on the plugin locally:

```bash
git clone https://github.com/H4ZM47/ssdf-cc
cd ssdf-cc
/plugin marketplace add .
/plugin install ssdf-cc
```

### Final Step

After installation completes, restart Claude Code to activate all SSDF commands.

### Optional: Beads Integration

This plugin works well with [Beads](https://github.com/steveyegge/beads), an AI-supervised issue tracker. When Beads is installed, security findings can be tracked as issues through their full lifecycle—from discovery to remediation to verification.

Without Beads, the plugin still works fine. Findings go to evidence files instead of being tracked as issues.

To install Beads:

```bash
/plugin marketplace add steveyegge/beads
/plugin install beads
```

### Optional: Project Configuration

If you want to customize which practices are enabled or set severity thresholds, copy the config template to your project:

```bash
# After cloning the plugin repo
cp -r ssdf-config-template /path/to/your/project/.claude/ssdf
```

Then edit `.claude/ssdf/ssdf-config.yaml`.

## Commands

Each command runs a security check and saves evidence to `.claude/ssdf/evidence/`.

### Reviews

| Command | Description |
|---------|-------------|
| `/ssdf-security-review [files]` | Security-focused code review. Run before merging PRs. |
| `/ssdf-threat-model [feature]` | STRIDE threat model for a feature. Good for design phase. |
| `/ssdf-architecture-review [scope]` | Looks at trust boundaries and attack surface. |

### Dependencies & Supply Chain

| Command | Description |
|---------|-------------|
| `/ssdf-dependency-check [package]` | Checks for vulnerable or outdated dependencies. |
| `/ssdf-sbom-generate [format]` | Generates SPDX or CycloneDX SBOM. Required for many contracts. |
| `/ssdf-slsa-provenance [action]` | SLSA attestations for your build artifacts. |

### Code & Build

| Command | Description |
|---------|-------------|
| `/ssdf-coding-standards [scope]` | Checks against OWASP/CERT/CWE guidelines. |
| `/ssdf-secure-defaults [framework]` | Makes sure configs aren't using insecure defaults. |
| `/ssdf-build-hardening [language]` | Checks compiler flags and build security settings. |

### Infrastructure

| Command | Description |
|---------|-------------|
| `/ssdf-integrity-check [scope]` | Verifies commit signing and branch protection. |
| `/ssdf-toolchain-audit [scope]` | Audits your dev tools for known issues. |
| `/ssdf-env-audit [environment]` | Reviews access controls, secrets handling, network config. |

### Vulnerabilities

| Command | Description |
|---------|-------------|
| `/ssdf-vuln-assess [description]` | Assess a specific vulnerability—severity, impact, remediation. |
| `/ssdf-vuln-patterns [period]` | Looks at your vulnerability history for recurring issues. |

### Compliance & Reporting

| Command | Description |
|---------|-------------|
| `/ssdf-compliance-report [scope]` | Overall compliance status against SSDF practices. |
| `/ssdf-evidence-export [format]` | Bundles evidence for auditors. |
| `/ssdf-metrics [period]` | Security metrics and trends. |

### Policy & Automation

| Command | Description |
|---------|-------------|
| `/ssdf-policy-as-code [action]` | Define security policies that get checked automatically. |
| `/ssdf-continuous-monitoring [action]` | Set up git hooks and CI/CD integration. |

### Team

| Command | Description |
|---------|-------------|
| `/ssdf-training-gaps [team]` | Figures out what security training your team needs based on actual issues. |
| `/ssdf-threat-model-evolution [action]` | Tracks threat model changes over time. |

## How It Works

1. You run a command
2. The plugin dispatches AI agents with the right expertise (security-auditor for reviews, dependency-manager for deps, etc.)
3. They analyze your code and configs
4. Findings get written to `.claude/ssdf/evidence/` with timestamps

The evidence files have everything an auditor wants: what was checked, what was found, severity levels, and remediation steps.

## Quick Start

Try these to get a feel for it:

```
/ssdf-security-review src/
```
Runs a security review on your source code.

```
/ssdf-dependency-check
```
Scans your package files for known vulnerabilities.

```
/ssdf-compliance-report
```
Shows where you stand against SSDF practices.

## Tips

- Start with `/ssdf-security-review` on your most important code
- Set up `/ssdf-continuous-monitoring` early so checks run automatically
- Run scans regularly
- The evidence files are useful for security conversations with stakeholders—concrete findings beat vague assurances
- If you keep finding the same types of bugs, `/ssdf-vuln-patterns` can help figure out why

## References

- [NIST SP 800-218 (SSDF)](https://csrc.nist.gov/publications/detail/sp/800-218/final)
- [Executive Order 14028](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/)
- [SLSA](https://slsa.dev/)
- [OWASP](https://owasp.org/)
- [CERT Secure Coding](https://wiki.sei.cmu.edu/confluence/display/seccode)
- [CWE](https://cwe.mitre.org/)
- [Beads Issue Tracker](https://github.com/steveyegge/beads)

## License

MIT
