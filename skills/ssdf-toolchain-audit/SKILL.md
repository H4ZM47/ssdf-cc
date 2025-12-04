---
name: ssdf-toolchain-audit
description: Use to audit development toolchain for security per SSDF PO.3. Checks compilers, package managers, build tools, and IDE plugins. Dispatches devops-engineer and security-auditor agents.
---

# SSDF Toolchain Audit (PO.3)

## Overview

Audit the security of development toolchain including compilers, package managers, build tools, and IDE extensions. Ensures tools are up-to-date and securely configured.

**SSDF References:**
- PO.3.1 (Specify Tools for Software Development)
- PO.3.2 (Evaluate and Manage Tool Risk)

## When to Use

**Required for:**
- Initial project setup
- Annual security audits
- After toolchain changes

**Recommended for:**
- Quarterly reviews
- New team member onboarding
- CI/CD pipeline updates

## Toolchain Audit Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: devops-engineer**
```
Inventory development toolchain for:
[Repository path]

Tasks:
1. Identify compilers and interpreters in use
2. List package managers and versions
3. Document build tools and versions
4. Check for version pinning
5. Identify CI/CD tools
```

**Agent 2: security-auditor**
```
Security assessment of toolchain for:
[Repository path]

Tasks:
1. Check tools against CVE databases
2. Verify tool signatures/checksums
3. Assess configuration security
4. Identify EOL or deprecated tools
5. Check for known supply chain issues
```

### Step 2: Tool Discovery

**Compilers and Interpreters:**
```bash
# Node.js
node --version
npm --version

# Python
python3 --version
pip --version

# Go
go version

# Rust
rustc --version
cargo --version

# Java
java --version
javac --version

# C/C++
gcc --version
clang --version
cmake --version
```

**Package Managers:**
```bash
# Check package manager versions
npm --version
yarn --version
pnpm --version
pip --version
poetry --version
cargo --version
go version
bundler --version
composer --version
```

**Build Tools:**
```bash
# Common build tools
make --version
cmake --version
gradle --version
mvn --version
```

### Step 3: Vulnerability Check

**Check tools against vulnerability databases:**

| Tool | Check Method |
|------|--------------|
| Node.js | nodejs.org/en/blog/vulnerability/ |
| npm | npm audit (for npm itself) |
| Python | python.org/downloads/ |
| Go | golang.org/doc/devel/release |
| Rust | rust-lang.org/security.html |
| Java | oracle.com/security-alerts/ |

**Version freshness check:**
```bash
# Example: Check if Node.js is current
# Compare installed version against LTS
node -v  # Should be recent LTS
```

**Known vulnerable versions to flag:**
- Node.js < 18.x (EOL)
- Python < 3.8 (EOL)
- npm < 9.x (older versions have issues)
- Go < 1.20 (older security model)

### Step 4: Configuration Security

**npm/Node.js:**
```bash
# Check npm configuration
npm config list
# Look for:
# - audit-level setting
# - ignore-scripts (should be false for trusted packages only)
# - registry (should be official or trusted private)
```

**pip/Python:**
```bash
# Check pip configuration
pip config list
# Verify trusted hosts
# Check index-url settings
```

**Git:**
```bash
# Check git security settings
git config --list | grep -E "gpgsign|credential"
# Should have commit signing enabled
```

### Step 5: IDE/Editor Plugins

**VS Code extensions audit:**
```bash
# List installed extensions
code --list-extensions

# Security-relevant extensions to check:
# - Publisher verification
# - Last update date
# - Known vulnerabilities
```

**Check for risky patterns:**
- Extensions from unknown publishers
- Extensions with excessive permissions
- Outdated extensions (>1 year)

### Step 6: CI/CD Tool Security

**GitHub Actions:**
```yaml
# Check .github/workflows/*.yml for:
# - Pinned action versions (use @sha not @v1)
# - Trusted actions only
# - No secrets in logs
```

**Docker:**
```bash
# Check Docker version
docker --version

# Check for security options
docker info | grep -E "Security|seccomp"
```

### Step 7: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/toolchain/YYYY-MM-DD-toolchain-audit.md
```

**Report template:**
```markdown
# Toolchain Security Audit Report

**Date:** YYYY-MM-DD
**Project:** [project name]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PO.3.1, PO.3.2

## Executive Summary

| Category | Tools | Issues | Status |
|----------|-------|--------|--------|
| Compilers | X | Y | PASS/WARN/FAIL |
| Package Managers | X | Y | PASS/WARN/FAIL |
| Build Tools | X | Y | PASS/WARN/FAIL |
| IDE Extensions | X | Y | PASS/WARN/FAIL |
| CI/CD | X | Y | PASS/WARN/FAIL |

## Tool Inventory

### Compilers and Interpreters
| Tool | Version | Latest | Status | CVEs |
|------|---------|--------|--------|------|
| Node.js | X.Y.Z | A.B.C | Current/Outdated | None/List |
| Python | X.Y.Z | A.B.C | Current/Outdated | None/List |

### Package Managers
| Tool | Version | Latest | Status | Notes |
|------|---------|--------|--------|-------|

### Build Tools
| Tool | Version | Latest | Status | Notes |
|------|---------|--------|--------|-------|

### CI/CD Tools
| Tool | Version | Configuration | Status |
|------|---------|---------------|--------|

## Vulnerability Assessment

### Critical Vulnerabilities
| Tool | Version | CVE | Severity | Fixed In |
|------|---------|-----|----------|----------|

### High Vulnerabilities
| Tool | Version | CVE | Severity | Fixed In |
|------|---------|-----|----------|----------|

## Configuration Issues

| Tool | Issue | Risk | Recommendation |
|------|-------|------|----------------|

## IDE/Editor Security

### VS Code Extensions
| Extension | Publisher | Last Updated | Status |
|-----------|-----------|--------------|--------|

### Risky Extensions
| Extension | Issue | Recommendation |
|-----------|-------|----------------|

## CI/CD Security

### GitHub Actions
| Workflow | Issues | Recommendation |
|----------|--------|----------------|

### Action Pinning
| Action | Current | Recommended |
|--------|---------|-------------|
| actions/checkout | @v4 | @<sha> |

## Recommendations

### Immediate Actions (Critical)
1. [Upgrade vulnerable tools]

### Short-term Actions (High)
1. [Update outdated tools]

### Long-term Actions (Medium)
1. [Improve configurations]

## Compliance Checklist

- [ ] All tools inventoried
- [ ] No critical vulnerabilities
- [ ] No EOL tools in production use
- [ ] Tool configurations reviewed
- [ ] CI/CD actions pinned to SHA
- [ ] IDE extensions from trusted sources

## Sign-Off

- [ ] Toolchain meets security baseline
- [ ] All critical issues addressed
- [ ] Audit evidence archived
```

## Integration

**Create issue for toolchain problems:**
```bash
bd create "Security: [Toolchain Issue]" --type security -d "Found during SSDF PO.3 toolchain audit. See evidence: .claude/ssdf/evidence/toolchain/[date].md"
```

## Commit Message Format

```
[SSDF:PO.3.1] Update toolchain security

- Upgraded Node.js to LTS version
- Pinned GitHub Actions to SHA
- Evidence: .claude/ssdf/evidence/toolchain/YYYY-MM-DD.md
```

## Tool Security Baselines

### Node.js/npm
- Use LTS version
- Enable `npm audit` in CI
- Use `package-lock.json`
- Consider `npm ci` over `npm install`

### Python/pip
- Use Python 3.10+
- Use `pip-compile` for lockfiles
- Enable hash checking
- Use virtual environments

### Go
- Use Go 1.21+
- Enable `go mod verify`
- Use `go.sum` for verification

### Rust/Cargo
- Use stable channel
- Enable `cargo audit`
- Review `Cargo.lock`

### Docker
- Use specific image tags (not :latest)
- Enable content trust
- Use minimal base images
- Scan images for vulnerabilities
