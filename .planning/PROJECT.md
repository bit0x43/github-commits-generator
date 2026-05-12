# GitHub Activity Generator

## What This Is

A utility that generates synthetic commit history to populate a GitHub contributions graph. Supports two modes: backfill (one-time bulk generation of historical commits) and daily (one commit per day ongoing).

## Core Value

Generate believable GitHub activity without requiring actual development work. Useful for making a profile look active without spending time on trivial commits.

## Requirements

### Active

- [ ] CI pipeline fixes (build.yml, main.yml)
- [ ] Python 3.12 LTS upgrade
- [ ] Refactor: backfill mode + daily workflow
- [ ] Documentation refresh

### Out of Scope

- [Real-time commits] — Not needed; scheduled/daily is sufficient
- [Multiple commits per day] — One per day is enough for the use case
- [GitHub API mode] — Keep git-based approach for simplicity; only explore if performance is poor

## Context

**Brownfield project:**
- Existing codebase: `contribute.py` (128 lines), `test_contribute.py` (32 lines)
- CI workflows: `.github/workflows/main.yml` (daily), `build.yml` (broken)
- No external dependencies — pure Python standard library

**Current issues:**
- build.yml uses Python 2.7, 3.8 (EOL), outdated actions
- No mode separation — script does everything in one run
- No daily workflow for ongoing commits

## Constraints

- **Stack**: Python 3.12+ (keep existing Python-only approach)
- **No dependencies**: Continue using standard library only
- **Git-based**: Use git CLI for commits (simpler than API)

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Keep git-based approach | Simpler, no API token management | — Pending |
| Daily workflow over API | Reliable, one line append per day | — Pending |
| Backfill for historical, daily for ongoing | Clear separation of concerns | — Pending |

## Evolution

This document evolves at phase transitions and milestone boundaries.

**After each phase transition** (via `/gsd-transition`):
1. Requirements invalidated? → Move to Out of Scope with reason
2. Requirements validated? → Move to Validated with phase reference
3. New requirements emerged? → Add to Active
4. Decisions to log? → Add to Key Decisions
5. "What This Is" still accurate? → Update if drifted

**After each milestone** (via `/gsd-complete-milestone`):
1. Full review of all sections
2. Core Value check — still the right priority?
3. Audit Out of Scope — reasons still valid?
4. Update Context with current state

---

## Current Milestone: v1.0 Pipeline Fixes & Modernization

**Goal:** Fix broken CI pipelines, upgrade to Python 3.12 LTS, and refactor core mechanism into backfill + daily modes.

**Target features:**
- CI pipeline fixes (build.yml, main.yml)
- Python 3.12 modernization
- Refactor: backfill mode + daily workflow
- Documentation refresh

*Last updated: 2026-05-12 after milestone start*
