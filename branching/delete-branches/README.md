# Delete Local and Remote Branches

## Why Delete Branches?

After successfully merging a feature branch, keeping old branches clutters your repository and can cause confusion. Clean branch management keeps your repository organized and makes it easier to see what work is currently active.

## Deleting Local Branches

### Safe Delete (Merged Branch)
```bash
# Delete a branch that has been merged
git branch -d feature/user-authentication

# Git responds with:
# Deleted branch feature/user-authentication (was a1b2c3d).
```

### Force Delete (Unmerged Branch)
```bash
# Force delete a branch (even if not merged)
git branch -D feature/experimental-feature

# Git responds with:
# Deleted branch feature/experimental-feature (was x9y8z7w).
```

### Real-World Example: Post-Merge Cleanup
```bash
# After successfully merging shopping cart feature
git checkout main
git merge feature/shopping-cart-checkout
# Merge successful

# Clean up the local branch
git branch -d feature/shopping-cart-checkout
# Deleted branch feature/shopping-cart-checkout (was m5n6o7p).

# Verify branch is gone
git branch
# * main
#   feature/product-catalog
#   bugfix/mobile-navigation
```

## Deleting Remote Branches

### Delete Remote Branch
```bash
# Delete branch from remote repository (GitHub, GitLab, etc.)
git push origin --delete feature/user-authentication

# Alternative syntax (older Git versions)
git push origin :feature/user-authentication

# Git responds with:
# To https://github.com/yourname/yourproject.git
#  - [deleted]         feature/user-authentication
```

### Complete Cleanup Workflow
```bash
# Complete branch cleanup after merge
# 1. Switch to main and update
git checkout main
git pull origin main

# 2. Delete local branch
git branch -d feature/payment-integration

# 3. Delete remote branch  
git push origin --delete feature/payment-integration

# 4. Verify cleanup
git branch -a
# * main
#   remotes/origin/main
#   remotes/origin/feature/user-dashboard
```

## Batch Delete Multiple Branches

### Delete Multiple Local Branches
```bash
# Delete several merged branches at once
git branch -d feature/old-search feature/deprecated-api bugfix/old-login-issue

# Force delete multiple unmerged branches
git branch -D feature/experiment-1 feature/experiment-2 feature/prototype-ui
```

### Delete All Merged Local Branches
```bash
# List all merged branches (excluding main)
git branch --merged main | grep -v main

# Delete all merged branches except main
git branch --merged main | grep -v main | xargs git branch -d

# Example output:
# Deleted branch feature/header-redesign (was a1b2c3d).
# Deleted branch bugfix/footer-links (was e4f5g6h).
# Deleted branch feature/contact-form (was i7j8k9l).
```

## Checking Branch Status Before Deletion

### List All Branches
```bash
# Show local branches
git branch
# * main
#   feature/user-profiles
#   feature/email-notifications
#   bugfix/navbar-dropdown

# Show remote branches
git branch -r
#   origin/main
#   origin/feature/user-profiles
#   origin/feature/shopping-cart

# Show all branches (local and remote)
git branch -a
# * main
#   feature/user-profiles
#   remotes/origin/main
#   remotes/origin/feature/user-profiles
#   remotes/origin/feature/shopping-cart
```

### Check Merge Status
```bash
# See which branches are merged into main
git branch --merged main
# * main
#   feature/user-authentication
#   bugfix/login-validation

# See which branches are NOT merged into main
git branch --no-merged main
#   feature/advanced-search
#   feature/payment-gateway
#   bugfix/mobile-menu
```

## Safe Branch Deletion Practices

### Verify Branch is Merged
```bash
# Before deleting, confirm the branch was merged
git log --oneline main | head -10
# Look for your feature commits in main branch history

# Or check merge status
git branch --merged main | grep feature/shopping-cart
# If it appears, it's been merged and safe to delete
```

### Create Backup Before Force Delete
```bash
# If you need to force delete but want a backup
git tag backup/feature-experiment feature/risky-experiment
git branch -D feature/risky-experiment

# Later, if you need it back:
git checkout -b feature/risky-experiment backup/feature-experiment
```

## Handling Common Scenarios

### Branch Delete Fails - Not Fully Merged
```bash
git branch -d feature/incomplete-work
# error: The branch 'feature/incomplete-work' is not fully merged.
# If you are sure you want to delete it, run 'git branch -D feature/incomplete-work'.

# Options:
# 1. Merge the branch first
git checkout main
git merge feature/incomplete-work
git branch -d feature/incomplete-work

# 2. Force delete if you're sure
git branch -D feature/incomplete-work
```

### Remote Branch Already Deleted
```bash
# Someone else deleted the remote branch
git push origin --delete feature/user-settings
# error: unable to delete 'feature/user-settings': remote ref does not exist

# Clean up local tracking reference
git remote prune origin

# Or update remote references
git fetch --prune
```

### Clean Up Stale Remote References
```bash
# Remove local references to deleted remote branches
git remote prune origin

# Or during fetch
git fetch --prune

# See what would be pruned (dry run)
git remote prune origin --dry-run
# * [would prune] origin/feature/old-feature
# * [would prune] origin/bugfix/resolved-issue
```

## Automated Branch Cleanup Scripts

### Bash Script for Local Cleanup
```bash
#!/bin/bash
# cleanup-merged-branches.sh

echo "Cleaning up merged local branches..."

# Switch to main branch
git checkout main

# Pull latest changes
git pull origin main

# Delete all merged branches except main
git branch --merged main | grep -v -E "(main|master)" | xargs -n 1 git branch -d

echo "Local branch cleanup complete!"
```

### Usage Example
```bash
# Make script executable
chmod +x cleanup-merged-branches.sh

# Run cleanup
./cleanup-merged-branches.sh

# Output:
# Cleaning up merged local branches...
# Already on 'main'
# Deleted branch feature/user-login (was a1b2c3d).
# Deleted branch bugfix/header-styling (was e4f5g6h).
# Local branch cleanup complete!
```

## Team Workflow Examples

### Feature Branch Lifecycle
```bash
# 1. Create and work on feature
git checkout -b feature/product-reviews
git commit -m "Add product review form"
git commit -m "Add review validation and storage"
git commit -m "Add review display on product pages"

# 2. Push and create pull request
git push origin feature/product-reviews

# 3. After pull request is merged by team lead
git checkout main
git pull origin main  # Get the merged changes

# 4. Clean up both local and remote
git branch -d feature/product-reviews
git push origin --delete feature/product-reviews
```

### Hotfix Branch Lifecycle
```bash
# 1. Create hotfix from main
git checkout main
git pull origin main
git checkout -b hotfix/critical-security-patch

# 2. Make fix and deploy
git commit -m "Fix SQL injection vulnerability in search"
git push origin hotfix/critical-security-patch

# 3. Merge directly to main (emergency)
git checkout main
git merge hotfix/critical-security-patch
git push origin main

# 4. Immediate cleanup
git branch -d hotfix/critical-security-patch  
git push origin --delete hotfix/critical-security-patch
```

## Best Practices

### Regular Cleanup Schedule
```bash
# Weekly cleanup routine
# 1. Update main branch
git checkout main && git pull origin main

# 2. See what can be cleaned up
git branch --merged main | grep -v main

# 3. Delete merged branches
git branch --merged main | grep -v main | xargs git branch -d

# 4. Clean up remote references
git fetch --prune
```

### Protection Against Accidents
```bash
# Set up Git aliases for safer deletion
git config --global alias.delete-branch '!f() { git branch -d "$1" && git push origin --delete "$1"; }; f'
git config --global alias.force-delete-branch '!f() { git branch -D "$1" && git push origin --delete "$1"; }; f'

# Usage:
git delete-branch feature/completed-feature
git force-delete-branch feature/abandoned-experiment
```

### Branch Naming for Easy Cleanup
```bash
# Use consistent prefixes for automated cleanup
git checkout -b feature/add-search-filters     # Features
git checkout -b bugfix/fix-login-redirect      # Bug fixes  
git checkout -b hotfix/patch-security-issue    # Hotfixes
git checkout -b experiment/try-new-framework    # Experiments

# Script can then target specific patterns
git branch | grep "experiment/" | xargs git branch -D
```