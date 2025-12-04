---
description: Generate SSDF compliance status report
arguments:
  - name: scope
    description: Report scope (project, organization)
    required: false
---

# SSDF Compliance Report

Generate comprehensive SSDF compliance status report.

**Scope:** $ARGUMENTS.scope (default: current project)

## Report Sections

### 1. Executive Summary
- Overall compliance status
- Key metrics and trends
- Critical findings requiring attention

### 2. Practice Group Coverage

#### PO - Prepare Organization
- [ ] PO.1: Security requirements defined
- [ ] PO.2: Security roles assigned
- [ ] PO.3: Supporting toolchains implemented
- [ ] PO.4: Secure environments configured
- [ ] PO.5: Criteria for software security defined

#### PS - Protect Software
- [ ] PS.1: Code protection measures
- [ ] PS.2: Archive protection
- [ ] PS.3: SBOM maintained and current

#### PW - Produce Well-Secured Software
- [ ] PW.1: Threat models for all features
- [ ] PW.2: Security automation in CI/CD
- [ ] PW.4: Third-party components verified
- [ ] PW.5: Secure coding practices enforced
- [ ] PW.6: Build process security
- [ ] PW.7: Code review process
- [ ] PW.8: Security testing performed
- [ ] PW.9: Default configurations secure

#### RV - Respond to Vulnerabilities
- [ ] RV.1: Vulnerability identification process
- [ ] RV.2: Vulnerability assessment and response
- [ ] RV.3: Root cause analysis performed

### 3. Evidence Summary
- Total evidence artifacts generated
- Coverage by practice group
- Gaps requiring attention

### 4. Recommendations
- Priority improvements
- Process enhancements
- Training needs

## Output

Generate report in `.claude/ssdf/reports/YYYY-MM-DD-compliance-report.md`
