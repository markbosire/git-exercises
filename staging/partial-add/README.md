# Stage Specific Lines or Hunks using git add -p

## Interactive Staging

**Start patch mode:**
```bash
git add -p
```

**Stage specific file:**
```bash
git add -p filename
```

## Patch Mode Options

When prompted, use these keys:
- `y` - Stage this hunk
- `n` - Don't stage this hunk
- `s` - Split hunk into smaller parts
- `e` - Edit hunk manually
- `q` - Quit patch mode
- `?` - Show help

## Manual Editing

**When using 'e' option:**
- Lines starting with `-` will be removed
- Lines starting with `+` will be added
- Remove lines you don't want to stage
- Save and exit editor