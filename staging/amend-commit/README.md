# Amend the Most Recent Commit

## Basic Amend

**Change commit message:**
```bash
git commit --amend -m "New commit message"
```

**Open editor to modify message:**
```bash
git commit --amend
```

## Add Changes to Last Commit

**Stage new changes and amend:**
```bash
git add modified-files
git commit --amend --no-edit
```

**Amend with new message:**
```bash
git add modified-files
git commit --amend -m "Updated commit message"
```

## Author Information

**Change author of last commit:**
```bash
git commit --amend --author="Name <email@example.com>"
```

⚠️ **Warning:** Only amend commits that haven't been pushed to shared repositories.