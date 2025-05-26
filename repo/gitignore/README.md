# Git Ignore Exercise: Managing Untracked Files

This exercise demonstrates how to use `.gitignore` to control which files Git tracks in your repository. You'll learn to use wildcards to ignore specific files while keeping others.

## Exercise Objectives
- Create sample files to demonstrate ignoring
- Use `.gitignore` with wildcards
- Observe the effects with `git status`

## Setup Instructions

1. **Initialize a new Git repository**
   ```bash
   mkdir git-ignore-exercise
   cd git-ignore-exercise
   git init
   ```

2. **Create the test files**
   ```bash
   touch ignoreme donotignoreme ignoremetoo keepme.txt deleteme.log
   ```

## Exercise Steps

### 1. Create a .gitignore file
```bash
touch .gitignore
```

### 2. Edit the .gitignore file
Add these patterns to your `.gitignore`:
```
# Ignore specific filenames
ignoreme

# Ignore using wildcards
ignoreme*

# Ignore all .log files
*.log
```

### 3. Check which files are being ignored
```bash
git status
```

### Expected Output
You should see:
- `donotignoreme` and `keepme.txt` as untracked files (will be tracked if you add them)
- The other files (`ignoreme`, `ignoremetoo`, `deleteme.log`) won't appear because they're ignored

### 4. Verify with git status
After running `git status`, you should only see:
```
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        donotignoreme
        keepme.txt
```

## Key Concepts Demonstrated

1. **Exact filename matching**: `ignoreme` ignores just that specific file
2. **Wildcard patterns**: `ignoreme*` ignores all files starting with "ignoreme"
3. **Extension patterns**: `*.log` ignores all files with the .log extension
4. **The .gitignore file itself should be tracked** (added to version control)

## Advanced Practice

Try these modifications to deepen your understanding:

1. Add `!ignoremeimportant` to your `.gitignore` to make an exception
2. Create a directory and ignore all files within it using `mydirectory/`
3. Use `**/temp` to ignore all files named 'temp' in any subdirectory

Remember to check `git status` after each change to see the effects!

## Clean Up
When finished, you can remove the test directory:
```bash
cd ..
rm -rf git-ignore-exercise
```
