# Git Repository Initialization Guide

This guide will walk you through the basic steps to initialize a Git repository and make your first commit.

## Prerequisites
- Git installed on your system
- Basic command line knowledge

## Steps to Initialize a Git Repository

1. **Open your terminal or command prompt**

2. **Navigate to your project directory**
   ```bash
   cd path/to/your/project
   ```

3. **Initialize a new Git repository**
   ```bash
   git init
   ```
   This creates a new `.git` subdirectory in your project folder.

4. **Check the status of your repository**
   ```bash
   git status
   ```
   This shows which files are untracked or modified.

5. **Add files to the staging area**
   ```bash
   git add .
   ```
   (or specify individual files instead of `.` for all files)

6. **Commit your changes**
   ```bash
   git commit -m "Initial commit"
   ```
   Replace "Initial commit" with a meaningful message about your changes.

## Next Steps
- Create a remote repository (on GitHub, GitLab, etc.)
- Connect your local repository to the remote:
  ```bash
  git remote add origin [remote-repository-url]
  ```
- Push your changes:
  ```bash
  git push -u origin main
  ```


