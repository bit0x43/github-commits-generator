# Integrations

**Date:** 2026-05-12

## External Services

### GitHub
- **Authentication:** GitHub Personal Access Token (PAT) via `ACTIONS_GITHUB_TOKEN`
- **Remote Operations:** `git push` to remote repository
- **Contribution Tracking:** Script generates commits that appear on GitHub profile

## Databases

None — no database connectivity.

## Auth Providers

- **GitHub Actions Secrets:** `TARGET_USER`, `TARGET_EMAIL`, `TARGET_REPO`
- **Git Config:** User identity set via `git config user.name` / `user.email`

## Webhooks

None.

## API Integrations

- **Git Push:** Direct git protocol (SSH or HTTPS)
- No REST API calls to GitHub

## Execution Environment

- **Local:** Python script runs in any environment with Python + Git installed
- **CI/CD:** GitHub Actions (Ubuntu latest)

---

**Related:**
- `contribute.py` — Main script with git integration
- `.github/workflows/main.yml` — CI workflow with secrets