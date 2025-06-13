# Git Push/Pull

## Purpose
Learn to synchronize local changes with remote repositories using `git push` and `git pull`. These commands keep your local and remote code in sync when working with teams or across multiple devices.

## Prerequisites
- Git installed
- GitHub/GitLab account
- Basic Git knowledge (add, commit)

## Exercise 1: First Push

**Goal:** Upload local commits to remote repository

1. Create new repository on GitHub
2. Clone to local machine:
   ```bash
   git clone https://github.com/username/repo-name.git
   cd repo-name
   ```
3. Create a file:
   ```bash
   echo "Hello World" > README.md
   ```
4. Stage and commit:
   ```bash
   git add README.md
   git commit -m "Add README"
   ```
5. Push to remote:
   ```bash
   git push origin main
   ```

## Exercise 2: Pull Remote Changes

**Goal:** Download changes made by others or from another device

1. Make changes directly on GitHub (edit a file)
2. Pull changes to local:
   ```bash
   git pull origin main
   ```
3. Verify changes appear locally

## Exercise 3: Push/Pull Workflow

**Goal:** Practice typical development cycle

1. Pull latest changes:
   ```bash
   git pull origin main
   ```
2. Make local changes:
   ```bash
   echo "New feature" >> feature.txt
   git add feature.txt
   git commit -m "Add new feature"
   ```
3. Push changes:
   ```bash
   git push origin main
   ```

## Exercise 4: Handling Conflicts

**Goal:** Resolve conflicts when remote has newer changes

1. Make local changes without pulling first
2. Try to push (will fail)
3. Pull changes:
   ```bash
   git pull origin main
   ```
4. Resolve any merge conflicts
5. Push resolved changes:
   ```bash
   git push origin main
   ```

## Key Commands Summary

- `git push origin main` - Send local commits to remote
- `git pull origin main` - Download remote changes
- `git push` - Push to default remote/branch
- `git pull` - Pull from default remote/branch

## Best Practices

- Always pull before starting work
- Commit frequently with clear messages
- Push regularly to share progress
- Pull before pushing to avoid conflicts