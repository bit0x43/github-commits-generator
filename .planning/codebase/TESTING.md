# Testing Patterns

**Analysis Date:** 2026-05-12

## Test Framework

**Runner:**
- Python `unittest` - Standard library testing framework
- No external test dependencies
- Config: No formal test configuration (no `pytest.ini`, `setup.cfg`, or `pyproject.toml` test section)

**Assertion Library:**
- `unittest.TestCase` assertions (`assertTrue`, `assertEqual`)

**Run Commands:**
```bash
python -m unittest                  # Run all tests
python -m unittest test_contribute   # Run specific test file
python test_contribute.py           # Run directly
```

## Test File Organization

**Location:**
- Co-located with source: `contribute.py` and `test_contribute.py` in same directory

**Naming:**
- `test_*.py` pattern - Matches source filename with `test_` prefix

**Structure:**
```
.
├── contribute.py          # Main module
└── test_contribute.py    # Test module
```

## Test Structure

**Suite Organization:**
```python
import unittest
import contribute

class TestContribute(unittest.TestCase):

    def test_arguments(self):
        # Test case
        pass

    def test_contributions_per_day(self):
        # Test case
        pass

    def test_commits(self):
        # Test case
        pass
```

**Setup/Teardown:**
- None observed - No `setUp()` or `tearDown()` methods

**Assertion Pattern:**
```python
def test_arguments(self):
    args = contribute.arguments(['-nw'])
    self.assertTrue(args.no_weekends)
    self.assertEqual(args.max_commits, 10)
    self.assertTrue(1 <= contribute.contributions_per_day(args) <= 20)
```

## Mocking

**Framework:** None - No mocking library used

**Patterns:**
- No `unittest.mock` imports
- No `pytest-mock` or `unittest-mock`
- Tests directly call real functions

**What to Mock:** (Current pattern - none)
- Subprocess calls (currently `run()` executes real git commands)
- File I/O (currently writes to real filesystem)
- Random number generation (no mocking of `randint`)

**Test Isolation Issue:**
- `test_commits` test actually creates files and git commits:
```python
def test_commits(self):
    contribute.NUM = 11   # limiting the number only for unittesting
    contribute.main(['-nw', ...])
    # Creates actual directory and git repository
```
- This test leaves side effects in the filesystem

## Fixtures and Factories

**Test Data:**
- CLI argument arrays passed to `contribute.arguments()`
- Example: `['-nw', '--user_name=sampleusername', ...]`

**Location:**
- Inline in test methods - no separate fixture files

## Coverage

**Requirements:** None enforced

**View Coverage:**
```bash
python -m coverage run test_contribute.py
python -m coverage report
```
- Coverage not integrated into CI

## Test Types

**Unit Tests:**
- `test_arguments()` - Tests CLI argument parsing
- `test_contributions_per_day()` - Tests numeric validation function

**Integration Tests:**
- `test_commits()` - Tests end-to-end flow including file I/O and git operations

**E2E Tests:** None

## Common Patterns

### Async Testing: Not applicable
- No async code in this project

### Error Testing:
- No error/exception testing observed

### Test with Side Effects:
```python
def test_commits(self):
    contribute.NUM = 11   # Monkey-patching module-level state
    contribute.main([...])  # Creates files, directories, git repo
    # No cleanup - leaves artifacts
```

## CI Integration

**GitHub Actions:**
- Workflow: `.github/workflows/main.yml`
- Python version: 3.11
- Test execution: Not included in CI workflow
- The workflow runs `contribute.py` directly, not the tests

**Observations:**
- Tests are not run in CI/CD pipeline
- No test step in any workflow file

---

*Testing analysis: 2026-05-12*
