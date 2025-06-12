# Git Tags 

## Setup Repository

```bash
# Initialize repository
git init tags-demo
cd tags-demo

# Create initial commits
echo "# Project v1.0" > README.md
git add README.md
git commit -m "Initial commit"

echo "Feature 1 complete" > feature1.txt
git add feature1.txt
git commit -m "Add feature 1"

echo "Bug fixes applied" > bugfix.txt
git add bugfix.txt
git commit -m "Fix critical bugs"
```

## View Current History

```bash
git log --oneline
```

Output:
```
abc1234 Fix critical bugs
def5678 Add feature 1
ghi9012 Initial commit
```

## Lightweight Tags

### Create Lightweight Tag
```bash
# Tag current commit (HEAD)
git tag v1.0

# Tag specific commit
git tag v0.9 def5678

# Tag with different naming
git tag release-candidate abc1234
```

### List Tags
```bash
git tag
```

Output:
```
release-candidate
v0.9
v1.0
```

### List Tags with Pattern
```bash
git tag -l "v*"
```

Output:
```
v0.9
v1.0
```

## Annotated Tags

### Create Annotated Tag
```bash
# Current commit with message
git tag -a v1.1 -m "Release version 1.1 - Major features"

# Specific commit with message
git tag -a v1.0-beta def5678 -m "Beta release for testing"

# Multi-line message
git tag -a v2.0 -m "Version 2.0 Release

Features:
- New authentication system
- Performance improvements
- Bug fixes"
```

### Create Annotated Tag Interactively
```bash
git tag -a v1.2
# Opens editor for detailed message
```

## View Tag Information

### Show Tag Details
```bash
# Lightweight tag (shows commit info)
git show v1.0

# Annotated tag (shows tag info + commit info)
git show v1.1
```

### Tag Information Only
```bash
git tag -n
```

Output:
```
release-candidate Fix critical bugs
v0.9             Add feature 1
v1.0             Fix critical bugs
v1.0-beta        Beta release for testing
v1.1             Release version 1.1 - Major features
```

### Detailed Tag Info
```bash
git tag -n5  # Show up to 5 lines of message
```

## Tag Operations

### Delete Tags
```bash
# Delete local tag
git tag -d v1.0-beta

# Delete multiple tags
git tag -d v0.9 release-candidate
```

### Rename Tag
```bash
# Create new tag, delete old one
git tag v1.0-stable v1.0
git tag -d v1.0
```

### Tag Later (Forgot to Tag)
```bash
# View history
git log --oneline

# Tag old commit
git tag -a v0.8 ghi9012 -m "Initial release"
```

## Working with Remote Tags

### Push Tags
```bash
# Push single tag
git push origin v1.1

# Push all tags
git push origin --tags

# Push only annotated tags
git push origin --follow-tags
```

### Fetch Tags
```bash
# Fetch all tags from remote
git fetch --tags

# Fetch specific tag
git fetch origin refs/tags/v2.0:refs/tags/v2.0
```

### Delete Remote Tags
```bash
# Delete from remote
git push origin --delete v1.0-beta

# Alternative syntax
git push origin :refs/tags/v1.0-beta
```

## Practical Scenarios

### Scenario 1: Release Workflow
```bash
# Feature complete
git add .
git commit -m "Complete v2.0 features"

# Create release tag
git tag -a v2.0 -m "Version 2.0 Release

New Features:
- User authentication
- Dashboard redesign
- API improvements

Bug Fixes:
- Memory leak fixed
- Login timeout issue resolved"

# Push to remote
git push origin v2.0
```

### Scenario 2: Hotfix Release
```bash
# Fix critical bug
git checkout v1.1
git checkout -b hotfix-1.1.1
# Make fixes...
git commit -m "Fix security vulnerability"

# Tag hotfix
git tag -a v1.1.1 -m "Hotfix 1.1.1 - Security patch"
git push origin v1.1.1
```

### Scenario 3: Pre-release Tags
```bash
# Alpha release
git tag -a v2.0-alpha.1 -m "Alpha release - testing only"

# Beta release
git tag -a v2.0-beta.1 -m "Beta release - feature complete"

# Release candidate
git tag -a v2.0-rc.1 -m "Release candidate 1"

# Final release
git tag -a v2.0 -m "Version 2.0 - Stable release"
```

## Checkout Tags

### Checkout Specific Version
```bash
# Checkout tag (detached HEAD)
git checkout v1.1

# Create branch from tag
git checkout -b bugfix-branch v1.1
```

### List Commits Between Tags
```bash
git log v1.0..v1.1 --oneline
```

## Tag Comparison

| Feature | Lightweight | Annotated |
|---------|-------------|-----------|
| Storage | Pointer to commit | Full object |
| Message | No | Yes |
| Tagger info | No | Yes (name, email, date) |
| GPG signing | No | Yes |
| Best for | Temporary markers | Releases |

## Semantic Versioning with Tags

```bash
# Major version (breaking changes)
git tag -a v2.0.0 -m "Major release - breaking changes"

# Minor version (new features)
git tag -a v2.1.0 -m "Minor release - new features"

# Patch version (bug fixes)
git tag -a v2.1.1 -m "Patch release - bug fixes"

# Pre-release versions
git tag -a v2.2.0-beta.1 -m "Beta release"
git tag -a v2.2.0-rc.1 -m "Release candidate"
```

## Advanced Tag Operations

### Find Tags Containing Commit
```bash
git tag --contains abc1234
```

### List Tags by Date
```bash
git tag -l --sort=-version:refname
git tag -l --sort=creatordate
```

### Tag Archive
```bash
# Create archive from tag
git archive --format=zip --output=v1.1.zip v1.1
```

## Best Practices

- Use annotated tags for releases (`-a` flag)
- Use semantic versioning (MAJOR.MINOR.PATCH)
- Include meaningful release notes in tag messages
- Tag stable points in development
- Don't move or delete published tags
- Use lightweight tags for temporary markers only

## Troubleshooting

### Tag Already Exists
```bash
# Force overwrite (dangerous)
git tag -f v1.0

# Better: delete and recreate
git tag -d v1.0
git tag -a v1.0 -m "New message"
```

### Missing Tags After Clone
```bash
# Fetch all tags
git fetch --tags
```