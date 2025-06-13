# Fork a Repo and Submit a Pull Request

## Purpose
Learn to contribute to projects you don't own by creating your own copy (fork), making changes, and proposing those changes back to the original project.

## Steps

### 1. Fork the Repository
- Go to the target repository on GitHub
- Click the "Fork" button (top right)
- Choose your account as the destination

### 2. Clone Your Fork
```bash
git clone https://github.com/YOUR_USERNAME/REPO_NAME.git
cd REPO_NAME
```

### 3. Create a Feature Branch
```bash
git checkout -b feature-branch-name
```

### 4. Make Your Changes
- Edit files as needed
- Add new files if required

### 5. Commit Changes
```bash
git add .
git commit -m "Brief description of changes"
```

### 6. Push to Your Fork
```bash
git push origin feature-branch-name
```

### 7. Create Pull Request
- Go to your fork on GitHub
- Click "Compare & pull request"
- Add title and description
- Click "Create pull request"

### 8. Wait for Review
- Project maintainers will review your changes
- Make additional commits if requested
- PR gets merged when approved

## Key Commands
```bash
git fork                    # (Done via GitHub interface)
git clone <fork-url>        # Download your fork
git checkout -b <branch>    # Create feature branch
git add <files>             # Stage changes
git commit -m "<message>"   # Save changes
git push origin <branch>    # Upload to your fork
```

## Best Practices
- Use descriptive branch names
- Write clear commit messages
- Keep changes focused and small
- Test your changes before submitting