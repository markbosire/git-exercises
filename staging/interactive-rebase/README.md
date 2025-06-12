# Git Interactive Rebase Tutorial

## Setup Repository

```bash
# Initialize repository
git init rebase-demo
cd rebase-demo

# Create initial file
echo "# Project" > README.md
git add README.md
git commit -m "Initial commit"
```

## Create Sample Commits

```bash
# Commit 1: Add feature A
echo "Feature A code" > feature-a.txt
git add feature-a.txt
git commit -m "Add feature A"

# Commit 2: Fix typo (we'll squash this later)
echo "Feature A code fixed" > feature-a.txt
git add feature-a.txt
git commit -m "Fix typo in feature A"

# Commit 3: Add feature B
echo "Feature B code" > feature-b.txt
git add feature-b.txt
git commit -m "Add feature B"

# Commit 4: Temporary debug (we'll drop this)
echo "console.log('debug')" > debug.txt
git add debug.txt
git commit -m "WIP: debug stuff"

# Commit 5: Add feature C
echo "Feature C code" > feature-c.txt
git add feature-c.txt
git commit -m "Add feature C"
```

## View Current History

```bash
git log --oneline
```

Output will show:
```
abc1234 Add feature C
def5678 WIP: debug stuff
ghi9012 Add feature B
jkl3456 Fix typo in feature A
mno7890 Add feature A
pqr1234 Initial commit
```

## Interactive Rebase

Start interactive rebase for last 5 commits:

```bash
git rebase -i HEAD~5
```

This opens your editor with:
```
pick mno7890 Add feature A
pick jkl3456 Fix typo in feature A
pick ghi9012 Add feature B
pick def5678 WIP: debug stuff
pick abc1234 Add feature C
```

## Rebase Operations

### 1. Reorder Commits
Move "Add feature C" before "Add feature B":
```
pick mno7890 Add feature A
pick jkl3456 Fix typo in feature A
pick abc1234 Add feature C
pick ghi9012 Add feature B
pick def5678 WIP: debug stuff
```

### 2. Squash Commits
Combine "Fix typo" into "Add feature A":
```
pick mno7890 Add feature A
squash jkl3456 Fix typo in feature A
pick abc1234 Add feature C
pick ghi9012 Add feature B
pick def5678 WIP: debug stuff
```

### 3. Drop Commits
Remove the debug commit:
```
pick mno7890 Add feature A
squash jkl3456 Fix typo in feature A
pick abc1234 Add feature C
pick ghi9012 Add feature B
drop def5678 WIP: debug stuff
```

### Complete Example
```
pick mno7890 Add feature A
squash jkl3456 Fix typo in feature A
pick abc1234 Add feature C
pick ghi9012 Add feature B
drop def5678 WIP: debug stuff
```

Save and exit editor.

## Handle Squash Message

When squashing, git opens editor for commit message:
```
# This is a combination of 2 commits.
# This is the 1st commit message:

Add feature A

# This is the commit message #2:

Fix typo in feature A
```

Edit to:
```
Add feature A

Complete implementation with typo fixes
```

Save and exit.

## Verify Results

```bash
git log --oneline
```

Final history:
```
xyz9876 Add feature B
abc1234 Add feature C
def5678 Add feature A
pqr1234 Initial commit
```

## Common Rebase Commands

| Command | Action |
|---------|--------|
| `pick` | Keep commit as-is |
| `reword` | Keep commit, edit message |
| `edit` | Pause to amend commit |
| `squash` | Combine with previous commit |
| `fixup` | Like squash, but discard message |
| `drop` | Remove commit entirely |

## Practical Scenarios

### Scenario 1: Clean up feature branch
```bash
# Before pushing feature branch
git rebase -i main
# Squash "fix" commits, reorder logical flow
```

### Scenario 2: Fix commit order
```bash
git rebase -i HEAD~3
# Reorder commits to logical sequence
```

### Scenario 3: Remove sensitive data
```bash
git rebase -i HEAD~5
# Drop commits with passwords/keys
```

## Safety Tips

- Never rebase shared/pushed commits
- Create backup branch: `git branch backup-branch`
- Abort if confused: `git rebase --abort`
- Continue after conflicts: `git rebase --continue`

## Troubleshooting

### Conflicts during rebase:
```bash
# Fix conflicts in files
git add .
git rebase --continue
```

### Abort rebase:
```bash
git rebase --abort
```

### Reset to original state:
```bash
git reset --hard backup-branch
```