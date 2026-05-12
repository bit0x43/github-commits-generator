# Architecture

**Analysis Date:** 2026-05-12

## System Overview

```text
┌─────────────────────────────────────────────────────────────┐
│                    CLI Entry Point                           │
│                  `contribute.py:main()`                      │
├─────────────────────────────────────────────────────────────┤
│                      Core Logic                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐   │
│  │   Date      │  │    Git      │  │     Arguments      │   │
│  │   Generator │  │  Operations │  │      Parser         │   │
│  │ `contribute`│  │    `run`    │  │   `arguments`      │   │
│  └─────────────┘  └─────────────┘  └─────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
         │                │                    │
         ▼                ▼                    ▼
┌─────────────────────────────────────────────────────────────┐
│                  Operating System                            │
│    - File System (os.mkdir, os.chdir, open file)            │
│    - Git CLI (subprocess Popen)                             │
│    - Date/Time (datetime)                                   │
└─────────────────────────────────────────────────────────────┘
```

## Component Responsibilities

| Component | Responsibility | File |
|-----------|----------------|------|
| `main()` | Orchestrates repository generation, date range iteration, commit creation | `contribute.py:11-56` |
| `contribute()` | Creates a single commit with timestamp and message | `contribute.py:58-63` |
| `run()` | Executes shell commands via subprocess | `contribute.py:66-67` |
| `message()` | Generates commit message from date | `contribute.py:70-71` |
| `contributions_per_day()` | Randomly determines number of commits per day (capped at 1-20) | `contribute.py:74-80` |
| `arguments()` | Parses CLI arguments via argparse | `contribute.py:83-124` |

## Pattern Overview

**Overall:** Monolithic script with procedural flow

**Key Characteristics:**
- Single-file architecture (`contribute.py` ~128 lines)
- No external dependencies beyond standard library
- Direct subprocess calls for git operations
- Sequential date iteration with randomization for commits

## Layers

**CLI Layer:**
- Purpose: Entry point and argument handling
- Location: `contribute.py:11-56`
- Contains: `main()` function, argument parsing
- Depends on: `arguments()`, `contribute()`, `run()`
- Used by: Python interpreter via `if __name__ == "__main__"`

**Core Logic Layer:**
- Purpose: Commit generation and git operations
- Location: `contribute.py:58-80`
- Contains: `contribute()`, `message()`, `contributions_per_day()`, `run()`
- Depends on: datetime, random, subprocess, os
- Used by: `main()`

**Utility Layer:**
- Purpose: Date manipulation and string formatting
- Location: `contribute.py:4-5, 70-71`
- Contains: datetime imports, timedelta usage, message formatting
- Depends on: Python standard library
- Used by: Core logic layer

## Data Flow

### Primary Request Path

1. **CLI Entry** (`contribute.py:11`) - `main()` receives sys.argv or def_args
2. **Argument Parsing** (`contribute.py:12`) - `arguments()` parses CLI flags
3. **Directory Setup** (`contribute.py:14-31`) - Creates working directory and initializes git repo
4. **Git Configuration** (`contribute.py:34-38`) - Sets user.name and user.email if provided
5. **Date Range Iteration** (`contribute.py:40-47`) - Loops through days_before to days_after
6. **Conditional Commit** (`contribute.py:43-47`) - Checks weekend filter and frequency probability
7. **Commit Creation** (`contribute.py:47`) - Calls `contribute()` for each commit time
8. **Remote Push** (`contribute.py:49-52`) - Pushes to remote if repository URL provided
9. **Success Output** (`contribute.py:54-55`) - Prints completion message

### Commit Creation Flow

1. **File Write** (`contribute.py:59-60`) - Appends message to README.md
2. **Git Stage** (`contribute.py:61`) - Runs `git add .`
3. **Git Commit** (`contribute.py:62-63`) - Creates commit with message and backdated timestamp

**State Management:**
- No persistent state; uses command-line arguments and environment variables
- Working directory is changed via `os.chdir()` during execution

## Key Abstractions

**Argument Parser:**
- Purpose: Unified interface for all CLI configuration options
- Examples: `contribute.py:83-124`
- Pattern: argparse with named arguments and defaults

**Command Runner:**
- Purpose: Execute external git commands
- Examples: `contribute.py:66-67`
- Pattern: `Popen(commands).wait()` - blocking subprocess call

**Date Generator:**
- Purpose: Create date ranges and format commit messages
- Examples: `contribute.py:70-71`
- Pattern: datetime.strftime for message formatting

## Entry Points

**Primary Entry:**
- Location: `contribute.py:127-128` (`if __name__ == "__main__"`)
- Triggers: Direct invocation via `python contribute.py` or `python -m contribute`
- Responsibilities: Parse args, generate commits, optionally push to remote

**Test Entry:**
- Location: `test_contribute.py` (unittest-based)
- Triggers: `python -m unittest test_contribute`
- Responsibilities: Verify argument parsing and commit generation logic

## Architectural Constraints

- **Threading:** Single-threaded, sequential execution
- **Global state:** Uses `os.chdir()` to change working directory (side effect)
- **Circular imports:** Not applicable (single-file project)
- **No module-level state:** All variables are local or function parameters

## Anti-Patterns

### Direct os.chdir()

**What happens:** `contribute.py:31` uses `os.chdir(directory)` to change working directory
**Why it's wrong:** Changes process-wide working directory, can affect subsequent operations or be unexpected by callers
**Do this instead:** Pass directory path explicitly to all file operations or use context managers

### Global random state

**What happens:** Uses module-level `randint` from random without seeding control
**Why it's wrong:** Non-deterministic behavior makes testing and reproducibility difficult
**Do this instead:** Accept seed as parameter or use `random.seed()` for deterministic runs

### Subprocess without error handling

**What happens:** `contribute.py:67` uses `Popen(commands).wait()` without checking return codes
**Why it's wrong:** Git command failures are silently ignored; partial failures go unnoticed
**Do this instead:** Check `wait()` return value and raise exceptions on non-zero exit codes

### Hard-coded commit message format

**What happens:** `contribute.py:70-71` returns hard-coded message format
**Why it's wrong:** Not configurable; users cannot customize commit messages
**Do this instead:** Accept message template as CLI argument

## Error Handling

**Strategy:** Minimal error checking with sys.exit() on invalid arguments

**Patterns:**
- `sys.exit('days_before must not be negative')` - argument validation
- No exception handling for git operations or file I/O failures
- No logging framework - uses print() for output

## Cross-Cutting Concerns

**Logging:** `print()` statements for success messages (with ANSI color codes)
**Validation:** Basic argument validation only (negative days_before/days_after)
**Authentication:** Not implemented in code; handled via environment/git config for remote push
**Error reporting:** Process exit with error messages via `sys.exit()`

---

*Architecture analysis: 2026-05-12*