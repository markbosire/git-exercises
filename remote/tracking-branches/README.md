# Git Upstream Branches and HEAD

## Purpose
Learn to track remote branches and understand Git's HEAD pointer for better branch management and collaboration.

## What You'll Learn
- Track upstream branches to sync with remote repositories
- Understand HEAD as Git's current position pointer
- Manage local and remote branch relationships

## Exercises

### Exercise 1: Set Up Upstream Tracking
```bash
# Clone a repository
git clone https://github.com/your-username/sample-repo.git
cd sample-repo

# Create and switch to new branch
git checkout -b feature-branch

# Push and set upstream tracking
git push -u origin feature-branch
```

**Purpose**: Connect your local branch to a remote branch for easy push/pull operations.

### Exercise 2: Check Upstream Status
```bash
# View upstream tracking info
git branch -vv

# Check current branch's upstream
git status
```

**What to observe**: Your branch shows `[origin/feature-branch]` indicating upstream connection.

### Exercise 3: Work with Upstream
```bash
# Make changes and commit
echo "New feature" > feature.txt
git add feature.txt
git commit -m "Add new feature"

# Push to upstream (no need to specify remote/branch)
git push

# Pull from upstream
git pull
```

**Purpose**: Once upstream is set, `git push` and `git pull` work without specifying remote/branch.

### Exercise 4: Understand HEAD
```bash
# Check what HEAD points to
cat .git/HEAD

# View HEAD in log
git log --oneline -5

# Move HEAD with checkout
git checkout main
cat .git/HEAD
```

**What to observe**: HEAD points to current branch or commit.

### Exercise 5: Detached HEAD State
```bash
# Create detached HEAD by checking out specific commit
git checkout HEAD~2

# Check HEAD status
cat .git/HEAD
git status

# Return to branch
git checkout main
```

**Purpose**: Understand when HEAD points directly to a commit vs. a branch.

### Exercise 6: Multiple Upstream Scenarios
```bash
# Create branch without upstream
git checkout -b no-upstream-branch

# Check status (notice no upstream)
git status

# Set upstream later
git push -u origin no-upstream-branch

# Change upstream to different remote branch
git branch -u origin/main
```

**Purpose**: Practice different upstream configurations.

## Key Concepts

### Upstream Branches
- **Definition**: Remote branch that your local branch tracks
- **Purpose**: Enables `git push`/`git pull` without specifying remote/branch
- **Setup**: Use `-u` flag when pushing: `git push -u origin branch-name`

### HEAD
- **Definition**: Pointer to current branch or commit
- **Location**: `.git/HEAD` file
- **Types**:
  - **Attached**: Points to branch (normal state)
  - **Detached**: Points directly to commit

### Useful Commands
```bash
# View upstream relationships
git branch -vv

# Set upstream for current branch
git branch -u origin/branch-name

# Push and set upstream
git push -u origin branch-name

# View HEAD
cat .git/HEAD

# Show current position
git show HEAD
```

## Common Issues

### No Upstream Set
```bash
# Error: no upstream branch
git push
# fatal: The current branch has no upstream branch

# Solution: set upstream
git push -u origin current-branch-name
```

### Lost Upstream Connection
```bash
# Re-establish upstream
git branch -u origin/branch-name
```

## Next Steps
- Practice with multiple remotes
- Learn about tracking branches in complex workflows
- Explore HEAD movement with rebase and reset