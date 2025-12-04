---
name: ssdf-architecture-review
description: Use to review software architecture for security per SSDF PW.2. Analyzes design patterns, attack surface, trust boundaries. Dispatches microservices-architect and security-auditor agents.
---

# SSDF Architecture Security Review (PW.2)

## Overview

Review software architecture for security including design patterns, attack surface analysis, and trust boundary identification.

**SSDF References:**
- PW.2.1 (Design Software to Meet Security Requirements)

## When to Use

**Required for:**
- New system design
- Major architectural changes
- Security assessments
- Compliance audits

**Recommended for:**
- Quarterly architecture reviews
- Before adding external integrations
- After security incidents

## Architecture Review Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: microservices-architect**
```
Analyze system architecture for:
[Repository/system path]

Tasks:
1. Map system components and interactions
2. Identify architectural patterns in use
3. Assess scalability and resilience
4. Review API boundaries
5. Evaluate data flow patterns
```

**Agent 2: security-auditor**
```
Security architecture review for:
[Repository/system path]

Tasks:
1. Identify trust boundaries
2. Map attack surface
3. Assess authentication/authorization architecture
4. Review data protection patterns
5. Evaluate defense in depth
```

### Step 2: Architecture Discovery

**Identify components from:**
```bash
# Service definitions
find . -name "docker-compose*.yml" -o -name "*.yaml" -path "*k8s/*"

# API definitions
find . -name "openapi*.yaml" -o -name "swagger*.json"

# Module structure
ls -la src/

# Dependencies
cat package.json | jq '.dependencies'
```

### Step 3: Trust Boundary Analysis

**Trust boundaries to identify:**
- External ↔ Internal network
- Public ↔ Authenticated
- User ↔ Admin
- Service ↔ Service
- Application ↔ Database

**Security controls at boundaries:**
| Boundary | Control | Status |
|----------|---------|--------|
| Internet → API | WAF, Rate limiting | |
| API → Database | Parameterized queries | |
| Service → Service | mTLS | |

### Step 4: Attack Surface Analysis

**Attack surface components:**
- Public endpoints
- Authentication mechanisms
- File upload handlers
- External integrations
- Admin interfaces

### Step 5: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/architecture/YYYY-MM-DD-architecture-review.md
```

**Report template:**
```markdown
# Architecture Security Review

**Date:** YYYY-MM-DD
**System:** [system name]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PW.2.1

## System Overview

[Architecture diagram description]

## Components

| Component | Type | Function | Exposure |
|-----------|------|----------|----------|

## Trust Boundaries

| Boundary | From | To | Controls |
|----------|------|----|---------|

## Attack Surface

### External Endpoints
| Endpoint | Method | Auth | Risk |
|----------|--------|------|------|

### High-Risk Areas
| Area | Risk | Mitigation |
|------|------|------------|

## Security Patterns

| Pattern | Implemented | Status |
|---------|-------------|--------|
| Defense in depth | Yes/No | |
| Least privilege | Yes/No | |
| Fail secure | Yes/No | |

## Recommendations

1. [Prioritized recommendations]

## Sign-Off

- [ ] All components mapped
- [ ] Trust boundaries identified
- [ ] Attack surface documented
```

## Commit Message Format

```
[SSDF:PW.2.1] Architecture security improvements

- Enhanced trust boundary controls
- Reduced attack surface
- Evidence: .claude/ssdf/evidence/architecture/YYYY-MM-DD.md
```
