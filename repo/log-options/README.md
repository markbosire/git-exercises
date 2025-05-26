# Git Commit History Exercise: Exploring `git log` Options

This exercise demonstrates how to view and format your Git commit history using various `git log` options to display information in different ways.

## Exercise Objectives
- Learn essential `git log` formatting options
- Understand how to visualize branch history
- Practice filtering commit history output

## Prerequisites
- An existing Git repository with multiple commits
- At least one branch besides `main`/`master`

## Basic Commands

### 1. View standard commit history
```bash
git log
```

### 2. One-line per commit (compact view)
```bash
git log --oneline
```

### 3. Show branch topology with ASCII graph
```bash
git log --graph --oneline
```

### 4. Show complete branch topology
```bash
git log --graph --oneline --all
```

## Exercise Steps

### 1. Create sample repository (if needed)
```bash
mkdir log-exercise && cd log-exercise
git init
# Create several commits and branches to have history to view
```

### 2. Experiment with different log formats

**Basic one-line output:**
```bash
git log --oneline
```

**With author and date information:**
```bash
git log --pretty=format:"%h - %an, %ar : %s"
```

**With graph visualization:**
```bash
git log --graph --oneline --decorate
```

### 3. Filtering options

**Last N commits:**
```bash
git log -n 3  # Show last 3 commits
```

**Since/before specific dates:**
```bash
git log --since="2023-01-01"
git log --until="1 week ago"
```

**By author:**
```bash
git log --author="John"
```

### 4. Combining options

**Compact graph view with all branches:**
```bash
git log --all --graph --oneline --decorate
```

**Detailed history with stats:**
```bash
git log --stat -p
```

## Common Format Options

| Option | Description |
|--------|-------------|
| `%h`   | Abbreviated commit hash |
| `%an`  | Author name |
| `%ar`  | Author date, relative |
| `%s`   | Subject (commit message) |
| `%d`   | Ref names |

## Challenge Tasks

1. Create a custom format showing: hash, author, relative date, and message
2. Compare the history between two branches
3. Find all commits that modified a specific file
4. Search commits by message content

## Clean Up
When finished with the exercise repository:
```bash
cd ..
rm -rf log-exercise
``` 

## Additional Resources
- `git help log` - Full documentation
- `git log --help` - Online manual page
- Git documentation on pretty formats
