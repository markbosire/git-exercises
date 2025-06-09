# Cherry-pick Specific Commits Between Branches

## Basic Cherry-pick

**Pick single commit:**
```bash
git cherry-pick commit-hash
```

**Pick multiple commits:**
```bash
git cherry-pick commit1 commit2 commit3
```

**Pick range of commits:**
```bash
git cherry-pick start-commit..end-commit
```

## Handle Conflicts

**If conflicts occur:**
```bash
# Resolve conflicts in files
git add resolved-files
git cherry-pick --continue
```

**Abort cherry-pick:**
```bash
git cherry-pick --abort
```

## Options

**Cherry-pick without committing:**
```bash
git cherry-pick --no-commit commit-hash
```