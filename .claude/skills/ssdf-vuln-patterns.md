---
name: ssdf-vuln-patterns
description: Use to analyze vulnerability patterns and identify root causes per SSDF RV.3. Finds recurring issues and suggests preventive measures. Dispatches error-detective and security-auditor agents.
---

# SSDF Vulnerability Pattern Analysis (RV.3)

## Overview

Analyze vulnerability history to identify recurring patterns, common root causes, and systemic issues. Enables proactive prevention rather than reactive remediation.

**SSDF References:**
- RV.3.3 (Analyze Vulnerabilities for Root Causes)
- RV.3.4 (Review and Improve Development Practices)

## When to Use

**Required for:**
- Quarterly security reviews
- Post-incident analysis
- Process improvement planning

**Recommended for:**
- After multiple similar vulnerabilities
- Team retrospectives
- Security training planning

## Pattern Analysis Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: error-detective**
```
Analyze vulnerability patterns for:
[Repository path]

Tasks:
1. Collect all vulnerability data
2. Cluster by type, component, timing
3. Identify recurring patterns
4. Correlate with code changes
5. Calculate pattern statistics
```

**Agent 2: security-auditor**
```
Root cause analysis for:
[Repository path]

Tasks:
1. Trace vulnerabilities to origins
2. Identify systemic causes
3. Map to development practices
4. Assess control effectiveness
5. Recommend preventive measures
```

### Step 2: Data Collection

**Sources to analyze:**
```bash
# SSDF evidence files
find .claude/ssdf/evidence -name "*.md" -path "*vulnerabilities*"
find .claude/ssdf/evidence -name "*.md" -path "*security-reviews*"

# Git commit history
git log --grep="CVE\|vulnerability\|security" --oneline

# Dependency audit history
ls .claude/ssdf/evidence/dependencies/

# Issue tracker
bd list --label=security --status=closed
```

**Vulnerability data points:**
- CVE/CWE identifier
- Severity (CVSS score)
- Discovery date
- Resolution date
- Affected component
- Root cause category
- Developer/team
- Introduction commit
- Fix commit

### Step 3: Pattern Categories

#### By Vulnerability Type (CWE)
| Category | Examples |
|----------|----------|
| Injection | CWE-89, CWE-78, CWE-79 |
| Auth/Access | CWE-287, CWE-862, CWE-863 |
| Cryptography | CWE-327, CWE-328, CWE-330 |
| Memory | CWE-787, CWE-125, CWE-416 |
| Data Exposure | CWE-200, CWE-532, CWE-312 |

#### By Component
- API endpoints
- Authentication module
- Data layer
- Frontend
- Third-party libraries
- Configuration

#### By Root Cause
| Root Cause | Description |
|------------|-------------|
| Knowledge Gap | Developer unaware of secure practice |
| Time Pressure | Security shortcuts for deadlines |
| Tool Gap | Missing automated checks |
| Process Gap | No security review required |
| Design Flaw | Architectural security issue |
| Dependency | Third-party vulnerability |

#### By Introduction Path
- New feature development
- Bug fixes
- Refactoring
- Dependency updates
- Configuration changes

### Step 4: Statistical Analysis

**Metrics to calculate:**
```
Pattern Frequency = Count(vulns in category) / Total vulns × 100

Recurrence Rate = Count(same type reintroduced) / Total fixed × 100

Mean Time to Introduce = Avg(time between similar vulns)

Cluster Size = Count(related vulns in time window)
```

**Trend detection:**
- Increasing pattern (>20% MoM growth)
- Decreasing pattern (>20% MoM reduction)
- Seasonal pattern (consistent timing)
- Burst pattern (clustered around events)

### Step 5: Root Cause Analysis

**5 Whys technique:**
```
1. Why did the SQL injection occur?
   → User input was concatenated into query

2. Why was input concatenated?
   → Developer used string formatting

3. Why string formatting instead of parameterization?
   → Developer unfamiliar with ORM

4. Why unfamiliar with ORM?
   → No secure coding training provided

5. Why no training?
   → No onboarding security curriculum

ROOT CAUSE: Missing developer security training
```

**Fishbone diagram categories:**
- People (training, awareness)
- Process (review, testing)
- Tools (scanners, linters)
- Environment (pressure, culture)
- Materials (documentation, standards)

### Step 6: Correlation Analysis

**Correlate with:**
```bash
# Code complexity
find . -name "*.js" -exec wc -l {} \; | sort -n

# Change frequency
git log --format=format: --name-only | sort | uniq -c | sort -rn

# Team assignments
git shortlog -sn --all

# Timeline events
# - Releases
# - Team changes
# - Process changes
```

### Step 7: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/vuln-patterns/YYYY-MM-DD-vuln-patterns.md
```

**Report template:**
```markdown
# Vulnerability Pattern Analysis Report

**Date:** YYYY-MM-DD
**Period:** [analysis period]
**Project:** [project name]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** RV.3.3, RV.3.4

## Executive Summary

### Key Findings
- **Total Vulnerabilities Analyzed:** X
- **Distinct Patterns Identified:** Y
- **Top Pattern:** [pattern name] (Z% of total)
- **Primary Root Cause:** [cause]
- **Recommended Focus Area:** [area]

### Pattern Distribution
| Pattern | Count | % | Trend |
|---------|-------|---|-------|
| Injection | X | Y% | ↑/→/↓ |
| Auth Issues | X | Y% | ↑/→/↓ |
| Dependencies | X | Y% | ↑/→/↓ |
| Config | X | Y% | ↑/→/↓ |
| Other | X | Y% | ↑/→/↓ |

## Pattern Analysis

### Pattern 1: [Name] (X occurrences)

**Description:** [what the pattern represents]

**Examples:**
| CVE/Issue | Date | Component | Severity |
|-----------|------|-----------|----------|
| | | | |

**Root Cause Analysis:**
```
Why 1: [finding]
Why 2: [finding]
Why 3: [finding]
Why 4: [finding]
Why 5: [ROOT CAUSE]
```

**Contributing Factors:**
- [ ] Process gap
- [ ] Tool gap
- [ ] Knowledge gap
- [ ] Time pressure
- [ ] Design flaw

**Preventive Measures:**
1. [Specific action]
2. [Specific action]

### Pattern 2: [Name] (X occurrences)
[Repeat structure]

## Component Analysis

### Vulnerability Hotspots
| Component | Vulns | % | Risk Level |
|-----------|-------|---|------------|
| src/api/ | X | Y% | High |
| src/auth/ | X | Y% | High |
| lib/ | X | Y% | Medium |

### Dependency Vulnerabilities
| Package | CVEs | Recurrence | Action |
|---------|------|------------|--------|
| [name] | X | Y times | [action] |

## Timeline Analysis

### Monthly Trend
```
Month    | Critical | High | Medium | Low | Total
---------|----------|------|--------|-----|------
[M-6]    | X        | X    | X      | X   | X
[M-5]    | X        | X    | X      | X   | X
[M-4]    | X        | X    | X      | X   | X
[M-3]    | X        | X    | X      | X   | X
[M-2]    | X        | X    | X      | X   | X
[M-1]    | X        | X    | X      | X   | X
```

### Correlation Events
| Date | Event | Vulnerability Impact |
|------|-------|---------------------|
| | Release X.Y | +N vulns introduced |
| | Team change | Pattern shift |

## Root Cause Summary

| Root Cause | Occurrences | Impact | Priority |
|------------|-------------|--------|----------|
| [cause] | X | High/Med/Low | P1/P2/P3 |

### Systemic Issues
1. **[Issue Name]**
   - Affected patterns: X, Y, Z
   - Estimated impact: X% reduction if addressed
   - Recommended action: [action]

## Recommendations

### Immediate Actions (P1)
| Action | Addresses | Expected Impact |
|--------|-----------|-----------------|
| [action] | [patterns] | X% reduction |

### Short-term Actions (P2)
| Action | Addresses | Expected Impact |
|--------|-----------|-----------------|
| [action] | [patterns] | X% reduction |

### Long-term Actions (P3)
| Action | Addresses | Expected Impact |
|--------|-----------|-----------------|
| [action] | [patterns] | X% reduction |

## Process Improvements

### Development
- [ ] Add [specific check] to code review
- [ ] Implement [specific tool/gate]

### Testing
- [ ] Add [specific test type]
- [ ] Increase coverage for [area]

### Training
- [ ] [Specific training topic]
- [ ] Target audience: [role]

### Tooling
- [ ] Add [tool] to pipeline
- [ ] Configure [existing tool] for [check]

## Success Metrics

| Metric | Current | Target | Timeline |
|--------|---------|--------|----------|
| [Pattern] rate | X% | Y% | Q# |
| MTTR | X days | Y days | Q# |
| Recurrence rate | X% | Y% | Q# |

## Sign-Off

- [ ] All significant patterns identified
- [ ] Root causes documented
- [ ] Recommendations prioritized
- [ ] Evidence archived
```

## Integration

**Create improvement issues:**
```bash
bd create "Process: [Pattern Prevention]" --type task -d "Address recurring vulnerability pattern. See analysis: .claude/ssdf/evidence/vuln-patterns/[date].md"
```

## Commit Message Format

```
[SSDF:RV.3.3] Analyze vulnerability patterns

- Identified X recurring patterns
- Primary root cause: [cause]
- Evidence: .claude/ssdf/evidence/vuln-patterns/YYYY-MM-DD.md
```

## Prevention Strategies

### For Injection Patterns
- Mandatory parameterized queries
- Input validation library
- SAST rule enforcement

### For Auth Patterns
- Auth framework standardization
- Session management library
- Access control review checklist

### For Dependency Patterns
- Automated dependency updates
- Version pinning policy
- Private registry with scanning

### For Configuration Patterns
- Configuration-as-code
- Secret management solution
- Environment validation
