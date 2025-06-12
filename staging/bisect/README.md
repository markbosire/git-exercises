# Git Bisect 

## Setup Repository with Bug

```bash
# Initialize repository
git init bisect-demo
cd bisect-demo

# Create initial working version
echo "function calculate(a, b) { return a + b; }" > calculator.js
echo "console.log(calculate(2, 3)); // Should output 5" >> calculator.js
git add calculator.js
git commit -m "Initial calculator - working"

# Add more features (all working)
echo "function multiply(a, b) { return a * b; }" >> calculator.js
git add calculator.js
git commit -m "Add multiply function"

echo "function subtract(a, b) { return a - b; }" >> calculator.js
git add calculator.js
git commit -m "Add subtract function"

echo "function divide(a, b) { return a / b; }" >> calculator.js
git add calculator.js
git commit -m "Add divide function"

# Introduce bug (break addition)
sed -i 's/return a + b/return a * b/' calculator.js
git add calculator.js
git commit -m "Refactor calculation logic"

# More commits after bug
echo "// Version 1.1" >> calculator.js
git add calculator.js
git commit -m "Update version comment"

echo "function power(a, b) { return Math.pow(a, b); }" >> calculator.js
git add calculator.js
git commit -m "Add power function"

# Current state: addition is broken but we don't know when
```

## View History and Identify Problem

```bash
git log --oneline
```

Output:
```
abc1234 Add power function
def5678 Update version comment
ghi9012 Refactor calculation logic    # ← Bug introduced here
jkl3456 Add divide function
mno7890 Add subtract function
pqr2345 Add multiply function
stu6789 Initial calculator - working
```

Test current version:
```bash
node calculator.js  # Outputs 6 instead of 5 - BUG!
```

## Start Git Bisect

### Initialize Bisect Session
```bash
git bisect start
```

### Mark Bad and Good Commits
```bash
# Current HEAD is bad
git bisect bad

# First commit was good
git bisect good stu6789
```

Git responds:
```
Bisecting: 2 revisions left to test after this (roughly 2 steps)
[mno7890] Add subtract function
```

## Bisect Process

### Test Current Commit
```bash
# Test the checked out commit
node calculator.js
```

If output is 5 (correct):
```bash
git bisect good
```

Git responds:
```
Bisecting: 0 revisions left to test after this (roughly 1 step)
[ghi9012] Refactor calculation logic
```

### Test Next Commit
```bash
node calculator.js  # Outputs 6 - broken!
git bisect bad
```

Git identifies the culprit:
```
ghi9012 is the first bad commit
commit ghi9012
Author: Your Name <your.email@example.com>
Date: Thu Jun 12 10:30:00 2025

    Refactor calculation logic
```

### End Bisect Session
```bash
git bisect reset
```

## Automated Bisect

### Create Test Script
```bash
# Create test script
cat > test_calculator.sh << 'EOF'
#!/bin/bash
result=$(node calculator.js 2>/dev/null | head -1)
if [ "$result" = "5" ]; then
    exit 0  # Good commit
else
    exit 1  # Bad commit
fi
EOF

chmod +x test_calculator.sh
```

### Run Automated Bisect
```bash
git bisect start
git bisect bad HEAD
git bisect good stu6789
git bisect run ./test_calculator.sh
```

Git automatically finds:
```
ghi9012 is the first bad commit
Running: ./test_calculator.sh
Bisecting: 0 revisions left to test after this (roughly 0 steps)
```

## Advanced Bisect Scenarios

### Scenario 1: Skip Commits
```bash
git bisect start
git bisect bad
git bisect good stu6789

# Current commit won't compile
git bisect skip

# Continue with next commit
node calculator.js
git bisect good  # or bad
```

### Scenario 2: Multiple Good/Bad Points
```bash
git bisect start

# Mark multiple known good commits
git bisect good pqr2345
git bisect good mno7890

# Mark current as bad
git bisect bad

# Git finds optimal starting point
```

### Scenario 3: Bisect with Complex Test
```bash
# Test script with multiple checks
cat > complex_test.sh << 'EOF'
#!/bin/bash

# Test multiple functions
node -e "
const calc = require('./calculator.js');
const tests = [
    calculate(2, 3) === 5,
    multiply(2, 3) === 6,
    subtract(5, 2) === 3
];
process.exit(tests.every(t => t) ? 0 : 1);
"
EOF

git bisect run ./complex_test.sh
```

## Bisect Commands Reference

| Command | Purpose |
|---------|---------|
| `git bisect start` | Begin bisect session |
| `git bisect bad [commit]` | Mark commit as bad |
| `git bisect good [commit]` | Mark commit as good |
| `git bisect skip` | Skip problematic commit |
| `git bisect reset` | End session, return to HEAD |
| `git bisect run <script>` | Automate with test script |
| `git bisect log` | Show bisect session log |
| `git bisect replay <file>` | Replay bisect from log |

## Practical Scenarios

### Scenario 1: Performance Regression
```bash
# Create performance test
cat > perf_test.sh << 'EOF'
#!/bin/bash
start_time=$(date +%s%N)
node app.js > /dev/null 2>&1
end_time=$(date +%s%N)
duration=$(( (end_time - start_time) / 1000000 ))

# Fail if takes longer than 100ms
[ $duration -lt 100 ]
EOF

git bisect start
git bisect bad HEAD  # Current version is slow
git bisect good v1.0  # v1.0 was fast
git bisect run ./perf_test.sh
```

### Scenario 2: Test Suite Failure
```bash
# Use existing test suite
git bisect start
git bisect bad
git bisect good last-green-build
git bisect run npm test
```

### Scenario 3: Feature Regression
```bash
# Test specific feature
cat > feature_test.sh << 'EOF'
#!/bin/bash
# Test user login feature
curl -s -X POST localhost:3000/login \
  -d '{"user":"test","pass":"123"}' \
  -H "Content-Type: application/json" | \
  grep -q '"success":true'
EOF

git bisect run ./feature_test.sh
```

## Bisect Workflow Best Practices

### 1. Prepare Environment
```bash
# Ensure clean working directory
git status
git stash  # if needed

# Start bisect
git bisect start
```

### 2. Choose Good Reference Points
```bash
# Use tags for known good versions
git bisect good v1.0

# Or use recent known good commit
git bisect good last-working-commit
```

### 3. Create Reliable Tests
```bash
# Test should be deterministic
# Exit 0 for good, exit 1 for bad
# Handle edge cases (compilation failures, etc.)
```

### 4. Document Process
```bash
# Save bisect log
git bisect log > bisect-session.log

# Document findings
echo "Bug introduced in commit $(git rev-parse HEAD)" > bug-report.txt
```

## Troubleshooting

### Bisect Shows Wrong Result
```bash
# Review bisect log
git bisect log

# Reset and try again with different good/bad points
git bisect reset
git bisect start
git bisect bad HEAD~5  # Try different bad commit
git bisect good HEAD~15
```

### Skip Problematic Commits
```bash
# When commit won't build/test
git bisect skip

# Skip range of commits
git bisect skip commit1..commit5
```

### Visualize Bisect Progress
```bash
# See commits being tested
git bisect visualize
# or
gitk bisect/bad --not bisect/good-*
```

## Real-World Example

```bash
# Bug report: "Login stopped working sometime last week"

# Start investigation
git bisect start
git bisect bad HEAD  # Current version broken
git bisect good HEAD~20  # 20 commits ago it worked

# Git suggests middle commit to test
# Test: curl -X POST /api/login ...
# Result: Still broken
git bisect bad

# Test next suggested commit
# Result: Works fine
git bisect good

# Continue until git finds: 
# "abc1234 is the first bad commit"

# Examine the problematic commit
git show abc1234

# Found: accidentally removed authentication middleware
# Fix and deploy
```

## Integration with CI/CD

```bash
# Automated bisect in CI pipeline
#!/bin/bash
git bisect start
git bisect bad $CURRENT_COMMIT
git bisect good $LAST_KNOWN_GOOD

# Run automated test suite
git bisect run ./run_tests.sh

# Report results
echo "Faulty commit: $(git bisect log | grep 'first bad commit')"
git bisect reset
```

## Key Benefits

- **Efficient**: O(log n) search instead of linear
- **Automated**: Can run unattended with test scripts  
- **Reliable**: Binary search guarantees finding the exact commit
- **Flexible**: Works with any testable condition
- **Scalable**: Effective even with thousands of commits