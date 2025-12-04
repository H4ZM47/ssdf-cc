---
name: ssdf-secure-defaults
description: Use to validate application configuration against secure defaults per SSDF PW.9. Detects insecure settings that could expose vulnerabilities. Dispatches security-auditor and framework-specific agents.
---

# SSDF Secure Defaults Validation (PW.9)

## Overview

Validate application configuration against secure default baselines. Detects insecure settings before they reach production.

**SSDF References:**
- PW.9.1 (Configure Compilation, Build to Improve Executable Security)
- PW.9.2 (Review and Configure Default Settings)

## When to Use

**Required before:**
- Deployment to any environment
- Configuration changes to production
- Security audits

**Recommended for:**
- New project setup
- Framework upgrades
- After adding new dependencies

## Secure Defaults Validation Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: security-auditor**
```
Validate configuration security for:
[Repository path]

Check:
1. Authentication and session settings
2. Encryption and TLS configuration
3. Debug and logging settings
4. CORS and security headers
5. Secrets management
6. Docker/Kubernetes security
```

**Agent 2: Framework-specific agent**
Based on detected framework, dispatch one of:
- `typescript-pro` for Express.js/Node.js
- `django-developer` for Django
- `spring-boot-engineer` for Spring Boot
- `rails-expert` for Rails
- `laravel-specialist` for Laravel

### Step 2: Framework Detection

**Auto-detect from project files:**

| Framework | Detection Files |
|-----------|-----------------|
| Express.js | `package.json` with express, `app.js` |
| Django | `manage.py`, `settings.py` |
| Spring Boot | `pom.xml` with spring-boot, `application.properties` |
| Rails | `Gemfile` with rails, `config/application.rb` |
| Laravel | `composer.json` with laravel, `artisan` |
| FastAPI | `main.py` with FastAPI, `requirements.txt` |
| Flask | `app.py` with Flask |
| ASP.NET | `*.csproj`, `appsettings.json` |

**Detection command:**
```bash
# Check for framework indicators
ls package.json manage.py pom.xml Gemfile composer.json *.csproj 2>/dev/null
```

### Step 3: Configuration File Discovery

**Files to scan:**

```bash
# Environment files
find . -name ".env*" -type f 2>/dev/null

# Configuration directories
find . -path "*/config/*" -type f 2>/dev/null

# Framework-specific configs
ls application.properties application.yml settings.py \
   config.py web.config appsettings.json 2>/dev/null

# Container configs
find . \( -name "Dockerfile*" -o -name "docker-compose*.yml" \
          -o -name "*.yaml" -path "*/k8s/*" \
          -o -name "*.yaml" -path "*/manifests/*" \) 2>/dev/null
```

### Step 4: Security Baseline Checks

#### Authentication & Sessions

| Setting | Secure Value | Severity |
|---------|--------------|----------|
| Session timeout | <= 3600s (1 hour) | High |
| Cookie Secure | true | Critical |
| Cookie HttpOnly | true | High |
| Cookie SameSite | Strict/Lax | Medium |
| Password min length | >= 12 | High |
| MFA enforcement | true (recommended) | Medium |
| Session fixation protection | enabled | High |

**Check patterns:**
```bash
# Look for insecure session settings
grep -r "session.*timeout\|SESSION_COOKIE_AGE" --include="*.py" --include="*.js" --include="*.yaml"
grep -r "cookie.*secure.*false\|COOKIE_SECURE.*False" --include="*.py" --include="*.js"
```

#### Encryption & TLS

| Setting | Secure Value | Severity |
|---------|--------------|----------|
| TLS minimum version | 1.2+ | Critical |
| Cipher suites | No DES/RC4/MD5 | Critical |
| HSTS enabled | true | High |
| HSTS max-age | >= 31536000 | Medium |
| Certificate validation | enabled | Critical |

**Check patterns:**
```bash
# Look for weak TLS
grep -r "TLSv1\b\|TLSv1\.0\|SSLv" --include="*.conf" --include="*.yaml" --include="*.properties"
grep -r "ssl_verify.*false\|VERIFY_SSL.*False" --include="*.py" --include="*.js"
```

#### Debug & Logging

| Setting | Production Value | Severity |
|---------|------------------|----------|
| Debug mode | false | Critical |
| Verbose errors | false | High |
| Stack traces | disabled | High |
| Log sensitive data | false | Critical |
| Log level | INFO or higher | Low |

**Check patterns:**
```bash
# Look for debug mode enabled
grep -r "DEBUG.*=.*[Tt]rue\|debug.*:.*true" --include="*.py" --include="*.js" --include="*.yaml"
grep -r "DJANGO_DEBUG\|NODE_ENV.*development" --include=".env*"
```

#### CORS & Headers

| Setting | Secure Value | Severity |
|---------|--------------|----------|
| CORS allow all (*) | false | High |
| X-Frame-Options | DENY/SAMEORIGIN | Medium |
| X-Content-Type-Options | nosniff | Medium |
| Content-Security-Policy | configured | Medium |
| X-XSS-Protection | 1; mode=block | Low |

**Check patterns:**
```bash
# Look for permissive CORS
grep -r "Access-Control-Allow-Origin.*\*\|cors.*origin.*\*\|CORS_ALLOW_ALL"
grep -r "X-Frame-Options\|CSP\|Content-Security-Policy"
```

### Step 5: Framework-Specific Checks

#### Express.js/Node.js
```javascript
// INSECURE: Missing helmet middleware
// app.use(helmet())  // Not present

// INSECURE: No rate limiting
// Missing express-rate-limit

// INSECURE: Trust proxy misconfigured
app.set('trust proxy', true)  // Should be specific IP

// Check for:
// - helmet() middleware
// - express-rate-limit
// - cors() configuration
// - cookie-parser secure settings
```

#### Django
```python
# settings.py security checklist
DEBUG = False                          # Must be False in production
ALLOWED_HOSTS = ['specific.domain']    # Must not be ['*']
CSRF_COOKIE_SECURE = True              # Must be True
SESSION_COOKIE_SECURE = True           # Must be True
SECURE_SSL_REDIRECT = True             # Recommended
SECURE_HSTS_SECONDS = 31536000         # Recommended
SECURE_CONTENT_TYPE_NOSNIFF = True     # Recommended
X_FRAME_OPTIONS = 'DENY'               # Recommended
```

#### Spring Boot
```properties
# application.properties security checklist
server.error.include-stacktrace=never
management.endpoints.web.exposure.include=health,info
spring.security.user.password=[random]  # Not default
server.servlet.session.timeout=30m      # Reasonable timeout
server.ssl.enabled-protocols=TLSv1.2,TLSv1.3
```

#### Rails
```ruby
# config/environments/production.rb
config.force_ssl = true
config.action_dispatch.cookies_same_site_protection = :strict
config.action_controller.default_url_options = { protocol: 'https' }

# config/application.rb
config.action_dispatch.default_headers = {
  'X-Frame-Options' => 'SAMEORIGIN',
  'X-Content-Type-Options' => 'nosniff'
}
```

### Step 6: Docker/Kubernetes Security

#### Dockerfile Checks
```dockerfile
# INSECURE patterns to detect:
USER root                    # Should use non-root user
FROM image:latest           # Should pin version
RUN chmod 777               # Too permissive
ENV SECRET=value            # Secrets in env
```

#### Kubernetes Checks
```yaml
# INSECURE patterns to detect:
securityContext:
  privileged: true          # Should be false
  runAsRoot: true           # Should be false
  allowPrivilegeEscalation: true  # Should be false

# MISSING patterns to flag:
# - Resource limits not defined
# - No readOnlyRootFilesystem
# - No seccomp profile
```

**Security baseline:**
```yaml
# Secure pod spec template
securityContext:
  runAsNonRoot: true
  readOnlyRootFilesystem: true
  allowPrivilegeEscalation: false
  capabilities:
    drop: ["ALL"]
resources:
  limits:
    cpu: "1"
    memory: "512Mi"
```

### Step 7: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/config/YYYY-MM-DD-secure-defaults.md
```

**Report template:**
```markdown
# Secure Defaults Validation Report

**Date:** YYYY-MM-DD
**Project:** [project name]
**Framework:** [detected framework]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PW.9.1, PW.9.2

## Executive Summary

| Category | Issues | Critical | High | Medium | Low |
|----------|--------|----------|------|--------|-----|
| Authentication | X | | | | |
| Encryption | X | | | | |
| Debug/Logging | X | | | | |
| Headers | X | | | | |
| Docker/K8s | X | | | | |
| **Total** | X | Y | Z | A | B |

## Files Scanned

| File | Type | Issues Found |
|------|------|--------------|
| .env.production | Environment | X |
| settings.py | Django Config | X |
| docker-compose.yml | Container | X |

## Critical Issues

| Issue | Location | Current Value | Secure Value |
|-------|----------|---------------|--------------|
| Debug mode enabled | settings.py:12 | True | False |
| TLS 1.0 allowed | nginx.conf:45 | TLSv1 | TLSv1.2+ |

## High Severity Issues

| Issue | Location | Current Value | Secure Value |
|-------|----------|---------------|--------------|

## Medium Severity Issues

| Issue | Location | Current Value | Secure Value |
|-------|----------|---------------|--------------|

## Low Severity Issues

| Issue | Location | Current Value | Secure Value |
|-------|----------|---------------|--------------|

## Remediation

### Critical Fixes (Immediate)

**1. Disable debug mode in production**
```python
# settings.py - Line 12
# Before:
DEBUG = True

# After:
DEBUG = os.environ.get('DEBUG', 'False') == 'True'
```

**2. Upgrade TLS minimum version**
```nginx
# nginx.conf - Line 45
ssl_protocols TLSv1.2 TLSv1.3;
```

### High Priority Fixes

[Additional remediation steps...]

## Framework-Specific Recommendations

### [Framework Name]

1. [Recommendation 1]
2. [Recommendation 2]

## Container Security Recommendations

1. Use non-root user in Dockerfile
2. Pin base image versions
3. Add resource limits to Kubernetes specs

## Validation Checklist

- [ ] All critical issues resolved
- [ ] Debug mode disabled in production configs
- [ ] TLS properly configured
- [ ] Security headers present
- [ ] Container security baseline met
- [ ] Secrets not in configuration files

## Sign-Off

- [ ] No critical issues remain
- [ ] High severity issues documented with remediation plan
- [ ] Configuration ready for production
```

### Step 8: Remediation (--fix flag)

If `--fix` is specified, generate patches:

**Generate secure config patches:**
```bash
# Create patch files for each insecure setting
# Output to: .claude/ssdf/patches/secure-defaults/

# Example Django fix
cat > patches/settings.py.patch << 'EOF'
--- a/settings.py
+++ b/settings.py
@@ -10,7 +10,7 @@
-DEBUG = True
+DEBUG = os.environ.get('DEBUG', 'False') == 'True'

-ALLOWED_HOSTS = ['*']
+ALLOWED_HOSTS = os.environ.get('ALLOWED_HOSTS', '').split(',')

+# Security settings for production
+CSRF_COOKIE_SECURE = True
+SESSION_COOKIE_SECURE = True
+SECURE_SSL_REDIRECT = True
EOF
```

## Integration

**Create issue for configuration problems:**
```bash
bd create "Security: [Config Issue]" --type security -d "Found during SSDF PW.9 secure defaults check. See evidence: .claude/ssdf/evidence/config/[date].md"
```

## Commit Message Format

```
[SSDF:PW.9.1] Fix insecure configuration defaults

- Disabled debug mode in production
- Configured secure session cookies
- Added security headers
- Evidence: .claude/ssdf/evidence/config/YYYY-MM-DD.md
```

## Compliance Thresholds

| Severity | Maximum Allowed | Action |
|----------|-----------------|--------|
| Critical | 0 | Must fix before deploy |
| High | 0 for production | Must fix or document exception |
| Medium | Unlimited | Should fix within sprint |
| Low | Unlimited | Nice to have |

## Common Issues and Fixes

**Issue: Debug mode varies by environment**
```python
# Use environment variable
DEBUG = os.environ.get('DEBUG', 'False').lower() == 'true'
```

**Issue: Secrets in configuration files**
```bash
# Move to environment variables or secret manager
# Replace hardcoded values with:
SECRET_KEY = os.environ['SECRET_KEY']
```

**Issue: Missing security headers**
```nginx
# Add to nginx.conf
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
```
