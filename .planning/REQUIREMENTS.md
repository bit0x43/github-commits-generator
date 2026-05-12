# Requirements: GitHub Activity Generator

**Defined:** 2026-05-12
**Core Value:** Generate believable GitHub activity without requiring actual development work.

## v1.0 Requirements

### CI/CD

- [ ] **CI-01**: Fix `.github/workflows/build.yml` — update Python versions from [2.7, 3.8] to [3.12], upgrade actions to v4, remove/update flake8 lint
- [ ] **CI-02**: Fix `.github/workflows/main.yml` — verify daily contribute workflow works with current config

### Modernization

- [ ] **MOD-01**: Update shebang in `contribute.py` to `#!/usr/bin/env python3`
- [ ] **MOD-02**: Update CI workflows to use Python 3.12 LTS
- [ ] **MOD-03**: Remove Python 2 references from `.github/README.md` (system requirements)

### Refactor

- [ ] **REF-01**: Backfill mode — refactor `contribute.py` to support `--mode backfill` for one-time bulk commit generation
- [ ] **REF-02**: Daily workflow — create `.github/workflows/daily.yml` for ongoing one-commit-per-day
- [ ] **REF-03**: Unified CLI — add `--mode` flag to `contribute.py` with values `backfill` (default) and `daily`

### Performance

- [ ] **PERF-01**: Evaluate optimization approaches — compare parallel git subprocesses vs GitHub API direct commits
- [ ] **PERF-02**: Implement optimization if backfill is slow (after measuring)

### Documentation

- [ ] **DOCS-01**: Refresh `.github/README.md` — document new architecture (backfill + daily modes), Python 3.12+ requirement

## v2 Requirements

### Features

- **AUTO-01**: Add auto-detection mode that checks if backfill is needed vs daily is enough
- **SCHED-01**: Allow customizable schedule for daily mode (not just 13:00 UTC)

## Out of Scope

| Feature | Reason |
|---------|--------|
| GitHub API mode | Extra complexity not needed; git-based is sufficient |
| Multiple commits per day | One per day is enough for the use case |
| Real-time commits | Scheduled/daily is more reliable |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| CI-01 | Phase 1 | Pending |
| CI-02 | Phase 1 | Pending |
| MOD-01 | Phase 2 | Pending |
| MOD-02 | Phase 2 | Pending |
| MOD-03 | Phase 2 | Pending |
| REF-01 | Phase 3 | Pending |
| REF-02 | Phase 3 | Pending |
| REF-03 | Phase 3 | Pending |
| PERF-01 | Phase 4 | Pending |
| PERF-02 | Phase 4 | Pending |
| DOCS-01 | Phase 5 | Pending |

**Coverage:**
- v1.0 requirements: 11 total
- Mapped to phases: 11
- Unmapped: 0 ✓

---
*Requirements defined: 2026-05-12*
*Last updated: 2026-05-12 after milestone v1.0 definition*