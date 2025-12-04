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

```bash
# Clone it
git clone https://github.com/your-org/ssdf-cc.git
cd ssdf-cc

# Copy to your Claude Code config
cp -r .claude/skills/ssdf-*.md ~/.claude/skills/
cp -r .claude/commands/ssdf-*.md ~/.claude/commands/

# Copy project config to your target project
cp -r .claude/ssdf /path/to/your/project/.claude/
```

Then edit `.claude/ssdf/ssdf-config.yaml` for your project.

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
- Run scans regularly, not just before releases
- The evidence files are useful for security conversations with stakeholders—concrete findings beat vague assurances
- If you keep finding the same types of bugs, `/ssdf-vuln-patterns` can help figure out why

## References

- [NIST SP 800-218 (SSDF)](https://csrc.nist.gov/publications/detail/sp/800-218/final)
- [Executive Order 14028](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/)
- [SLSA](https://slsa.dev/)
- [OWASP](https://owasp.org/)
- [CERT Secure Coding](https://wiki.sei.cmu.edu/confluence/display/seccode)
- [CWE](https://cwe.mitre.org/)

## License

MIT
