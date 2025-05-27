# Git Undo Mechanisms: reset, checkout, and restore

This guide explains the differences between three Git commands for undoing changes: 
`git reset`, `git checkout`, and `git restore`.

## Table of Contents
1. [Overview](#overview)
2. [git reset](#git-reset)
3. [git checkout](#git-checkout)
4. [git restore](#git-restore)
5. [Comparison Table](#comparison-table)
6. [When to Use Each](#when-to-use-each)

## Overview

Git provides multiple ways to undo changes, each with different purposes:
- `git reset`: Manipulates the commit history and staging area
- `git checkout`: Switches branches or restores working tree files
- `git restore`: Newer command specifically for restoring files

## git reset

### What it does:
- Moves the current branch HEAD to a specified commit
- Can modify the staging area and working directory depending on mode

### Modes:
1. `--soft`: Only moves HEAD, keeps staging and working directory unchanged
   ```bash
   git reset --soft <commit>
   ```
2. `--mixed` (default): Moves HEAD and resets staging area, keeps working directory
   ```bash
   git reset <commit>
   ```
3. `--hard`: Moves HEAD, resets staging area AND working directory
   ```bash
   git reset --hard <commit>
   ```

### Use cases:
- Undo commits while keeping changes in working directory (`--soft`)
- Unstage changes (`--mixed`)
- Completely discard commits and changes (`--hard`)

## git checkout

### Two primary uses:

1. **Switch branches**:
   ```bash
   git checkout <branch-name>
   ```

2. **Restore files** (legacy functionality):
   ```bash
   git checkout <commit> -- <file>
   ```
   - Brings file version from specified commit to working directory and staging area
   - Can be dangerous as it overwrites changes without warning

## git restore

### Purpose:
- Newer command (Git 2.23+) specifically designed for restoring files
- More intuitive and safer alternative to `git checkout` for file restoration

### Modes:
1. Restore working directory file (discard unstaged changes):
   ```bash
   git restore <file>
   ```
2. Restore staged file (unstage changes):
   ```bash
   git restore --staged <file>
   ```
3. Restore file from specific commit:
   ```bash
   git restore --source=<commit> <file>
   ```

## Comparison Table

| Command        | Scope               | Primary Use Case                          | Safety |
|----------------|---------------------|-------------------------------------------|--------|
| `git reset`    | Commit history      | Undo commits, unstage changes             | ⚠ (with --hard) |
| `git checkout` | Branches/files      | Switch branches, restore files (legacy)   | ⚠ (file restoration) |
| `git restore`  | Files               | Restore working directory or staged files | Safe   |

## When to Use Each

- **Want to undo commits?** → `git reset`
- **Want to switch branches?** → `git checkout`
- **Want to discard unstaged changes?** → `git restore`
- **Want to unstage files?** → `git restore --staged` or `git reset`
- **Want to restore old file version?** → `git restore --source=<commit>`

> ⚠ Warning: Both `git reset --hard` and `git checkout <file>` can permanently discard changes. Always ensure you don't need your changes before using these commands.



