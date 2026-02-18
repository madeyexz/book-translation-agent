---
name: pr-screenshot
description: Add before/after comparison screenshots to a PR using browser automation. Captures UI on the base branch and the PR branch, saves to docs/screenshot/, and updates the PR body with a side-by-side comparison table using GitHub CDN URLs.
license: MIT
metadata:
  author: madeyexz
  version: "2.0.0"
---

# PR Screenshot Skill

Add before/after comparison screenshots to a pull request using browser automation and GitHub CDN. This captures the UI on the **base branch** (before) and the **PR branch** (after), then presents them in a side-by-side comparison table.

## Arguments

- `$ARGUMENTS` - PR number and optional description of what to screenshot (e.g. pages/routes to capture)

## Execution Steps

### 1. Parse Arguments & Get Repo Info

```bash
# Get current repo info
REPO=$(gh repo view --json nameWithOwner -q .nameWithOwner)

# Get PR info (base branch, head branch, title, body)
gh pr view <PR_NUMBER> --json baseRefName,headRefName,title,body

# Save branch names
BASE_BRANCH=<baseRefName>   # e.g. main
HEAD_BRANCH=<headRefName>   # e.g. feature/new-ui
```

### 2. Capture "Before" Screenshots (Base Branch)

```bash
# Stash any uncommitted changes, then checkout the base branch
git stash
git checkout $BASE_BRANCH

# Install dependencies (in case they differ)
pnpm install

# Start dev server
pnpm dev &
DEV_PID=$!
sleep 5

# Create screenshot directories
mkdir -p docs/screenshot/before docs/screenshot/after

# Open the page(s) to screenshot
agent-browser open "http://localhost:3000/path/to/feature"
sleep 2

# Login if needed (check CLAUDE.md for credentials)
agent-browser snapshot -i
agent-browser fill @e1 "email"
agent-browser fill @e2 "password"
agent-browser click @e3

# Navigate to the relevant page and take screenshot(s)
agent-browser screenshot docs/screenshot/before/feature-overview.png
agent-browser screenshot docs/screenshot/before/feature-detail.png

# Stop dev server and close browser
agent-browser close
kill $DEV_PID 2>/dev/null || true
pkill -f "next dev" || true
```

### 3. Capture "After" Screenshots (PR Branch)

```bash
# Checkout the PR branch
git checkout $HEAD_BRANCH
git stash pop || true

# Install dependencies
pnpm install

# Start dev server
pnpm dev &
DEV_PID=$!
sleep 5

# Open the same page(s)
agent-browser open "http://localhost:3000/path/to/feature"
sleep 2

# Login if needed (same steps as before)
agent-browser snapshot -i
agent-browser fill @e1 "email"
agent-browser fill @e2 "password"
agent-browser click @e3

# Navigate to the same page and take screenshot(s)
agent-browser screenshot docs/screenshot/after/feature-overview.png
agent-browser screenshot docs/screenshot/after/feature-detail.png

# Stop dev server and close browser
agent-browser close
kill $DEV_PID 2>/dev/null || true
pkill -f "next dev" || true
```

### 4. Screenshot Naming Convention

Use matching filenames in `before/` and `after/` so the comparison table is easy to construct:

```
docs/screenshot/
├── before/
│   ├── feature-overview.png
│   ├── feature-detail.png
│   └── feature-empty.png
└── after/
    ├── feature-overview.png
    ├── feature-detail.png
    └── feature-empty.png
```

Naming ideas:
- `feature-overview.png` — main view
- `feature-detail.png` — expanded/detailed view
- `feature-empty.png` — empty state
- `feature-loading.png` — loading state
- `feature-mobile.png` — mobile viewport

### 5. Commit & Push

```bash
git add docs/screenshot/
git commit -m "$(cat <<'EOF'
docs: add before/after screenshots for [feature]

Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
EOF
)"
git push
```

### 6. Update PR Body with Before/After Comparison Table

**IMPORTANT**: Use GitHub CDN URL format with `?raw=true`:

```
https://github.com/<REPO>/blob/<BRANCH>/docs/screenshot/<before|after>/<filename>.png?raw=true
```

Where `<REPO>` is the `nameWithOwner` from step 1 and `<BRANCH>` is the **PR head branch**.

Update PR body with a comparison table:

```bash
gh pr edit <PR_NUMBER> --body "$(cat <<'EOF'
# PR Title

## Summary
[existing summary]

## Screenshots

### Feature Overview
| Before | After |
|--------|-------|
| ![Before](https://github.com/<REPO>/blob/<BRANCH>/docs/screenshot/before/feature-overview.png?raw=true) | ![After](https://github.com/<REPO>/blob/<BRANCH>/docs/screenshot/after/feature-overview.png?raw=true) |

### Detail View
| Before | After |
|--------|-------|
| ![Before](https://github.com/<REPO>/blob/<BRANCH>/docs/screenshot/before/feature-detail.png?raw=true) | ![After](https://github.com/<REPO>/blob/<BRANCH>/docs/screenshot/after/feature-detail.png?raw=true) |

[rest of PR body]
EOF
)"
```

**Add one table per page/state you screenshot.** Each table has a single row with the before image on the left and the after image on the right.

### 7. Clean Up

```bash
agent-browser close
pkill -f "next dev" || true
git checkout $HEAD_BRANCH
```

## GitHub CDN URL Format

Always use this exact format for images to render in GitHub:

```
https://github.com/OWNER/REPO/blob/BRANCH/path/to/image.png?raw=true
```

- **OWNER/REPO**: Get from `gh repo view --json nameWithOwner -q .nameWithOwner`
- **BRANCH**: The PR head branch name (get from `gh pr view`)
- **?raw=true**: Required suffix for images to display

## Authentication

If the app requires login, check the project's `CLAUDE.md` for test credentials.

## Tips

- **Consistency is key**: screenshot the exact same pages/viewports/states on both branches so the comparison is meaningful
- Use `agent-browser tab list` to see all open tabs
- Use `agent-browser tab N` to switch tabs
- For elements not in snapshot, use CSS selectors: `agent-browser click "button.submit"`
- Scroll with: `agent-browser scroll down 500`
- For responsive comparisons, resize the viewport: `agent-browser eval "window.resizeTo(375, 812)"`
- If a page doesn't exist on the base branch (new page), skip the "before" screenshot and note it in the PR body as **New Page**
