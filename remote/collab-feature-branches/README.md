# Git Collaboration: Feature Branching & Pull Requests

## Purpose
Learn to collaborate safely on projects using feature branches and pull requests. This workflow prevents conflicts, enables code review, and maintains a clean project history.

## Prerequisites
- Basic git knowledge (clone, add, commit, push)
- GitHub/GitLab account
- Access to a shared repository

## Step-by-Step Tutorial

### 1. Clone and Setup
```bash
git clone <repository-url>
cd <project-name>
git checkout main
git pull origin main
```

### 2. Create Feature Branch
```bash
git checkout -b feature/your-feature-name
```
*Branch naming: `feature/`, `bugfix/`, or `hotfix/` prefix*

### 3. Make Changes
```bash
# Edit files
git add .
git commit -m "Add: brief description of changes"
```
*Use clear commit messages: "Add", "Fix", "Update", "Remove"*

### 4. Push Feature Branch
```bash
git push origin feature/your-feature-name
```

### 5. Create Pull Request
- Go to repository on GitHub/GitLab
- Click "New Pull Request" or "Merge Request"
- Select: `feature/your-feature-name` → `main`
- Add title and description
- Request reviewers
- Submit

### 6. Code Review Process
**As Author:**
- Respond to feedback
- Make requested changes
- Push updates (automatically updates PR)

**As Reviewer:**
- Review code changes
- Leave comments/suggestions
- Approve or request changes

### 7. Merge Pull Request
```bash
# After PR approval, merge via web interface
# Or locally:
git checkout main
git pull origin main
git merge feature/your-feature-name
git push origin main
```

### 8. Cleanup
```bash
git branch -d feature/your-feature-name
git push origin --delete feature/your-feature-name
```

## Key Commands Summary
```bash
git checkout -b feature/name    # Create branch
git push origin feature/name    # Push branch
git checkout main              # Switch to main
git pull origin main          # Update main
git merge feature/name        # Merge branch
git branch -d feature/name    # Delete local branch
```

## Best Practices
- Keep branches focused on single features
- Write clear commit messages
- Pull latest main before creating branches
- Test changes before creating PR
- Delete merged branches promptly
- Use descriptive PR titles and descriptions

## Common Issues
**Merge conflicts:** Pull latest main, merge into feature branch, resolve conflicts
**Outdated branch:** Rebase or merge main into feature branch before PR
**Failed tests:** Fix issues in feature branch, push updates