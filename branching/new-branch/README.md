# Creating and Switching to Git Branches

## Quick Commands

### Create and switch to a new branch in one command:
```bash
git checkout -b feature/shopping-cart
```

### Alternative using newer Git syntax:
```bash
git switch -c feature/shopping-cart
```

## Step-by-Step Method

If you prefer to do it in separate steps:

```bash
# Create the branch
git branch feature/shopping-cart

# Switch to the branch
git checkout feature/shopping-cart
# OR
git switch feature/shopping-cart
```

## Example Usage

```bash
# Create and switch to a feature branch
git checkout -b feature/user-authentication

# Create and switch to a bugfix branch
git checkout -b bugfix/login-error

# Create and switch to a development branch
git switch -c develop
```

## Why We Create Branches This Way

### Isolation of Work
Branches allow you to work on different features, experiments, or bug fixes without affecting the main codebase. Each branch represents an independent line of development.

### Safe Experimentation
You can try new approaches or make breaking changes without worrying about damaging the stable main branch. If something goes wrong, you can simply delete the branch.

### Parallel Development
Multiple developers can work on different features simultaneously without conflicts. Each person works on their own branch and merges when ready.

### Code Review Process
Branches enable pull requests and code reviews. You can propose changes from your branch to the main branch, allowing team members to review before merging.

### Version Control Best Practices
Following the Git flow methodology, branches help maintain a clean commit history and make it easier to track what changes were made for specific features or fixes.

## Branch Naming Conventions

Good branch names are descriptive and follow a pattern:

- `feature/description` - for new features
- `bugfix/description` - for bug fixes  
- `hotfix/description` - for urgent production fixes
- `refactor/description` - for code refactoring
- `docs/description` - for documentation updates

## Checking Your Current Branch

```bash
# See all branches and highlight current one
git branch

# See current branch name only
git branch --show-current
```

## Switching Between Existing Branches

```bash
# Switch to an existing branch
git checkout feature/user-profile
# OR
git switch feature/user-profile

# Switch back to main branch
git checkout main
# OR
git switch main
```

## Best Practices

1. **Always create branches from main/master**: Ensure you're on the main branch before creating a new one
2. **Use descriptive names**: Make it clear what the branch is for
3. **Keep branches focused**: One branch should address one feature or fix
4. **Delete merged branches**: Clean up branches after they've been merged to keep your repository tidy
5. **Pull latest changes**: Make sure your main branch is up to date before creating new branches

```bash
# Update main branch before creating new branch
git checkout main
git pull origin main
git checkout -b feature/payment-integration
```