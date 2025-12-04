---
description: Review software architecture for security (PW.2)
arguments:
  - name: scope
    description: Component or system to review (default: entire system)
    required: false
---

# SSDF Architecture Review

Use the `ssdf-architecture-review` skill to assess architecture security.

**Scope:** $ARGUMENTS.scope (or entire system if not specified)

## Process

1. Dispatch agents:
   - `microservices-architect`: Architecture analysis
   - `security-auditor`: Security assessment

2. Map system architecture and data flows
3. Identify trust boundaries and attack surface
4. Assess security design patterns
5. Generate evidence file in `.claude/ssdf/evidence/architecture/`

## Output Requirements

- Architecture diagram analysis
- Trust boundary identification
- Attack surface assessment
- SSDF references (PW.2.1) in commit messages
