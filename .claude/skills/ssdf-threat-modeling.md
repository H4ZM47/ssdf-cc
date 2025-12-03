---
name: ssdf-threat-modeling
description: Use when designing new features, APIs, or components - guides systematic threat modeling per SSDF PW.1. Dispatches security-auditor agent for attack surface analysis.
---

# SSDF Threat Modeling (PW.1)

## Overview

Systematic threat modeling before implementation. Required for new features, API endpoints, data flows, and security-sensitive components.

**SSDF References:** PW.1.1 (Risk Modeling), PW.1.2 (Track Requirements), PW.1.3 (Security Features)

## When to Use

**Required for:**
- New API endpoints or services
- Authentication/authorization changes
- Data storage or processing features
- External integrations
- User input handling
- Cryptographic operations

## Threat Modeling Process

### Step 1: Define Scope

Document what you're building:
```markdown
## Feature: [Name]
**Description:** [What it does]
**Data Handled:** [Types of data]
**Users/Actors:** [Who interacts with it]
**Trust Boundaries:** [Where trust changes]
```

### Step 2: Dispatch Security-Auditor Agent

Use the Task tool to dispatch `security-auditor` agent:

```
Analyze the following feature for security threats:
[Include feature description, data flow, and trust boundaries]

Identify:
1. Attack surfaces (entry points)
2. Potential threats (STRIDE categories)
3. Sensitive data exposure risks
4. Authentication/authorization gaps
5. Recommended mitigations
```

### Step 3: STRIDE Analysis

For each component, consider:

| Category | Question | Example Threats |
|----------|----------|-----------------|
| **S**poofing | Can identity be faked? | Session hijacking, credential theft |
| **T**ampering | Can data be modified? | SQL injection, parameter tampering |
| **R**epudiation | Can actions be denied? | Missing audit logs |
| **I**nformation Disclosure | Can data leak? | Error messages, logs exposing secrets |
| **D**enial of Service | Can it be overwhelmed? | Resource exhaustion, infinite loops |
| **E**levation of Privilege | Can access be escalated? | Missing authorization checks |

### Step 4: Document Mitigations

For each identified threat:
```markdown
### Threat: [Name]
**Category:** [STRIDE category]
**Risk Level:** [High/Medium/Low]
**Attack Vector:** [How it could be exploited]
**Mitigation:** [How to prevent it]
**SSDF Task:** [Reference, e.g., PW.5.1]
```

### Step 5: Generate Evidence

Create evidence file:
```bash
# Filename: .claude/ssdf/evidence/threat-models/YYYY-MM-DD-[feature-name].md
```

**Required sections:**
1. Feature description
2. Data flow diagram (text-based)
3. Trust boundaries
4. Identified threats (STRIDE)
5. Mitigations planned
6. Security requirements derived
7. Agent analysis results

## Data Flow Template

```
[User] --> |HTTPS| --> [API Gateway] --> |Internal| --> [Service]
                                                            |
                                                            v
                                                      [Database]
                                                            |
                                    Trust Boundary ─────────┘
```

## Threat Model Checklist

Before proceeding to implementation:

- [ ] All entry points identified
- [ ] Trust boundaries documented
- [ ] STRIDE analysis completed
- [ ] Mitigations defined for High/Medium risks
- [ ] Security requirements added to implementation plan
- [ ] Evidence file created
- [ ] Beads issue linked (if applicable)

## Integration

**Create tracking issue:**
```bash
bd create "Threat Model: [Feature Name]" --type security -d "SSDF PW.1.1 threat modeling for [feature]"
```

**Reference in commits:**
```
[SSDF:PW.1.1] Design authentication flow with threat mitigations

Threat model: .claude/ssdf/evidence/threat-models/2024-12-03-auth-flow.md
```
