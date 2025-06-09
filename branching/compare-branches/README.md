# Compare Branches with diff and log

## Using git diff

**Compare two branches:**
```bash
git diff branch1..branch2
```

**Compare current branch with another:**
```bash
git diff branch-name
```

**Show only file names that differ:**
```bash
git diff --name-only branch1..branch2
```

## Using git log

**Show commits in branch1 but not in branch2:**
```bash
git log branch2..branch1
```

**Show commits unique to each branch:**
```bash
git log --left-right --oneline branch1...branch2
```

**Visual comparison:**
```bash
git log --oneline --graph branch1 branch2
```