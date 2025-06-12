# Git Blame 

## Setup Repository with History

```bash
# Initialize repository
git init blame-demo
cd blame-demo

# Create initial file
cat > user.js << 'EOF'
class User {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }
}
EOF

git add user.js
git commit -m "Initial User class" --author="Alice <alice@example.com>"

# Add validation method
cat >> user.js << 'EOF'

    validate() {
        return this.name && this.email;
    }
EOF

git add user.js
git commit -m "Add validation method" --author="Bob <bob@example.com>"

# Add email validation
sed -i '/validate() {/,/}/ c\
    validate() {\
        return this.name && this.email && this.email.includes("@");\
    }' user.js

git add user.js
git commit -m "Improve email validation" --author="Charlie <charlie@example.com>"

# Add toString method
cat >> user.js << 'EOF'

    toString() {
        return `${this.name} <${this.email}>`;
    }
}
EOF

git add user.js
git commit -m "Add toString method" --author="Diana <diana@example.com>"

# Fix bug in toString
sed -i 's/return `${this.name} <${this.email}>`;/return `User: ${this.name} <${this.email}>`;/' user.js

git add user.js
git commit -m "Fix toString format" --author="Eve <eve@example.com>"
```

## Basic Git Blame

### View Line-by-Line History
```bash
git blame user.js
```

Output:
```
^abc1234 (Alice   2025-06-12 10:00:00 +0000  1) class User {
^abc1234 (Alice   2025-06-12 10:00:00 +0000  2)     constructor(name, email) {
^abc1234 (Alice   2025-06-12 10:00:00 +0000  3)         this.name = name;
^abc1234 (Alice   2025-06-12 10:00:00 +0000  4)         this.email = email;
^abc1234 (Alice   2025-06-12 10:00:00 +0000  5)     }
def5678 (Bob     2025-06-12 10:05:00 +0000  6) 
ghi9012 (Charlie 2025-06-12 10:10:00 +0000  7)     validate() {
ghi9012 (Charlie 2025-06-12 10:10:00 +0000  8)         return this.name && this.email && this.email.includes("@");
ghi9012 (Charlie 2025-06-12 10:10:00 +0000  9)     }
jkl3456 (Diana   2025-06-12 10:15:00 +0000 10) 
jkl3456 (Diana   2025-06-12 10:15:00 +0000 11)     toString() {
mno7890 (Eve     2025-06-12 10:20:00 +0000 12)         return `User: ${this.name} <${this.email}>`;
jkl3456 (Diana   2025-06-12 10:15:00 +0000 13)     }
jkl3456 (Diana   2025-06-12 10:15:00 +0000 14) }
```

### Simplified Output
```bash
git blame -s user.js  # Show only commit hash
```

Output:
```
^abc1234  1) class User {
^abc1234  2)     constructor(name, email) {
^abc1234  3)         this.name = name;
^abc1234  4)         this.email = email;
^abc1234  5)     }
def5678  6) 
ghi9012  7)     validate() {
ghi9012  8)         return this.name && this.email && this.email.includes("@");
ghi9012  9)     }
jkl3456 10) 
jkl3456 11)     toString() {
mno7890 12)         return `User: ${this.name} <${this.email}>`;
jkl3456 13)     }
jkl3456 14) }
```

## Blame Options

### Show Email Addresses
```bash
git blame -e user.js
```

### Show Line Numbers
```bash
git blame -n user.js
```

### Limit Line Range
```bash
# Show only lines 7-9
git blame -L 7,9 user.js
```

Output:
```
ghi9012 (Charlie 2025-06-12 10:10:00 +0000  7)     validate() {
ghi9012 (Charlie 2025-06-12 10:10:00 +0000  8)         return this.name && this.email && this.email.includes("@");
ghi9012 (Charlie 2025-06-12 10:10:00 +0000  9)     }
```

### Show Specific Function
```bash
# Show lines containing "toString" function
git blame -L /toString/,+3 user.js
```

### Copy Detection
```bash
# Detect moved/copied lines
git blame -C user.js

# More aggressive copy detection
git blame -CCC user.js
```

## Advanced Blame Features

### Ignore Whitespace Changes
```bash
git blame -w user.js  # Ignore whitespace
```

### Show Root Commits
```bash
git blame --root user.js
```

### Reverse Blame (Show When Lines Were Deleted)
```bash
git blame --reverse HEAD~2..HEAD user.js
```

### Blame at Specific Commit
```bash
# Show blame as of specific commit
git blame ghi9012 user.js

# Show blame one commit before
git blame ghi9012~1 user.js
```

## Practical Scenarios

### Scenario 1: Bug Investigation
```bash
# Create file with bug
cat > buggy.js << 'EOF'
function processData(data) {
    // TODO: Add validation
    if (data.length = 0) {  // BUG: assignment instead of comparison
        return [];
    }
    return data.map(item => item.toUpperCase());
}
EOF

git add buggy.js
git commit -m "Add data processing function" --author="Junior <junior@example.com>"

# Later, bug is found - who wrote this line?
git blame -L 3,3 buggy.js
```

Output shows Junior wrote the buggy line.

### Scenario 2: Code Review History
```bash
# Add complex function over multiple commits
cat > complex.js << 'EOF'
function calculateTax(income, deductions) {
    const taxableIncome = income - deductions;
    return taxableIncome * 0.2;
}
EOF

git add complex.js
git commit -m "Add basic tax calculation" --author="Senior <senior@example.com>"

# Update tax brackets
sed -i 's/return taxableIncome \* 0.2;/if (taxableIncome > 50000) {\
        return taxableIncome * 0.3;\
    }\
    return taxableIncome * 0.2;/' complex.js

git add complex.js
git commit -m "Add progressive tax brackets" --author="Manager <manager@example.com>"

# View evolution
git blame complex.js
```

### Scenario 3: Documentation Changes
```bash
# Create README with multiple contributors
cat > README.md << 'EOF'
# Project Documentation

## Installation
Run `npm install` to install dependencies.

## Usage
Basic usage example coming soon.
EOF

git add README.md
git commit -m "Add initial documentation" --author="Writer <writer@example.com>"

# Update installation instructions
sed -i 's/Run `npm install` to install dependencies./Run `npm install --production` for production install.\
For development: `npm install --dev`/' README.md

git add README.md
git commit -m "Update install instructions" --author="DevOps <devops@example.com>"

# View documentation history
git blame README.md
```

## Blame with Porcelain Format

### Machine-Readable Output
```bash
git blame --porcelain user.js
```

Output:
```
abc1234 1 1 1
author Alice
author-mail <alice@example.com>
author-time 1718184000
author-tz +0000
committer Alice
committer-mail <alice@example.com>
committer-time 1718184000
committer-tz +0000
summary Initial User class
filename user.js
	class User {
abc1234 2 2
	    constructor(name, email) {
```

### Parse Porcelain Output
```bash
# Extract just authors and lines
git blame --porcelain user.js | awk '
/^author / { author = substr($0, 8) }
/^\t/ { print author ": " substr($0, 2) }
'
```

## Blame Integration

### Blame with Log
```bash
# Show commits that modified specific lines
git log -L 7,9:user.js --oneline
```

Output:
```
ghi9012 Improve email validation
def5678 Add validation method
```

### Blame with Show
```bash
# Show full commit details for specific line
LINE_COMMIT=$(git blame -L 8,8 user.js | cut -d' ' -f1)
git show $LINE_COMMIT
```

### Blame with Diff
```bash
# Show what changed in the commit that last modified line 8
git blame -L 8,8 user.js | cut -d' ' -f1 | xargs git show
```

## Blame Aliases

### Useful Git Aliases
```bash
# Add to ~/.gitconfig
git config --global alias.who 'blame -w -C -C -C'
git config --global alias.wholine 'blame -L'
git config --global alias.whofunc 'blame -L'

# Usage
git who user.js
git wholine 7,9 user.js
```

### Blame Function
```bash
# Add to ~/.bashrc or ~/.zshrc
blame-line() {
    git blame -L "$2,$2" "$1" | head -1
}

# Usage: blame-line user.js 8
```

## Blame Limitations and Workarounds

### Issue: Formatting Commits Hide Real Authors
```bash
# Code formatter commit hides real authors
git blame --ignore-rev abc1234 user.js

# Ignore multiple formatting commits
git blame --ignore-revs-file .git-blame-ignore-revs user.js
```

Create `.git-blame-ignore-revs`:
```
# Formatting commits to ignore
abc1234  # Prettier formatting
def5678  # ESLint fixes
```

### Issue: Moved Code
```bash
# Detect code movement between files
git blame -C -C -C user.js

# Show original file for moved code
git blame -C -C -C --show-stats user.js
```

### Issue: Renamed Files
```bash
# Follow file renames
git blame --follow renamed-file.js

# Blame file through renames
git log --follow --patch -- renamed-file.js
```

## Blame in Different Formats

### JSON Output (with jq)
```bash
# Custom JSON blame output
git blame --porcelain user.js | \
awk '
BEGIN { print "[" }
/^[0-9a-f]+ / { 
    if (commit) print commit ","
    commit = "  {\"commit\":\"" $1 "\", \"line\":" $3 
}
/^author / { commit = commit ", \"author\":\"" substr($0,8) "\"" }
/^\t/ { commit = commit ", \"code\":\"" substr($0,2) "\"}" }
END { print commit "\n]" }
' | jq '.'
```

### HTML Blame Report
```bash
# Generate HTML blame report
cat > generate-blame-report.sh << 'EOF'
#!/bin/bash
echo "<html><body><h1>Blame Report</h1><pre>" > blame-report.html
git blame --date=short $1 | \
  sed 's/&/\&amp;/g; s/</\&lt;/g; s/>/\&gt;/g' >> blame-report.html
echo "</pre></body></html>" >> blame-report.html
EOF

chmod +x generate-blame-report.sh
./generate-blame-report.sh user.js
```

## Blame Best Practices

### 1. Investigate Systematically
```bash
# Start with line of interest
git blame -L 15,15 problematic-file.js

# Get commit hash
COMMIT=$(git blame -L 15,15 problematic-file.js | cut -d' ' -f1)

# Examine full commit
git show $COMMIT

# Check if this was part of larger change
git log --oneline -5 $COMMIT
```

### 2. Use Appropriate Flags
```bash
# For code reviews
git blame -w -C file.js  # Ignore whitespace, detect copies

# For bug hunting
git blame --show-stats file.js  # Show copy/move statistics

# For historical analysis
git blame --since="2024-01-01" file.js
```

### 3. Combine with Other Tools
```bash
# Blame + grep for specific patterns
git blame file.js | grep "TODO\|FIXME\|BUG"

# Blame + author filtering
git blame file.js | grep "Alice"

# Blame + date filtering
git blame --since="1 month ago" file.js
```

## Common Blame Workflows

### Workflow 1: Bug Investigation
```bash
# 1. Find the problematic line
git blame -L $LINE_NUM $FILE

# 2. Get commit details
git show $COMMIT_HASH

# 3. Check commit context
git log --oneline -3 $COMMIT_HASH

# 4. Review related files in same commit
git show --name-only $COMMIT_HASH
```

### Workflow 2: Code Ownership
```bash
# Find who owns most of the file
git blame $FILE | cut -d'(' -f2 | cut -d' ' -f1 | sort | uniq -c | sort -nr

# Find recent contributors
git blame --since="3 months ago" $FILE | cut -d'(' -f2 | cut -d' ' -f1 | sort | uniq -c
```

### Workflow 3: Historical Analysis
```bash
# Track evolution of specific function
git log -L :function_name:file.js --oneline

# Combine with blame for complete picture
git blame -L /function_name/,/^}/ file.js
```

## Troubleshooting

### Blame Shows Wrong Information
```bash
# Check if file was renamed
git log --follow --name-status file.js

# Use more aggressive copy detection
git blame -C -C -C file.js
```

### Performance Issues with Large Files
```bash
# Blame specific range only
git blame -L 100,200 large-file.js

# Use incremental blame
git blame --incremental large-file.js
```

### Blame Doesn't Show Expected Author
```bash
# Check for ignored commits
git config --get-regexp blame.ignore

# Temporarily disable ignore list
git blame --ignore-revs-file=/dev/null file.js
```