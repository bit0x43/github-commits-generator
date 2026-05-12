# GitHub Activity Generator

Generate a GitHub contribution graph for any target repository.

## Features

- **Backfill Mode** — Generate historical commits for the past year
- **Daily Mode** — Create a single commit for today
- **GitHub Actions** — Run automatically on a schedule
- **Parallel Generation** — Optional multi-threaded backfill

## Prerequisites

- Python 3.12 or higher
- Git
- A GitHub Personal Access Token (PAT) with `repo` scope

## Quick Start

### 1. Clone this repository

```bash
git clone https://github.com/bit0x43/github-commits-generator.git
cd github-commits-generator
```

### 2. Create a target repository

Create a new **empty** GitHub repository (do not initialize with README or .gitignore).

### 3. Run locally

```bash
python contribute.py \
  --mode daily \
  --repository=https://x-access-token:YOUR_PAT@github.com/username/target-repo.git \
  --user_name="Your Name" \
  --user_email="your@email.com"
```

Replace:
- `YOUR_PAT` — your GitHub Personal Access Token
- `username/target-repo` — your target repository
- `Your Name` / `your@email.com` — your GitHub credentials

## Usage

### Local Usage

**Daily mode** — One commit for today:
```bash
python contribute.py --mode daily --repository=...
```

**Backfill mode** — Historical commits (default):
```bash
python contribute.py --mode backfill --repository=...
```

**Backfill with options:**
```bash
python contribute.py \
  --mode backfill \
  --days_before=365 \
  --max_commits=10 \
  --frequency=80 \
  --no_weekends \
  --parallel \
  --repository=...
```

### GitHub Actions (Automated)

The `main.yml` workflow runs daily at 13:00 UTC. To enable:

1. Go to your repository **Settings** → **Secrets and variables** → **Actions**
2. Add these secrets:
   - `TARGET_USER` — your GitHub username
   - `TARGET_EMAIL` — your GitHub email
   - `TARGET_REPO` — target repository (e.g., `username/target-repo`)
3. The workflow will run automatically every day

You can also trigger manually:
- Go to **Actions** → **Daily Contribute** → **Run workflow**

## Options

| Flag | Default | Description |
|------|---------|-------------|
| `-m, --mode` | `backfill` | Execution mode: `backfill` or `daily` |
| `-mc, --max_commits` | 10 | Max commits per day (1-20) |
| `-fr, --frequency` | 80 | Percentage of active days (0-100) |
| `-nw, --no_weekends` | false | Skip weekends |
| `-db, --days_before` | 365 | Days to backfill |
| `-da, --days_after` | 0 | Future days to include |
| `-p, --parallel` | false | Use 4 worker threads |
| `-r, --repository` | — | Target repo URL (empty = local only) |
| `-un, --user_name` | — | Git user name |
| `-ue, --user_email` | — | Git user email |

## Creating a GitHub Personal Access Token

1. Go to **GitHub Settings** → **Developer settings** → **Personal access tokens** → **Tokens (classic)**
2. Click **Generate new token (classic)**
3. Select scopes: `repo` (full control)
4. Copy the token (it won't be shown again)

## Troubleshooting

### "Repository not found"
- Ensure the target repository exists and is empty
- Verify your PAT has `repo` scope

### "Authentication failed"
- Regenerate your GitHub PAT
- Check the repository URL format is correct

### "Email not recognized"
- Ensure the email in `--user_email` matches your GitHub account email
- Check with: `git config --global user.email`

### "No commits appearing on GitHub"
- Wait 2-5 minutes — GitHub reindexes periodically
- For private repos, enable "Private contributions" on your profile

### "Workflow not running"
- Verify secrets are set in repository Settings
- Check the Actions tab for workflow runs

## License

MIT License — do what you want with it.
