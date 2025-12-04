---
name: ssdf-env-audit
description: Use to audit development and deployment environment security per SSDF PO.5. Checks infrastructure, access controls, network segmentation. Dispatches platform-engineer and security-engineer agents.
---

# SSDF Environment Audit (PO.5)

## Overview

Audit the security of development, staging, and production environments including infrastructure configuration, access controls, and network segmentation.

**SSDF References:**
- PO.5.1 (Separate Development and Build Environments)
- PO.5.2 (Secure Development and Build Environments)

## When to Use

**Required for:**
- New environment setup
- Environment changes
- Compliance audits
- Security assessments

**Recommended for:**
- Quarterly reviews
- After infrastructure changes
- Cloud migration planning

## Environment Audit Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: platform-engineer**
```
Audit environment infrastructure for:
[Environment type/path]

Tasks:
1. Inventory infrastructure components
2. Check configuration management
3. Verify environment isolation
4. Assess scalability and resilience
5. Review IaC templates
```

**Agent 2: security-engineer**
```
Security assessment for environment:
[Environment type/path]

Tasks:
1. Audit access controls
2. Check network segmentation
3. Review secrets management
4. Assess logging and monitoring
5. Verify compliance controls
```

### Step 2: Environment Inventory

**Infrastructure components to audit:**

| Category | Components |
|----------|------------|
| Compute | VMs, containers, serverless |
| Storage | Databases, object storage, file systems |
| Network | VPCs, subnets, load balancers |
| Identity | IAM, service accounts, roles |
| Secrets | Vaults, KMS, certificates |
| Monitoring | Logging, metrics, alerts |

**Detection commands:**
```bash
# AWS
aws ec2 describe-instances
aws rds describe-db-instances
aws s3 ls

# Kubernetes
kubectl get nodes
kubectl get namespaces
kubectl get pods --all-namespaces

# Docker
docker ps -a
docker network ls
docker volume ls
```

### Step 3: Access Control Audit

**IAM best practices:**
- Least privilege principle
- No shared credentials
- MFA enforced
- Regular access review
- Service account isolation

**Check patterns:**
```bash
# AWS IAM
aws iam list-users
aws iam get-account-authorization-details

# Kubernetes RBAC
kubectl get clusterroles
kubectl get rolebindings --all-namespaces
```

**Access control matrix:**
| Principal | Environment | Access Level | MFA | Last Used |
|-----------|-------------|--------------|-----|-----------|
| [user] | Production | Admin | Yes | [date] |
| [service] | Staging | Read | N/A | [date] |

### Step 4: Network Segmentation

**Verify isolation between:**
- Development ↔ Production
- Internal ↔ External
- Application tiers
- Tenant boundaries

**Network checks:**
```bash
# AWS VPC
aws ec2 describe-vpcs
aws ec2 describe-security-groups

# Kubernetes Network Policies
kubectl get networkpolicies --all-namespaces
```

**Expected controls:**
| From | To | Allowed | Protocol | Ports |
|------|----|---------||---------|-------|
| Dev | Prod | No | - | - |
| App | DB | Yes | TCP | 5432 |

### Step 5: Secrets Management

**Check for:**
- Hardcoded secrets in code/configs
- Secrets in environment variables
- Unencrypted secrets storage
- Secret rotation policies

**Detection:**
```bash
# Find potential secrets in files
grep -rn "password\|secret\|api_key\|token" --include="*.env" --include="*.yaml"

# Check for unencrypted secrets in K8s
kubectl get secrets -o yaml | grep -v "ca.crt\|tls"
```

### Step 6: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/environment/YYYY-MM-DD-env-audit.md
```

**Report template:**
```markdown
# Environment Security Audit Report

**Date:** YYYY-MM-DD
**Environment:** [dev/staging/prod/all]
**Project:** [project name]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PO.5.1, PO.5.2

## Executive Summary

| Environment | Components | Issues | Status |
|-------------|------------|--------|--------|
| Development | X | Y | PASS/WARN/FAIL |
| Staging | X | Y | PASS/WARN/FAIL |
| Production | X | Y | PASS/WARN/FAIL |

## Infrastructure Inventory

### Development
| Component | Type | Status | Notes |
|-----------|------|--------|-------|

### Staging
| Component | Type | Status | Notes |
|-----------|------|--------|-------|

### Production
| Component | Type | Status | Notes |
|-----------|------|--------|-------|

## Environment Isolation

| Check | Dev→Staging | Staging→Prod | Status |
|-------|-------------|--------------|--------|
| Network isolated | Yes/No | Yes/No | |
| Credentials separate | Yes/No | Yes/No | |
| Data isolated | Yes/No | Yes/No | |

## Access Control Audit

### Users
| User | Environments | Roles | MFA | Status |
|------|--------------|-------|-----|--------|

### Service Accounts
| Account | Environment | Permissions | Last Rotated |
|---------|-------------|-------------|--------------|

### Issues Found
| Issue | Severity | Affected | Remediation |
|-------|----------|----------|-------------|

## Network Segmentation

### VPC/Network Configuration
| Network | CIDR | Environment | Egress |
|---------|------|-------------|--------|

### Security Groups/Firewall Rules
| Rule | Source | Destination | Ports | Status |
|------|--------|-------------|-------|--------|

### Network Policies
| Policy | Namespace | Effect | Status |
|--------|-----------|--------|--------|

## Secrets Management

### Findings
| Location | Issue | Severity | Remediation |
|----------|-------|----------|-------------|

### Recommendations
- [ ] Use secrets manager
- [ ] Implement rotation
- [ ] Remove hardcoded secrets

## Logging and Monitoring

| Check | Status | Notes |
|-------|--------|-------|
| Audit logging enabled | | |
| Log retention adequate | | |
| Alerts configured | | |
| SIEM integration | | |

## Compliance Checklist

- [ ] Environments properly isolated
- [ ] Least privilege enforced
- [ ] No shared credentials
- [ ] Network segmentation complete
- [ ] Secrets properly managed
- [ ] Logging enabled
- [ ] MFA enforced for production

## Recommendations

### Critical
1. [Critical findings]

### High
1. [High priority items]

### Medium
1. [Medium priority items]

## Sign-Off

- [ ] All environments audited
- [ ] Critical issues addressed
- [ ] Evidence archived
```

## Integration

**Create issue for findings:**
```bash
bd create "Security: [Environment Issue]" --type security -d "Found during SSDF PO.5 environment audit. See evidence: .claude/ssdf/evidence/environment/[date].md"
```

## Commit Message Format

```
[SSDF:PO.5.1] Improve environment security

- Fixed network segmentation gaps
- Implemented secrets rotation
- Evidence: .claude/ssdf/evidence/environment/YYYY-MM-DD.md
```
