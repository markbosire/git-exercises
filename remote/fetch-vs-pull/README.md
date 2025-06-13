# Git Fetch vs Pull

## Purpose
Learn when to use `git fetch` vs `git pull` to safely sync your local repository with remote changes.

## Key Difference
- **fetch**: Downloads remote changes but doesn't merge them
- **pull**: Downloads remote changes AND merges them automatically

## Git Fetch

### What it does
Downloads latest commits from remote without changing your working directory.

### When to use
- Check what's new before merging
- Review changes before applying them
- Working on sensitive code that shouldn't break

### Steps
```bash
# 1. Fetch latest changes
git fetch origin

# 2. See what's new
git log HEAD..origin/main

# 3. Merge when ready
git merge origin/main
```

## Git Pull

### What it does
Downloads AND merges remote changes in one command (`fetch` + `merge`).

### When to use
- Quick updates on stable branches
- You trust the remote changes
- Working alone or on non-critical code

### Steps
```bash
# Pull and merge in one step
git pull origin main
```

## Exercise

1. **Safe approach**: Use `fetch` → review → `merge`
2. **Quick approach**: Use `pull` for routine updates
3. **Best practice**: Default to `fetch` until you're comfortable with remote changes

## Remember
- `fetch` = safe preview
- `pull` = immediate merge
- When in doubt, fetch first