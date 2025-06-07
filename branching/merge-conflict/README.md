# Handling Merge Conflicts and Resolving Them Manually

## What Are Merge Conflicts?

Merge conflicts occur when Git cannot automatically merge changes from different branches because the same lines in the same files have been modified differently. Git needs your help to decide which changes to keep.

## When Conflicts Happen

### Common Scenario
```bash
# Developer A creates feature branch
git checkout -b feature/user-profile
# Modifies line 15 in user.js: const userAge = 25;

# Developer B creates different feature branch  
git checkout -b feature/user-settings
# Modifies line 15 in user.js: const userAge = 30;

# Both branches try to merge into main
# Conflict occurs on line 15 of user.js
```

## Step-by-Step Conflict Resolution

### 1. Attempt the Merge
```bash
git checkout main
git pull origin main
git merge feature/shopping-cart-updates

# Git responds with:
# Auto-merging cart.js
# CONFLICT (content): Merge conflict in cart.js
# Automatic merge failed; fix conflicts and then commit the result.
```

### 2. Check Conflict Status
```bash
git status
# Shows:
# You have unmerged paths.
#   (fix conflicts and run "git commit")
#   (use "git merge --abort" to abort the merge)
#
# Unmerged paths:
#   (use "git add <file>..." to mark resolution)
#         both modified:   cart.js
#         both modified:   checkout.js
```

### 3. Open Conflicted Files
When you open `cart.js`, you'll see conflict markers:

```javascript
function calculateTotal(items) {
    let total = 0;
    
<<<<<<< HEAD
    // Calculate total with tax included
    items.forEach(item => {
        total += item.price * item.quantity * 1.08; // 8% tax
    });
=======
    // Calculate total with discount applied first
    items.forEach(item => {
        const discountedPrice = item.price * (1 - item.discount);
        total += discountedPrice * item.quantity;
    });
>>>>>>> feature/shopping-cart-updates
    
    return total;
}
```

### 4. Understand the Conflict Markers
- `<<<<<<< HEAD`: Start of changes from current branch (main)
- `=======`: Separator between conflicting changes
- `>>>>>>> feature/shopping-cart-updates`: End of changes from merging branch

### 5. Resolve the Conflict Manually

#### Option A: Keep Only Main Branch Changes
```javascript
function calculateTotal(items) {
    let total = 0;
    
    // Calculate total with tax included
    items.forEach(item => {
        total += item.price * item.quantity * 1.08; // 8% tax
    });
    
    return total;
}
```

#### Option B: Keep Only Feature Branch Changes
```javascript
function calculateTotal(items) {
    let total = 0;
    
    // Calculate total with discount applied first
    items.forEach(item => {
        const discountedPrice = item.price * (1 - item.discount);
        total += discountedPrice * item.quantity;
    });
    
    return total;
}
```

#### Option C: Combine Both Changes (Most Common)
```javascript
function calculateTotal(items) {
    let total = 0;
    
    // Calculate total with discount applied first, then add tax
    items.forEach(item => {
        const discountedPrice = item.price * (1 - item.discount);
        total += discountedPrice * item.quantity * 1.08; // 8% tax
    });
    
    return total;
}
```

### 6. Mark Conflicts as Resolved
```bash
# After fixing all conflicts in cart.js
git add cart.js

# Check status
git status
# Shows: All conflicts fixed but you are still merging.
```

### 7. Complete the Merge
```bash
# Commit the merge resolution
git commit -m "Resolve merge conflicts in shopping cart calculation

- Combined discount logic from feature branch with tax calculation from main
- Updated calculateTotal function to apply discounts before tax
- Maintained 8% tax rate from main branch requirements"
```

## Real-World Example: CSS Conflict

### Conflicted styles.css file:
```css
.header-navigation {
    display: flex;
    justify-content: space-between;
<<<<<<< HEAD
    background-color: #2c3e50;
    padding: 20px 30px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
=======
    background-color: #34495e;
    padding: 15px 25px;
    border-bottom: 3px solid #e74c3c;
>>>>>>> feature/header-redesign
}
```

### Resolved version combining best of both:
```css
.header-navigation {
    display: flex;
    justify-content: space-between;
    background-color: #34495e;
    padding: 20px 30px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    border-bottom: 3px solid #e74c3c;
}
```

## Advanced Conflict Resolution

### Multiple File Conflicts
```bash
# See all conflicted files
git diff --name-only --diff-filter=U

# Resolve each file one by one
git add navbar.js
git add styles.css
git add index.html

# Check remaining conflicts
git status
```

### Abort Merge if Needed
```bash
# If conflicts are too complex, abort and try different approach
git merge --abort

# This returns you to the state before attempting merge
```

### Use Visual Merge Tools
```bash
# Configure and use a visual merge tool
git config --global merge.tool vimdiff
git mergetool

# Or use VS Code as merge tool
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

## Prevention Strategies

### Regular Updates
```bash
# While working on feature/payment-integration
git checkout main
git pull origin main
git checkout feature/payment-integration
git merge main  # Resolve conflicts early and often
```

### Small, Focused Changes
```bash
# Instead of one large commit affecting many files
git commit -m "Add payment form HTML structure"
git commit -m "Add payment validation logic"
git commit -m "Add payment processing API calls"
```

### Communication
```bash
# Before making major changes to shared files
# Coordinate with team to avoid conflicting modifications
```

## Common Conflict Patterns

### Import Statements
```javascript
// Conflict in imports
<<<<<<< HEAD
import { calculateTax } from './tax-utils.js';
import { formatCurrency } from './format-utils.js';
=======
import { calculateDiscount } from './discount-utils.js';
import { formatCurrency } from './format-utils.js';
>>>>>>> feature/discount-system

// Resolution: Keep both imports
import { calculateTax } from './tax-utils.js';
import { calculateDiscount } from './discount-utils.js';
import { formatCurrency } from './format-utils.js';
```

### Configuration Files
```json
// package.json conflict
<<<<<<< HEAD
  "dependencies": {
    "express": "^4.18.0",
    "mongoose": "^6.0.0"
=======
  "dependencies": {
    "express": "^4.18.0",
    "bcrypt": "^5.1.0"
>>>>>>> feature/user-authentication
  }

// Resolution: Merge dependencies
  "dependencies": {
    "express": "^4.18.0",
    "mongoose": "^6.0.0",
    "bcrypt": "^5.1.0"
  }
```

## Best Practices

### Clear Resolution Messages
```bash
git commit -m "Resolve merge conflicts in user authentication module

- Kept bcrypt dependency from feature branch
- Maintained existing mongoose version from main
- Combined login validation logic from both branches
- Updated error handling to use consistent format"
```

### Test After Resolution
```bash
# Always test your application after resolving conflicts
npm test
npm start
# Verify functionality works correctly
```

### Document Complex Resolutions
```bash
# For complex conflicts, document your decision
git commit -m "Resolve complex merge conflict in data processing pipeline

- Combined streaming approach from feature branch with error handling from main
- Maintained backward compatibility with existing API
- Added performance optimizations from both implementations
- See MERGE_CONFLICTS.md for detailed resolution rationale"
```