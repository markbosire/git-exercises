# Rename Branches Locally and Remotely

## Rename Local Branch

**Rename current branch:**
```bash
git branch -m new-branch-name
```

**Rename specific branch (from another branch):**
```bash
git branch -m old-branch-name new-branch-name
```

## Rename Remote Branch

**Delete old remote branch and push new one:**
```bash
git push origin --delete old-branch-name
git push origin -u new-branch-name
```

**Alternative: Push new branch first, then delete old:**
```bash
git push origin new-branch-name
git push origin --delete old-branch-name
git branch --set-upstream-to=origin/new-branch-name
```