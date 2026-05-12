# Coding Conventions

**Analysis Date:** 2026-05-12

## Language

**Primary:**
- Python 3.11 - All source code

## Naming Patterns

**Files:**
- `snake_case.py` - Standard Python convention
- Example: `contribute.py`, `test_contribute.py`

**Functions:**
- `snake_case` - Standard Python convention
- Example: `contribute()`, `run()`, `message()`, `contributions_per_day()`

**Variables:**
- `snake_case` - Standard Python convention
- Example: `user_name`, `user_email`, `days_before`

**Constants:**
- No explicit constants observed (module-level mutable attributes like `contribute.NUM`)

**Classes:**
- `PascalCase` - Standard Python convention
- Example: `TestContribute` in `test_contribute.py`

## Code Style

**Formatting:**
- No automatic formatter configured (no `black`, `ruff`, etc.)
- Manual indentation with spaces (4 spaces typical)
- Maximum line length appears to be ~80-100 characters

**Line Length Examples:**
- Long arguments wrapped at ~80-100 chars
- Multi-line conditionals with backslash continuation

**Linting:**
- No linting configuration detected (no `.flake8`, `.pylintrc`, `pyproject.toml`)
- No pre-commit hooks

## Import Organization

**Order:**
1. Standard library imports (`import os`, `import sys`)
2. Third-party imports (`from random import randint`)
3. Local imports (`import contribute`)

**Full import list from `contribute.py`:**
```python
import argparse
import os
from datetime import datetime
from datetime import timedelta
from random import randint
from subprocess import Popen
import sys
```

## Error Handling

**Approach:**
- Simple string-based exit for validation: `sys.exit('days_before must not be negative')`
- No try/except blocks
- No custom exception classes
- No error logging

**Pattern - Validation:**
```python
if days_before < 0:
    sys.exit('days_before must not be negative')
if days_after < 0:
    sys.exit('days_after must not be negative')
```

**Pattern - Command Execution:**
```python
def run(commands):
    Popen(commands).wait()
```
- No error handling for subprocess failures

## Comments

**Docstrings:**
- Not used in main module

**Inline Comments:**
- None observed

**Variable/Function Comments:**
- None observed

## Function Design

**Size:**
- Functions are relatively small and focused
- `main()`: ~55 lines - handles CLI parsing and orchestration
- `contribute()`: ~5 lines - writes file and creates git commit
- `run()`: ~2 lines - wrapper around Popen
- `message()`: ~1 line - formats date string
- `contributions_per_day()`: ~6 lines - validates and returns random value
- `arguments()`: ~42 lines - defines all CLI arguments

**Parameters:**
- Uses argparse for CLI argument handling
- No type hints
- Default parameter values used where appropriate

**Return Values:**
- argparse: Returns `Namespace` object with attributes
- Functions return various types (int, str, None)

## Module Design

**Exports:**
- All functions are module-level (public by default)
- No `__all__` defined

**Testing Pattern:**
- Tests import module directly: `import contribute`
- Tests access internal functions: `contribute.arguments()`, `contribute.contributions_per_day()`
- Tests modify module-level state: `contribute.NUM = 11`

## Style Observations

**POSITIVE:**
- Single-purpose functions
- Clear function names describing intent
- Simple, readable code flow
- No unnecessary complexity

**AREAS FOR IMPROVEMENT:**
- No type annotations
- No docstrings
- Validation errors use `sys.exit()` with string (unusual pattern)
- No error handling for file I/O or subprocess failures
- Direct module-level attribute modification in tests (`contribute.NUM = 11`)
- Duplicate import: `from datetime import datetime` and `from datetime import timedelta`

---

*Convention analysis: 2026-05-12*
