---
name: ssdf-integrity-check
description: Use to verify code repository integrity - validates commit signing, branch protection, and release integrity per SSDF PS.1 and PS.2. Dispatches security-auditor and git-workflow-manager agents.
---

# SSDF Integrity Check (PS.1, PS.2)

## Overview

Verify integrity of code repositories and releases. Ensures code hasn't been tampered with and security controls are in place.

**SSDF References:**
- PS.1.1 (Store Code Based on Least Privilege)
- PS.2.1 (Make Integrity Verification Available)

**EO14028 References:**
- 4e(iii) - Maintaining trusted build environments
- 4e(x) - Integrity verification mechanisms

## When to Use

**Required before:**
- Any release to production
- Audit preparation
- After security incidents

**Recommended for:**
- Regular security posture checks
- New repository setup verification
- CI/CD pipeline configuration

## Integrity Verification Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: security-auditor**
```
Verify code integrity for the repository:
[Repository path]

Check:
1. Commit signing configuration (GPG/SSH)
2. Unsigned commits in history
3. Release tag signatures
4. Checksum files for releases
5. Code signing certificates (if applicable)
6. Branch protection status
```

**Agent 2: git-workflow-manager**
```
Analyze git configuration for security:
[Repository path]

Verify:
1. commit.gpgsign setting
2. Branch protection rules on main/master
3. Required reviewers configuration
4. CI status check requirements
5. Force-push protection
6. Tag signing configuration
```

### Step 2: Commit Signing Verification

**Check signing configuration:**
```bash
git config --get commit.gpgsign
git config --get user.signingkey
git config --get gpg.format  # gpg or ssh
```

**Analyze commit history:**
```bash
# Count signed vs unsigned commits (last 100)
git log --oneline -100 --show-signature 2>/dev/null | grep -c "Good signature"

# List unsigned commits
git log --oneline -100 --format="%H %s" | while read hash msg; do
  if ! git verify-commit "$hash" 2>/dev/null; then
    echo "UNSIGNED: $hash - $msg"
  fi
done
```

**Verification checklist:**
- [ ] Signing enabled (`commit.gpgsign = true`)
- [ ] Signing key configured
- [ ] Recent commits are signed (target: 100%)
- [ ] Key not expired
- [ ] Key matches authorized committers

### Step 3: Branch Protection Analysis

**For GitHub repositories:**
```bash
# Check branch protection via gh CLI
gh api repos/{owner}/{repo}/branches/main/protection
```

**Expected protections:**
| Protection | Required | Status |
|------------|----------|--------|
| Require PR reviews | Yes | |
| Dismiss stale reviews | Yes | |
| Require status checks | Yes | |
| Require signed commits | Recommended | |
| Include administrators | Yes | |
| Restrict force push | Yes | |
| Restrict deletions | Yes | |

**For GitLab repositories:**
```bash
# Check protected branches
glab api projects/:id/protected_branches
```

### Step 4: Release Integrity Verification

**Check release tags:**
```bash
# List tags and verify signatures
git tag -l -n1 | while read tag msg; do
  if git verify-tag "$tag" 2>/dev/null; then
    echo "SIGNED: $tag"
  else
    echo "UNSIGNED: $tag"
  fi
done
```

**Verify checksums exist:**
```bash
# Check for checksum files in releases
ls -la *.sha256 *.sha512 CHECKSUMS* 2>/dev/null
```

**Release integrity checklist:**
- [ ] Release tags are signed
- [ ] SHA-256 checksums published
- [ ] Checksums are signed/verified
- [ ] Release artifacts match checksums
- [ ] No unauthorized modifications

### Step 5: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/integrity/YYYY-MM-DD-integrity-check.md
```

**Report template:**
```markdown
# Integrity Verification Report

**Date:** YYYY-MM-DD
**Repository:** [repo name]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PS.1.1, PS.2.1

## Executive Summary

| Metric | Value | Status |
|--------|-------|--------|
| Commits Analyzed | X | |
| Signed Commits | Y (Z%) | PASS/WARN/FAIL |
| Branch Protection | Enabled/Partial/None | PASS/WARN/FAIL |
| Signed Releases | X/Y | PASS/WARN/FAIL |

## Commit Signing

### Configuration
- `commit.gpgsign`: [true/false]
- `gpg.format`: [gpg/ssh]
- Signing key: [configured/not configured]

### Compliance
| Period | Total | Signed | Percentage |
|--------|-------|--------|------------|
| Last 30 days | | | |
| Last 90 days | | | |
| All time | | | |

### Unsigned Commits (if any)
| Hash | Author | Date | Subject |
|------|--------|------|---------|

## Branch Protection

### Main Branch
| Rule | Status | Notes |
|------|--------|-------|
| PR required | | |
| Reviews required | | |
| Status checks | | |
| Signed commits | | |
| Force push blocked | | |

### Other Protected Branches
[List other protected branches and their rules]

## Release Integrity

### Signed Releases
| Tag | Signed | Signer | Date |
|-----|--------|--------|------|

### Checksums
| Release | SHA-256 | SHA-512 | Verified |
|---------|---------|---------|----------|

## Findings

| Severity | Issue | Remediation |
|----------|-------|-------------|
| [H/M/L] | [description] | [action] |

## Recommendations

1. [Prioritized recommendations]

## Sign-Off

- [ ] Commit signing compliance >= 95%
- [ ] Branch protection fully configured
- [ ] All releases signed with checksums
- [ ] No critical issues found
```

### Step 6: Remediation (--fix flag)

If `--fix` is specified, apply safe fixes:

**Enable commit signing:**
```bash
git config --local commit.gpgsign true
echo "Commit signing enabled for this repository"
```

**Create pre-commit hook for unsigned commits:**
```bash
cat > .git/hooks/pre-commit << 'EOF'
#!/bin/bash
if [ "$(git config --get commit.gpgsign)" != "true" ]; then
  echo "WARNING: Commit signing is not enabled"
  echo "Run: git config commit.gpgsign true"
fi
EOF
chmod +x .git/hooks/pre-commit
```

**Note:** Branch protection and release signing require manual configuration via GitHub/GitLab UI or API with appropriate permissions.

## Integration

**Create issue for integrity problems:**
```bash
bd create "Security: [Integrity Issue]" --type security -d "Found during SSDF PS.1/PS.2 integrity check. See evidence: .claude/ssdf/evidence/integrity/[date].md"
```

## Commit Message Format

```
[SSDF:PS.1.1] Enable commit signing

- Configured GPG signing for repository
- Evidence: .claude/ssdf/evidence/integrity/YYYY-MM-DD.md
```

## Compliance Thresholds

| Metric | Pass | Warn | Fail |
|--------|------|------|------|
| Signed commits | >= 95% | >= 80% | < 80% |
| Branch protection | Full | Partial | None |
| Signed releases | 100% | >= 90% | < 90% |

## Common Issues and Fixes

**Issue: Signing key not found**
```bash
# List available keys
gpg --list-secret-keys --keyid-format=long

# Configure signing key
git config user.signingkey [KEY_ID]
```

**Issue: SSH signing not working**
```bash
# For SSH signing (Git 2.34+)
git config gpg.format ssh
git config user.signingkey ~/.ssh/id_ed25519.pub
```

**Issue: Verification fails for old commits**
- Document historical unsigned commits
- Set policy for future commits
- Consider `--allow-unverified` for legacy code
