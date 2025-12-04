---
name: ssdf-training-gaps
description: Use to identify security training gaps per SSDF PO.2 (enhanced). Analyzes vulnerability patterns to recommend training. Dispatches data-analyst and security-auditor agents.
---

# SSDF Training Gap Analysis (PO.2 Enhanced)

## Overview

Analyze vulnerability patterns, code review findings, and security issues to identify training gaps and recommend targeted security education.

**SSDF References:**
- PO.2.2 (Train Development Team on Security)

## When to Use

**Required for:**
- Annual training planning
- After pattern analysis
- New team onboarding

**Recommended for:**
- Quarterly reviews
- Post-incident analysis
- Team expansion

## Training Gap Analysis Process

### Step 1: Dispatch Agents

**Agent 1: data-analyst**
```
Analyze security patterns for training gaps:
[Repository path]

Tasks:
1. Correlate vulnerabilities by type
2. Map issues to contributors
3. Identify recurring patterns
4. Calculate knowledge scores
5. Prioritize training topics
```

**Agent 2: security-auditor**
```
Assess training needs for:
[Team/repository]

Tasks:
1. Review vulnerability history
2. Assess by security domain
3. Identify high-impact gaps
4. Recommend resources
```

### Step 2: Data Sources

**Analyze:**
- Vulnerability history
- Code review comments
- Commit patterns
- Security scan results

### Step 3: Gap Identification

**Training domains:**
| Domain | Topics |
|--------|--------|
| Secure Coding | Input validation, output encoding, injection prevention |
| Authentication | Session management, MFA, credential handling |
| Cryptography | Algorithm selection, key management |
| Architecture | Trust boundaries, defense in depth |

### Step 4: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/training/YYYY-MM-DD-training-gaps.md
```

**Report template:**
```markdown
# Training Gap Analysis Report

**Date:** YYYY-MM-DD
**Scope:** [team/all]
**Project:** [project name]
**SSDF Reference:** PO.2.2

## Summary

| Domain | Gap Level | Priority | Recommended Hours |
|--------|-----------|----------|-------------------|
| Input Validation | High | P1 | 8 |
| Cryptography | Medium | P2 | 4 |

## Detailed Analysis

### By Topic
| Topic | Issues | Contributors | Training Need |
|-------|--------|--------------|---------------|

### By Team/Role
| Team | Primary Gaps | Secondary Gaps |
|------|--------------|----------------|

## Recommended Training

### Priority 1 (Immediate)
1. **Topic:** [name]
   - Issues addressed: X
   - Audience: [roles]
   - Resources: [links]

### Priority 2 (Quarter)
1. **Topic:** [name]
   - Issues addressed: X
   - Audience: [roles]

## Training Resources

| Topic | Resource | Type | Duration |
|-------|----------|------|----------|
| OWASP Top 10 | OWASP Training | Online | 8h |

## Success Metrics

| Metric | Current | Target |
|--------|---------|--------|
| [Domain] issues/quarter | X | Y |

## Sign-Off

- [ ] Gaps identified and prioritized
- [ ] Training plan created
- [ ] Resources allocated
```

## Commit Message Format

```
[SSDF:PO.2.2] Training gap analysis

- Identified X priority gaps
- Training plan created
- Evidence: .claude/ssdf/evidence/training/YYYY-MM-DD.md
```
