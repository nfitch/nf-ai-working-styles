# Bootstrap Instructions for Claude

**AUDIENCE:** This file contains instructions for Claude (AI agent) on how to bootstrap the working styles pattern into a project or verify an existing installation.

## Overview

The working styles pattern consists of:
1. Symlinked `working-styles/` directory in the root of a target repository.
2. Minimal working styles section in project's `CLAUDE.md`
3. `.gitignore` entry for the symlinked directory

## Three Bootstrap Scenarios

### Scenario A: Brand New Empty Repository

**When to use:** Repository has no CLAUDE.md file and no working-styles directory.

**Steps:**

1. **Determine paths**
   ```bash
   # Get absolute path to nf-ai-working-styles
   STYLES_PATH="/path/to/nf-ai-working-styles/working-styles"
   # Get absolute path to project directory
   PROJECT_PATH=$(pwd)
   ```

2. **Create symlink**
   ```bash
   ln -s "$STYLES_PATH" "$PROJECT_PATH/working-styles"
   ```

3. **Create CLAUDE.md from template**
   - Copy content from `nf-ai-working-styles/claude-template.md`
   - Replace `[PROJECT NAME]` with actual project name
   - Add project-specific content below the working styles section

4. **Create/update .gitignore**
   ```bash
   # Check if .gitignore exists
   if [ -f .gitignore ]; then
     # Append if it doesn't already contain working-styles/
     if ! grep -q "working-styles/" .gitignore; then
       echo "" >> .gitignore
       echo "# AI agent working styles (symlinked)" >> .gitignore
       echo "working-styles/" >> .gitignore
     fi
   else
     # Create new .gitignore
     cat > .gitignore << 'EOF'
# AI agent working styles (symlinked)
working-styles/
EOF
   fi
   ```

5. **Verify installation** (see Verification section below)

### Scenario B: Existing Repository with No CLAUDE.md

**When to use:** Repository exists with files/code but has no CLAUDE.md.

**Steps:**

1. **Check for existing working-styles directory**
   ```bash
   if [ -e working-styles ]; then
     echo "WARNING: working-styles already exists"
     ls -la working-styles
     # Ask user what to do: rename, remove, or abort
   fi
   ```

2. **Follow Scenario A steps** (symlink, create CLAUDE.md, update .gitignore)

3. **Verify installation**

### Scenario C: Existing Repository with Existing CLAUDE.md

**When to use:** Repository has existing CLAUDE.md that needs working styles added.

**CRITICAL:** Be extremely careful not to overwrite existing project-specific content.

**Steps:**

1. **Read existing CLAUDE.md**
   ```bash
   cat CLAUDE.md
   ```

2. **Check if working styles section exists**
   - Search for "Working Styles" or "working-styles/" references
   - If already present, verify it's correct (see Verification section)
   - If missing, proceed with merge

3. **Merge working styles section**
   - **DO NOT overwrite the entire file**
   - Ask user where to add working styles section:
     - At the top (after title)
     - After a specific section
     - At the bottom
   - Use Edit tool to insert minimal working styles section:
   ```markdown
   ## Working Styles

   This project uses AI agent working styles for consistent collaboration.

   Consult `working-styles/README.md` for instructions.
   ```

4. **Create symlink** (if not exists)
   ```bash
   if [ ! -e working-styles ]; then
     ln -s "/path/to/nf-ai-working-styles/working-styles" ./working-styles
   fi
   ```

5. **Update .gitignore** (same as Scenario A, careful append)

6. **Verify installation**

## Verification Steps

After bootstrap, verify the installation is correct:

### 1. Check Symlink Exists
```bash
ls -la working-styles
# Should show: working-styles -> /path/to/nf-ai-working-styles/working-styles
```

### 2. Verify Symlink Target
```bash
readlink working-styles
# Should output: /path/to/nf-ai-working-styles/working-styles
```

### 3. Test Symlink Works
```bash
ls working-styles/
# Should show directories: nf (and any other user styles)
cat working-styles/README.md
# Should display the working styles README
```

### 4. Check CLAUDE.md Structure
```bash
grep -A 10 "Working Styles" CLAUDE.md
```

Verify CLAUDE.md contains:
- "Working Styles" section
- Instructions to identify user
- Instructions to discover styles (`ls working-styles/`)
- Instructions to load style file
- Reference to `working-styles/README.md`

### 5. Check .gitignore
```bash
grep "working-styles" .gitignore
# Should show: working-styles/
```

### 6. Test Style Loading
```bash
# List available styles
ls working-styles/
# Read a style file
cat working-styles/nf/working-style.md
```

All commands should succeed without errors.

## Verification Report Format

After running verification, provide report:

```
Working Styles Installation Verification

1. Symlink exists: [PASS/FAIL]
   Location: ./working-styles -> /path/to/...

2. Symlink target accessible: [PASS/FAIL]
   Can read: working-styles/README.md

3. Available styles: [count]
   - nf
   [- other styles if present]

4. CLAUDE.md has working styles section: [PASS/FAIL]
   Location: line X

5. .gitignore configured: [PASS/FAIL]
   Entry: working-styles/

6. Style files readable: [PASS/FAIL]
   Tested: working-styles/nf/working-style.md

Overall: [PASS/FAIL]
```

## Debugging Common Issues

### Symlink shows "No such file or directory"
- Symlink target path is wrong
- nf-ai-working-styles repo not cloned or not at expected location
- Fix: Update symlink with correct path
  ```bash
  rm working-styles
  ln -s /correct/path/to/nf-ai-working-styles/working-styles ./working-styles
  ```

### "ls working-styles/" shows no directories
- Symlink target is empty or wrong
- nf-ai-working-styles repo not fully set up
- Fix: Verify nf-ai-working-styles has working-styles/nf/ directory

### Changes to working-styles/ not appearing
- working-styles/ is copied, not symlinked
- Fix: Remove copied directory, create symlink instead
  ```bash
  rm -rf working-styles
  ln -s /path/to/nf-ai-working-styles/working-styles ./working-styles
  ```

### Git trying to commit working-styles/
- Missing or incorrect .gitignore entry
- Fix: Add to .gitignore:
  ```
  # AI agent working styles (symlinked)
  working-styles/
  ```

### CLAUDE.md working styles section conflicts with project content
- Section placed incorrectly
- Fix: Move section to appropriate location using Edit tool
- Working styles section should be near top, before project-specific content

## Path Determination

When user asks to bootstrap from different locations:

**User in project root:**
```bash
PROJECT_PATH=$(pwd)
# Create symlink relative or absolute
ln -s /absolute/path/to/nf-ai-working-styles/working-styles ./working-styles
```

**User in parent directory:**
```bash
# Ask which subdirectory to bootstrap
cd project-directory
# Then proceed with bootstrap
```

**User asks to bootstrap multiple projects:**
```bash
# Process one at a time
# For each project:
#   1. cd into project
#   2. Run bootstrap for that scenario
#   3. Verify
#   4. Move to next
```

## Important Notes

1. **Never overwrite existing CLAUDE.md** - Always merge working styles section carefully
2. **Always append to .gitignore** - Never replace entire file
3. **Use absolute paths in symlinks** - Prevents issues when working directory changes
4. **Verify after every bootstrap** - Catch issues immediately
5. **Ask before destructive operations** - Removing existing files, overwriting content
6. **Handle conflicts gracefully** - If working-styles/ exists, ask user what to do

## Quick Reference

**Bootstrap new project:**
```bash
ln -s /path/to/nf-ai-working-styles/working-styles ./working-styles
# Copy claude-template.md to CLAUDE.md
# Add working-styles/ to .gitignore
# Verify
```

**Verify existing installation:**
```bash
ls -la working-styles  # Check symlink
readlink working-styles  # Check target
grep "Working Styles" CLAUDE.md  # Check CLAUDE.md
grep "working-styles" .gitignore  # Check gitignore
ls working-styles/  # Test access
```
