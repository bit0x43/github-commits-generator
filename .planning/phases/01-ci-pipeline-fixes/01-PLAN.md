---
wave: 1
depends_on: []
files_modified:
  - .github/workflows/build.yml
  - .github/workflows/main.yml
autonomous: true
requirements:
  - CI-01
  - CI-02
---

# Plan: 01-ci-pipeline-fixes

## Goal

Restore and modernize broken build pipelines.

## Tasks

### 01-fix-build-yml

**Requirements:** CI-01

**<action>**
Update `.github/workflows/build.yml`:
1. Change `python-version` matrix from `[2.7, 3.8]` to `[3.12]`
2. Upgrade `actions/checkout@v2` to `actions/checkout@v4`
3. Upgrade `actions/setup-python@v2` to `actions/setup-python@v5`
4. Remove flake8 lint (or replace with ruff if preferred)
5. Keep unittest test execution
</action>

**<read_first>**
- `.github/workflows/build.yml` — current broken config
</read_first>

**<acceptance_criteria>**
- build.yml has `python-version: ['3.12']` in matrix
- actions/checkout@v4 is used
- actions/setup-python@v5 is used
- flake8 line removed or replaced
- `python -m unittest test_contribute` still runs in test step
</acceptance_criteria>

---

### 02-fix-main-yml

**Requirements:** CI-02

**<action>**
Review and fix `.github/workflows/main.yml`:
1. Ensure Python 3.12 is used (update if needed)
2. Verify workflow syntax is correct
3. Check that secret references are proper
4. Ensure workflow runs on correct triggers (schedule, push, dispatch)
</action>

**<read_first>**
- `.github/workflows/main.yml` — current daily workflow
</read_first>

**<acceptance_criteria>**
- main.yml uses Python 3.12
- Workflow syntax is valid (no YAML errors)
- All secret references use proper format (`secrets.ACTIONS_GITHUB_TOKEN`, etc.)
- Triggers: schedule, push to main, workflow_dispatch
</acceptance_criteria>

---

### 03-verify-ci

**Requirements:** CI-01, CI-02

**<action>**
Verify both workflows work:
1. Check YAML syntax is valid (no parsing errors)
2. Verify all action versions exist and are accessible
3. Confirm matrix builds correctly
</action>

**<read_first>**
- `.github/workflows/build.yml`
- `.github/workflows/main.yml`
</read_first>

**<acceptance_criteria>**
- Both YAML files parse without errors
- No deprecated action versions referenced
- Python 3.12 is specified in both workflows
</acceptance_criteria>

---

## Verification

1. Both workflow files are syntactically valid YAML
2. Actions versions are current (v4 for checkout, v5 for setup-python)
3. Python 3.12 is used in both
4. Test execution still works (`python -m unittest test_contribute`)
5. Lint step removed or replaced (flake8 → ruff or removed)

---

## Must Haves

- [ ] build.yml uses Python 3.12
- [ ] build.yml uses updated GitHub Actions
- [ ] main.yml is valid and uses Python 3.12
- [ ] CI pipelines will pass when pushed
