# Roadmap: GitHub Activity Generator

**Milestone:** v1.0 Pipeline Fixes & Modernization
**Total Phases:** 5
**Total Requirements:** 11

---

## Phase 1: CI Pipeline Fixes

**Goal:** Restore and modernize broken build pipelines

**Requirements:** CI-01, CI-02

**Success Criteria:**
1. `build.yml` passes CI with Python 3.12
2. `main.yml` workflow runs without errors
3. Lint passes (flake8 or replaced with ruff)
4. Tests pass (unittest)

---

## Phase 2: Python Modernization

**Goal:** Upgrade to Python 3.12 LTS, remove legacy Python 2 support

**Requirements:** MOD-01, MOD-02, MOD-03

**Success Criteria:**
1. `contribute.py` shebang updated to `python3`
2. All CI workflows use Python 3.12
3. `.github/README.md` shows Python 3.12+ requirement
4. No Python 2 references anywhere in project

---

## Phase 3: Refactor Core — Two Modes

**Goal:** Split functionality into backfill + daily modes for clear separation of concerns

**Requirements:** REF-01, REF-02, REF-03

**Success Criteria:**
1. `contribute.py --mode backfill` generates historical commits (tested with small date range)
2. `.github/workflows/daily.yml` created and runs on schedule
3. `contribute.py --mode daily` creates single commit for today
4. `contribute.py --help` documents both modes

---

## Phase 4: Performance Optimization

**Goal:** Evaluate and implement optimization for backfill mode

**Requirements:** PERF-01, PERF-02

**Success Criteria:**
1. Performance evaluation documented (parallel git vs API approach)
2. If needed, optimization implemented and benchmarked
3. Backfill of 365 days completes in reasonable time (< 5 minutes)

---

## Phase 5: Documentation Refresh

**Goal:** Update README with new architecture and features

**Requirements:** DOCS-01

**Success Criteria:**
1. `.github/README.md` documents both modes
2. Python 3.12+ requirement clearly stated
3. Usage examples for both backfill and daily modes
4. Includes troubleshooting section

---

## Summary

| Phase | Name | Requirements |
|-------|------|--------------|
| 1 | CI Pipeline Fixes | CI-01, CI-02 |
| 2 | Python Modernization | MOD-01, MOD-02, MOD-03 |
| 3 | Refactor Core | REF-01, REF-02, REF-03 |
| 4 | Performance | PERF-01, PERF-02 |
| 5 | Documentation | DOCS-01 |

---
*Roadmap created: 2026-05-12 for milestone v1.0*
