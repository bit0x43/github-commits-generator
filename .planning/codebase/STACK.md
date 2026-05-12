# Stack

**Date:** 2026-05-12

## Languages & Runtime

- **Python:** 3.x (no specific version pinned; CI uses Python 3.11)
- **Shell:** Bash (GitHub Actions)

## Frameworks & Libraries

- **Standard Library Only** — no external dependencies
  - `argparse` — CLI argument parsing
  - `subprocess` — git command execution
  - `datetime`, `random`, `os`, `sys` — core utilities

## Build & Tooling

- **GitHub Actions** — CI/CD automation
- **Git** — version control (invoked via subprocess)

## Configuration

- No configuration files (`.json`, `.yaml`, `.toml`, etc.)
- Runtime options passed via CLI arguments
- Environment variables for CI (secrets in GitHub Actions)

## Dependencies

None — pure Python script with no pip/poetry/conda requirements.

---

**Related:**
- `contribute.py` — Main entry point (128 lines)
- `.github/workflows/main.yml` — GitHub Actions workflow
