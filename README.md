# SSDF for Claude Code

A Claude Code plugin that helps you build secure software and demonstrate compliance with industry security standards.

## What Problem Does This Solve?

Building secure software is hard. Proving you built it securely is even harder.

Organizations face increasing pressure to demonstrate their software development practices meet security standards—whether from government contracts requiring NIST compliance, enterprise customers demanding security attestations, or internal security teams enforcing policies.

**This plugin helps you:**
- Catch security issues early, when they're cheapest to fix
- Generate audit-ready evidence without manual documentation
- Follow security best practices with AI-guided assistance
- Meet compliance requirements (NIST SSDF, EO14028) systematically

## Who Is This For?

**Development teams** who want to improve their security posture without slowing down delivery.

**Security engineers** who need to scale security reviews across multiple projects and teams.

**Compliance officers** who need evidence that security practices are actually being followed.

**Government contractors** required to meet NIST SP 800-218 (SSDF) or Executive Order 14028 requirements.

**Any organization** that wants to shift security left and build it into their development workflow.

## What Is NIST SSDF?

The **Secure Software Development Framework (SSDF)** is a set of practices published by the National Institute of Standards and Technology (NIST) that describes what organizations should do to reduce vulnerabilities in their software. It's organized into four groups:

| Group | Focus | Example Activities |
|-------|-------|-------------------|
| **Prepare the Organization** | Get your house in order | Define security policies, train developers, secure your tools |
| **Protect the Software** | Guard your code and artifacts | Sign commits, generate SBOMs, verify dependencies |
| **Produce Well-Secured Software** | Build security in | Threat modeling, code review, secure defaults |
| **Respond to Vulnerabilities** | Handle issues effectively | Triage vulnerabilities, analyze root causes, track patterns |

This plugin automates and assists with practices across all four groups.

## Installation

### Quick Install

```bash
# Clone the repository
git clone https://github.com/your-org/ssdf-cc.git
cd ssdf-cc

# Copy skills and commands to your Claude Code config
cp -r .claude/skills/ssdf-*.md ~/.claude/skills/
cp -r .claude/commands/ssdf-*.md ~/.claude/commands/

# Copy project configuration to your target project
cp -r .claude/ssdf /path/to/your/project/.claude/
```

### Configuration

Edit `.claude/ssdf/ssdf-config.yaml` in your project to customize:
- Which practices to enable
- Severity thresholds for findings
- Evidence output preferences
- Team-specific settings

## Available Commands

Use these slash commands in Claude Code to perform security activities. Each command generates evidence documentation automatically.

### Security Reviews & Analysis

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/ssdf-security-review [files]` | Performs a security-focused code review looking for vulnerabilities, injection flaws, authentication issues, and more | Before merging PRs, after major changes, or as part of regular security audits |
| `/ssdf-threat-model [feature]` | Creates a threat model identifying assets, threats, and mitigations using STRIDE methodology | When designing new features, before architecture decisions, or reviewing existing systems |
| `/ssdf-architecture-review [scope]` | Analyzes system architecture for trust boundaries, attack surface, and security patterns | During design reviews, before major releases, or when onboarding to a new codebase |

### Dependency & Supply Chain Security

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/ssdf-dependency-check [package]` | Checks dependencies for known vulnerabilities, outdated versions, and license issues | Before adding new dependencies, during regular maintenance, or after security advisories |
| `/ssdf-sbom-generate [format]` | Generates a Software Bill of Materials in SPDX or CycloneDX format | For releases, customer requests, government contracts, or supply chain audits |
| `/ssdf-slsa-provenance [action]` | Creates SLSA supply chain attestations proving how artifacts were built | For release pipelines, when customers require provenance, or to achieve SLSA levels |

### Code Quality & Standards

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/ssdf-coding-standards [scope]` | Validates code against OWASP, CERT, and CWE secure coding guidelines | During code reviews, for new team members, or establishing baseline compliance |
| `/ssdf-secure-defaults [framework]` | Checks that security configurations use safe defaults (HTTPS, secure cookies, etc.) | When setting up new projects, reviewing configurations, or after framework upgrades |
| `/ssdf-build-hardening [language]` | Analyzes build configuration for security flags and compiler protections | When setting up CI/CD, reviewing build scripts, or hardening release builds |

### Infrastructure & Environment

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/ssdf-integrity-check [scope]` | Verifies commit signing, branch protection rules, and release integrity | Before releases, during security audits, or setting up new repositories |
| `/ssdf-toolchain-audit [scope]` | Audits development tools (compilers, package managers, IDE plugins) for security | Periodically, after tool updates, or when establishing secure development environments |
| `/ssdf-env-audit [environment]` | Reviews environment security including access controls, secrets management, and network segmentation | When provisioning environments, during compliance audits, or after incidents |

### Vulnerability Management

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/ssdf-vuln-assess [description]` | Assesses a specific vulnerability with severity rating, impact analysis, and remediation guidance | When vulnerabilities are reported, triaging security findings, or incident response |
| `/ssdf-vuln-patterns [period]` | Analyzes historical vulnerabilities to identify patterns and systemic issues | Quarterly reviews, after multiple incidents, or when planning security improvements |

### Compliance & Reporting

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/ssdf-compliance-report [scope]` | Generates a comprehensive compliance status report against SSDF practices | For audits, management reviews, or customer security questionnaires |
| `/ssdf-evidence-export [format]` | Exports all collected evidence in a structured format for auditors | Before audits, for compliance submissions, or archiving |
| `/ssdf-metrics [period]` | Generates security metrics dashboard showing trends and compliance scores | For executive reporting, team retrospectives, or continuous improvement |

### Policy & Governance

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/ssdf-policy-as-code [action]` | Defines and validates security policies as code, enabling automated enforcement | When establishing governance, automating compliance checks, or managing exceptions |
| `/ssdf-continuous-monitoring [action]` | Configures git hooks and CI/CD integration for ongoing security checks | When setting up new projects, improving DevSecOps maturity, or automating gates |

### Team Development

| Command | What It Does | When to Use |
|---------|--------------|-------------|
| `/ssdf-training-gaps [team]` | Analyzes vulnerability patterns to identify security training needs | For training planning, after incidents, or when onboarding teams |
| `/ssdf-threat-model-evolution [action]` | Tracks how threat models change over time as the system evolves | When updating threat models, reviewing security posture, or during architecture changes |

## How It Works

When you run a command, the plugin:

1. **Dispatches specialized AI agents** - Different security tasks require different expertise. A code review uses a code-reviewer agent, while a dependency check uses a dependency-manager agent.

2. **Analyzes your code and configuration** - The agents examine relevant files, configurations, and patterns in your codebase.

3. **Generates findings** - Issues are identified with severity levels, locations, and remediation guidance.

4. **Creates evidence documentation** - Every activity generates a timestamped evidence file in `.claude/ssdf/evidence/` that can be used for audits.

## Evidence & Audit Trail

All security activities automatically generate evidence files organized by type:

```
.claude/ssdf/evidence/
├── security-reviews/     # Code review findings
├── threat-models/        # Threat modeling artifacts
├── dependencies/         # Dependency analysis results
├── sbom/                 # Software Bills of Materials
├── architecture/         # Architecture review findings
└── ...                   # Other evidence types
```

Each evidence file includes:
- **Timestamp** - When the activity was performed
- **Scope** - What was analyzed
- **Findings** - Issues discovered with severity and remediation
- **Sign-off checklist** - Verification that required steps were completed

This creates a continuous audit trail that demonstrates your security practices to auditors, customers, and stakeholders.

## Getting Started

### 1. Run Your First Security Review

```
/ssdf-security-review src/
```

This analyzes your source code for common vulnerabilities and generates a findings report.

### 2. Check Your Dependencies

```
/ssdf-dependency-check
```

This scans your dependency files (package.json, requirements.txt, etc.) for known vulnerabilities.

### 3. Generate a Compliance Report

```
/ssdf-compliance-report
```

This shows your current compliance status against SSDF practices and identifies gaps.

### 4. Create a Threat Model

```
/ssdf-threat-model "user authentication flow"
```

This walks you through threat modeling for a specific feature or component.

## Best Practices

**Start with security reviews** - Run `/ssdf-security-review` on your most critical code paths first.

**Integrate into your workflow** - Use `/ssdf-continuous-monitoring setup` to add security checks to your git hooks and CI/CD.

**Review regularly, not just once** - Security is ongoing. Run commands periodically, not just at release time.

**Use evidence for conversations** - The generated evidence helps communicate security status to stakeholders in concrete terms.

**Address patterns, not just bugs** - Use `/ssdf-vuln-patterns` to find systemic issues rather than playing whack-a-mole with individual vulnerabilities.

## Related Standards & Frameworks

This plugin helps you align with:

- **[NIST SP 800-218 (SSDF)](https://csrc.nist.gov/publications/detail/sp/800-218/final)** - The Secure Software Development Framework
- **[Executive Order 14028](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/)** - Improving the Nation's Cybersecurity
- **[SLSA](https://slsa.dev/)** - Supply-chain Levels for Software Artifacts
- **[OWASP](https://owasp.org/)** - Open Web Application Security Project guidelines
- **[CERT Secure Coding](https://wiki.sei.cmu.edu/confluence/display/seccode)** - SEI CERT Coding Standards
- **[CWE](https://cwe.mitre.org/)** - Common Weakness Enumeration

## License

MIT
