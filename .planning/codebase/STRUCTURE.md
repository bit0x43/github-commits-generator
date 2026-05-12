# Codebase Structure

**Analysis Date:** 2026-05-12

## Directory Layout

```
github-commits-generator/
├── .github/
│   ├── workflows/
│   │   ├── build.yml          # CI: Lint and test on push/PR
│   │   └── main.yml           # Scheduled daily commit generation
│   ├── README.md              # Project documentation
│   ├── before.png             # Screenshot: before contribution
│   └── after.png              # Screenshot: after contribution
├── .git/                      # Git repository metadata
├── contribute.py              # Main script (~128 lines)
├── test_contribute.py         # Unit tests (~32 lines)
├── LICENSE                    # Project license (MIT)
└── .planning/codebase/        # GSD analysis output
    ├── ARCHITECTURE.md
    └── STRUCTURE.md
```

## Directory Purposes

**Root (`/`):**
- Purpose: Project root containing all source code
- Contains: Python scripts, license, GitHub workflows

**`.github/workflows/`:**
- Purpose: GitHub Actions CI/CD pipelines
- Contains: build.yml (linting/testing), main.yml (scheduled execution)
- Key files: `build.yml`, `main.yml`

**`.planning/codebase/`:**
- Purpose: GSD codebase mapping output
- Contains: Architecture and structure analysis documents
- Generated: Yes (by gsd-map-codebase)
- Committed: Yes (to .planning/ in git)

## Key File Locations

**Entry Points:**
- `contribute.py`: CLI entry point, run via `python contribute.py [args]`
- `test_contribute.py`: Test entry point, run via `python -m unittest test_contribute`

**Configuration:**
- `contribute.py:83-124`: argument definitions (argparse)
- `.github/workflows/main.yml`: scheduled workflow configuration

**Core Logic:**
- `contribute.py:11-56`: main() - orchestrator function
- `contribute.py:58-63`: contribute() - single commit creation
- `contribute.py:66-67`: run() - subprocess command execution

**Testing:**
- `test_contribute.py`: unittest-based test suite

## Naming Conventions

**Files:**
- `snake_case.py`: Python scripts use snake_case
- Example: `contribute.py`, `test_contribute.py`

**Functions:**
- `snake_case`: All functions use snake_case
- Example: `contribute()`, `contributions_per_day()`, `run()`

**Variables:**
- `snake_case`: Local variables use snake_case
- Example: `user_name`, `days_before`, `repository`

**Constants:**
- No explicit constants in code; hard-coded values inline
- Magic number `20` at `contribute.py:76` (max commits cap)

**Classes:**
- Test class uses PascalCase: `TestContribute`
- Example: `test_contribute.py:6`

## Where to Add New Code

**New Feature:**
- Primary code: Modify `contribute.py` (single-file project)
- Tests: Add to `test_contribute.py`

**New CLI Argument:**
- Implementation: Add parser.add_argument() in `contribute.py:83-124`
- Validation: Add validation logic in `main()` (around lines 25-29)
- Documentation: Update `.github/README.md`

**New GitHub Workflow:**
- Implementation: Add new file in `.github/workflows/`
- Configuration: Use existing workflows as template

**Testing:**
- Unit tests: Add test methods to `test_contribute.py`
- Test patterns: Follow unittest framework as shown in existing tests

## Special Directories

**`.git/`:**
- Purpose: Git repository metadata
- Generated: Yes (by git init)
- Committed: Yes (contains repository history)

**`.github/`:**
- Purpose: GitHub-specific configuration (workflows, docs, assets)
- Generated: No (intentional project structure)
- Committed: Yes

**`.planning/`:**
- Purpose: GSD project planning and analysis
- Generated: Yes (by GSD commands)
- Committed: Yes

## Module Structure

**contribute.py:**
```
main()               # Entry point, orchestrates flow
├── arguments()      # Parse CLI arguments
├── os.mkdir()       # Create working directory
├── os.chdir()       # Change to working directory
├── run()            # Execute git init
├── run()            # Execute git config (if user provided)
├── for each day:    # Date range iteration
│   └── if conditions met:
│       └── for each commit time:
│           └── contribute()    # Create single commit
│               ├── open()       # Append to README.md
│               ├── run()        # git add
│               └── run()        # git commit
└── run()            # git remote add (if repository)
└── run()            # git push (if repository)
```

**test_contribute.py:**
```
TestContribute       # unittest.TestCase
├── test_arguments()          # Test argument parsing
├── test_contributions_per_day()  # Test random range
└── test_commits()            # End-to-end commit test
```

## Import Dependencies

**contribute.py:**
- argparse (stdlib): CLI argument parsing
- os (stdlib): File system operations
- datetime (stdlib): Date/time handling
- random (stdlib): Random number generation
- subprocess (stdlib): External command execution
- sys (stdlib): System exit

**test_contribute.py:**
- unittest (stdlib): Test framework
- contribute (local): Import module under test
- subprocess (stdlib): Check git commit count

## Build/Test Commands

**Run the script:**
```bash
python contribute.py --help
python contribute.py -r https://github.com/user/repo.git -mc 10 -fr 80
```

**Run tests:**
```bash
python -m unittest test_contribute
```

**Run linting:**
```bash
flake8 contribute.py
flake8 test_contribute.py
```

---

*Structure analysis: 2026-05-12*
