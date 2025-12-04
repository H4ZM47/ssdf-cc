---
name: ssdf-build-hardening
description: Use to analyze build configurations for security hardening features per SSDF PW.6. Validates compiler flags and exploit mitigations. Dispatches build-engineer and security-engineer agents.
---

# SSDF Build Hardening Analysis (PW.6)

## Overview

Analyze compiler flags, build configurations, and security hardening features. Ensures builds include modern exploit mitigations.

**SSDF References:**
- PW.6.1 (Use Hardening Options for Compilation and Linking)
- PW.6.2 (Minimize Attack Surface by Removal)

## When to Use

**Required before:**
- Release builds
- Production deployments
- Security audits

**Recommended for:**
- Build system changes
- Compiler/toolchain upgrades
- New project setup

## Build Hardening Analysis Process

### Step 1: Dispatch Agents

Use Task tool with agent dispatch:

**Agent 1: build-engineer**
```
Analyze build configuration for:
[Repository path]

Tasks:
1. Detect build system (Make, CMake, Cargo, etc.)
2. Parse build files for compiler flags
3. Identify security-relevant settings
4. Check for debug symbols in release builds
5. Analyze CI/CD build configurations
```

**Agent 2: security-engineer**
```
Validate build security for:
[Repository path]

Tasks:
1. Verify stack protection enabled
2. Check for ASLR/PIE support
3. Validate RELRO settings
4. Check for dangerous flags
5. Analyze binary security (if binaries exist)
```

### Step 2: Build System Detection

**Detect build system from files:**

| Build System | Detection Files |
|--------------|-----------------|
| Make | `Makefile`, `GNUmakefile` |
| CMake | `CMakeLists.txt` |
| Autotools | `configure.ac`, `configure` |
| Meson | `meson.build` |
| Cargo (Rust) | `Cargo.toml` |
| Go | `go.mod`, `Makefile` with `go build` |
| Maven | `pom.xml` |
| Gradle | `build.gradle`, `build.gradle.kts` |
| MSBuild | `*.csproj`, `*.sln` |
| npm/Node | `package.json` with build scripts |

**Detection command:**
```bash
# Check for build system indicators
ls Makefile CMakeLists.txt Cargo.toml go.mod pom.xml build.gradle *.csproj 2>/dev/null
```

### Step 3: Security Flag Analysis

#### C/C++ Compiler Flags

**Required security flags (GCC/Clang):**

| Flag | Purpose | Severity |
|------|---------|----------|
| `-fstack-protector-strong` | Stack buffer overflow protection | Critical |
| `-fstack-clash-protection` | Stack clash attack protection | High |
| `-D_FORTIFY_SOURCE=2` | Runtime buffer overflow checks | High |
| `-fPIE` / `-pie` | Position Independent Executable | High |
| `-Wl,-z,relro` | Partial RELRO | High |
| `-Wl,-z,now` | Full RELRO (immediate binding) | High |
| `-Wl,-z,noexecstack` | Non-executable stack | Medium |
| `-fcf-protection` | Control-flow integrity (Intel CET) | Medium |

**Check Makefile/CMake:**
```bash
# Extract CFLAGS/CXXFLAGS from Makefile
grep -E "^C(XX)?FLAGS|LDFLAGS" Makefile 2>/dev/null

# Extract from CMakeLists.txt
grep -E "CMAKE_C.*_FLAGS|add_compile_options|target_compile_options" CMakeLists.txt 2>/dev/null
```

**Forbidden flags:**

| Flag | Reason | Severity |
|------|--------|----------|
| `-fno-stack-protector` | Disables stack protection | Critical |
| `-z execstack` | Allows executable stack | Critical |
| `-D_FORTIFY_SOURCE=0` | Disables fortify | High |
| `-fno-PIE` | Disables ASLR | High |
| `-O0` (in release) | No optimization | Medium |
| `-g` (in release) | Debug symbols | Low |

#### Rust Security Settings

**Cargo.toml profile settings:**
```toml
[profile.release]
overflow-checks = true      # Check integer overflow
lto = true                  # Link-time optimization
panic = "abort"             # No unwinding, smaller binary
strip = "symbols"           # Strip debug symbols
codegen-units = 1           # Better optimization
```

**RUSTFLAGS for hardening:**
```bash
# Check for security rustflags
RUSTFLAGS="-C target-feature=+cet"  # Intel CET
RUSTFLAGS="-C link-arg=-Wl,-z,relro,-z,now"  # RELRO
```

**Check Cargo.toml:**
```bash
# Look for security settings
grep -A 10 "\[profile.release\]" Cargo.toml 2>/dev/null
```

#### Go Build Flags

**Required settings:**
```bash
# Secure build command
go build -trimpath -ldflags="-s -w -buildmode=pie"

# Flags explained:
# -trimpath     : Remove file paths (reproducibility)
# -s            : Strip symbol table
# -w            : Strip DWARF debug info
# -buildmode=pie: Position independent executable
```

**Check Go build scripts:**
```bash
# Look for go build commands
grep -r "go build" Makefile .github/workflows/*.yml 2>/dev/null
```

#### Java/JVM Security

**Maven pom.xml checks:**
```xml
<!-- Compiler plugin settings -->
<plugin>
  <artifactId>maven-compiler-plugin</artifactId>
  <configuration>
    <release>17</release>  <!-- Use recent Java version -->
    <compilerArgs>
      <arg>-Xlint:all</arg>  <!-- Enable warnings -->
    </compilerArgs>
  </configuration>
</plugin>
```

#### .NET Security

**Project file checks:**
```xml
<PropertyGroup>
  <PublishTrimmed>true</PublishTrimmed>
  <TrimMode>link</TrimMode>
  <DebugType>none</DebugType>  <!-- No debug in release -->
  <DebugSymbols>false</DebugSymbols>
</PropertyGroup>
```

### Step 4: Binary Analysis

**If compiled binaries exist, analyze with checksec:**
```bash
# Install checksec
# apt-get install checksec OR brew install checksec

# Analyze binary
checksec --file=./target/release/binary

# Expected output for hardened binary:
# RELRO           STACK CANARY      NX            PIE
# Full RELRO      Canary found      NX enabled    PIE enabled
```

**Manual checks with readelf:**
```bash
# Check for executable stack
readelf -l binary | grep -E "GNU_STACK.*RWE"  # Should not match

# Check for RELRO
readelf -l binary | grep GNU_RELRO  # Should exist

# Check for PIE
readelf -h binary | grep "DYN"  # Should be DYN for PIE

# Check for stack canary
readelf -s binary | grep __stack_chk  # Should exist
```

### Step 5: CI/CD Pipeline Analysis

**GitHub Actions:**
```yaml
# Check .github/workflows/*.yml for build flags
- name: Build
  run: |
    make CFLAGS="-fstack-protector-strong -D_FORTIFY_SOURCE=2"
```

**GitLab CI:**
```yaml
# Check .gitlab-ci.yml
build:
  script:
    - export CFLAGS="-fstack-protector-strong"
    - make
```

**Dockerfile:**
```dockerfile
# Check for hardening in Docker builds
RUN CFLAGS="-fstack-protector-strong" make
```

### Step 6: Reproducible Build Checks

**Verify reproducibility settings:**
```bash
# Check for SOURCE_DATE_EPOCH usage
grep -r "SOURCE_DATE_EPOCH" Makefile .github/workflows/ Dockerfile 2>/dev/null

# Check for timestamp elimination
grep -r "BUILD_TIME\|__DATE__\|__TIME__" src/ 2>/dev/null  # Should not exist
```

**Reproducibility checklist:**
- [ ] No embedded timestamps
- [ ] Deterministic file ordering
- [ ] SOURCE_DATE_EPOCH honored
- [ ] No randomized hash seeds in build

### Step 7: Generate Evidence Report

Create evidence file:
```
.claude/ssdf/evidence/build/YYYY-MM-DD-build-hardening.md
```

**Report template:**
```markdown
# Build Hardening Report

**Date:** YYYY-MM-DD
**Project:** [project name]
**Language:** [detected language]
**Build System:** [detected build system]
**Reviewer:** Claude Code (SSDF)
**SSDF Reference:** PW.6.1, PW.6.2

## Executive Summary

| Protection | Status | Details |
|------------|--------|---------|
| Stack Protection | PASS/FAIL | -fstack-protector-strong |
| ASLR (PIE) | PASS/FAIL | -fPIE -pie |
| RELRO | PASS/FAIL | Full/Partial/None |
| Fortify Source | PASS/FAIL | -D_FORTIFY_SOURCE=2 |
| Non-exec Stack | PASS/FAIL | -Wl,-z,noexecstack |
| CFI | PASS/FAIL | -fcf-protection |

## Build Configuration Analysis

### Build System: [system name]

**Detected Flags:**
```
CFLAGS = [current flags]
CXXFLAGS = [current flags]
LDFLAGS = [current flags]
```

### Security Flags Status

| Category | Required | Current | Gap |
|----------|----------|---------|-----|
| Stack Protection | -fstack-protector-strong | [found/missing] | |
| Fortify | -D_FORTIFY_SOURCE=2 | [found/missing] | |
| PIE | -fPIE -pie | [found/missing] | |
| RELRO | -Wl,-z,relro,-z,now | [found/missing] | |

### Dangerous Flags Found

| Flag | Location | Severity | Recommendation |
|------|----------|----------|----------------|

## Binary Analysis

### checksec Output
```
[checksec output if binaries analyzed]
```

### Security Features
| Feature | Status |
|---------|--------|
| RELRO | Full/Partial/None |
| Stack Canary | Found/Not Found |
| NX | Enabled/Disabled |
| PIE | Enabled/Disabled |
| RPATH | None/Present |
| RUNPATH | None/Present |

## CI/CD Pipeline Analysis

### GitHub Actions
| Workflow | Security Flags | Status |
|----------|---------------|--------|

### Docker Build
| Stage | Security Flags | Status |
|-------|---------------|--------|

## Reproducibility Assessment

| Check | Status | Notes |
|-------|--------|-------|
| No timestamps | PASS/FAIL | |
| SOURCE_DATE_EPOCH | PASS/FAIL | |
| Deterministic ordering | PASS/FAIL | |

## Recommendations

### Critical (Must Fix)
1. [Add missing critical security flags]

### High Priority
1. [Add missing high-priority flags]

### Medium Priority
1. [Optimization recommendations]

## Remediation

### For Makefile
```makefile
CFLAGS += -fstack-protector-strong -D_FORTIFY_SOURCE=2 -fPIE
CXXFLAGS += -fstack-protector-strong -D_FORTIFY_SOURCE=2 -fPIE
LDFLAGS += -pie -Wl,-z,relro,-z,now -Wl,-z,noexecstack
```

### For CMake
```cmake
add_compile_options(
  -fstack-protector-strong
  -D_FORTIFY_SOURCE=2
  -fPIE
)
add_link_options(
  -pie
  -Wl,-z,relro,-z,now
  -Wl,-z,noexecstack
)
```

### For Cargo.toml
```toml
[profile.release]
overflow-checks = true
lto = true
panic = "abort"
strip = "symbols"
```

## Sign-Off

- [ ] All critical security flags present
- [ ] No dangerous flags found
- [ ] Binaries pass checksec analysis
- [ ] CI/CD uses secure build flags
- [ ] Reproducible build settings configured
```

### Step 8: Remediation (--fix flag)

If `--fix` is specified, apply fixes:

**For Makefile:**
```bash
# Add security flags to Makefile
cat >> Makefile << 'EOF'

# SSDF PW.6 Security Hardening Flags
SECURITY_CFLAGS = -fstack-protector-strong -D_FORTIFY_SOURCE=2 -fPIE -fcf-protection
SECURITY_LDFLAGS = -pie -Wl,-z,relro,-z,now -Wl,-z,noexecstack
CFLAGS += $(SECURITY_CFLAGS)
CXXFLAGS += $(SECURITY_CFLAGS)
LDFLAGS += $(SECURITY_LDFLAGS)
EOF
```

**For CMakeLists.txt:**
```cmake
# Add to CMakeLists.txt
if(CMAKE_BUILD_TYPE STREQUAL "Release")
  add_compile_options(
    -fstack-protector-strong
    -D_FORTIFY_SOURCE=2
    -fPIE
  )
  add_link_options(
    -pie
    -Wl,-z,relro,-z,now
  )
endif()
```

## Integration

**Create issue for build problems:**
```bash
bd create "Security: [Build Hardening Issue]" --type security -d "Found during SSDF PW.6 build hardening check. See evidence: .claude/ssdf/evidence/build/[date].md"
```

## Commit Message Format

```
[SSDF:PW.6.1] Enable build security hardening flags

- Added stack protection flags
- Enabled RELRO and PIE
- Added FORTIFY_SOURCE
- Evidence: .claude/ssdf/evidence/build/YYYY-MM-DD.md
```

## Compliance Thresholds

| Protection | Required | Notes |
|------------|----------|-------|
| Stack Protection | Yes | Critical for C/C++ |
| ASLR (PIE) | Yes | Required for all binaries |
| RELRO | Full preferred | At minimum Partial |
| Fortify Source | Yes | Level 2 for production |
| Non-exec Stack | Yes | Standard protection |

## Common Issues and Fixes

**Issue: Missing -fPIE causes ASLR to be ineffective**
```makefile
# Add to CFLAGS
CFLAGS += -fPIE
LDFLAGS += -pie
```

**Issue: Partial RELRO instead of Full**
```makefile
# Change from -Wl,-z,relro to:
LDFLAGS += -Wl,-z,relro,-z,now
```

**Issue: Debug symbols in release build**
```makefile
# For release builds, strip symbols
LDFLAGS += -s
# Or use strip command
strip --strip-all binary
```

**Issue: Rust not stripping in release**
```toml
# Cargo.toml
[profile.release]
strip = "symbols"
```
