---
description: Analyze build configurations for security hardening (PW.6)
arguments:
  - name: language
    description: Language to check - auto, c, cpp, rust, go (default: auto)
    required: false
  - name: fix
    description: Add missing security flags to build config (default: false)
    required: false
---

# SSDF Build Hardening Analysis

Use the `ssdf-build-hardening` skill to verify build security settings.

**Language:** $ARGUMENTS.language (or auto-detect if not specified)
**Fix:** $ARGUMENTS.fix (or false if not specified)

## Process

1. Dispatch agents:
   - `build-engineer`: Build system analysis and flag detection
   - `security-engineer`: Security flag validation

2. Detect build system and language
3. Analyze compiler flags and linker settings
4. Verify security hardening features enabled
5. Check CI/CD pipeline configurations
6. Generate evidence file in `.claude/ssdf/evidence/build/`

## Output Requirements

- Build hardening report with flag analysis
- Binary analysis results (if binaries present)
- --fix adds security flags to build configuration
- SSDF references (PW.6.1, PW.6.2) in commit messages
