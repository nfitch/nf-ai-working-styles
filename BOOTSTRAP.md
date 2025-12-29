# Bootstrap Instructions for Claude

**AUDIENCE:** This file contains instructions for Claude (AI agent) on how to bootstrap the working styles pattern into a project or verify an existing installation.

## Overview

The working styles pattern consists of:
1. Symlinked `working-styles/` directory in the root of a target repository
2. Minimal working styles section in project's `CLAUDE.md`
3. `.gitignore` entry for the symlinked directory
4. User-level permissions in `~/.claude/settings.json` to allow reading symlinked directory
5. Claude Code hooks in `.claude/hooks/` for post-compaction reminders
6. User identifier in `.claude/current-user` file

## CRITICAL: Explain Plan and Get User Approval First

**BEFORE starting any bootstrap implementation:**

1. **Explore and understand** what needs to be done:
   - Read this BOOTSTRAP.md file
   - Check the current state of the target repository
   - Determine which scenario applies (A, B, or C)

2. **Explain the plan to the user**:
   - Tell them which scenario you identified
   - List all steps you will take (be specific)
   - Explain what each step does, especially:
     - Modifying ~/.claude/settings.json (user-level configuration)
     - Creating symlinks (what they point to)
     - Creating/modifying files in their repository

3. **Wait for user approval**:
   - Ask: "Does this approach sound good? Should I proceed?"
   - Only start implementation after user confirms

4. **Then proceed** with the bootstrap steps

**Why this is critical:**
- Users need to understand what will happen to their system
- ~/.claude/settings.json is a user-level configuration file
- Users may want to modify the plan or ask questions first
- This builds trust and ensures informed consent

## User-Level Permissions (Required for All Scenarios)

**CRITICAL:** Due to symlink limitations, you must configure user-level permissions FIRST.

**Add to `~/.claude/settings.json`:**

1. **Read the current settings**
   ```bash
   cat ~/.claude/settings.json
   ```

2. **Add additionalDirectories** (merge with existing content):
   ```json
   {
     "permissions": {
       "additionalDirectories": [
         "/absolute/path/to/nf-ai-working-styles/working-styles"
       ]
     }
   }
   ```

3. **If file already has permissions block, merge it**:
   ```json
   {
     "existingKey": "existingValue",
     "permissions": {
       "additionalDirectories": [
         "/absolute/path/to/nf-ai-working-styles/working-styles"
       ],
       "allow": [
         "existing rules..."
       ]
     }
   }
   ```

**Why this is required:**
- Symlinks point outside project directory tree
- Claude Code cannot read through symlinks without explicit permission
- User-level settings apply to all projects
- Project-level additionalDirectories does NOT work for symlinks (undocumented limitation)

**This step must be done once per user, not per project.**

---

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
     # Append if it doesn't already contain working-styles
     if ! grep -q "working-styles" .gitignore; then
       echo "" >> .gitignore
       echo "# AI agent working styles (symlinked)" >> .gitignore
       echo "working-styles" >> .gitignore
     fi
   else
     # Create new .gitignore
     cat > .gitignore << 'EOF'
# AI agent working styles (symlinked)
working-styles
EOF
   fi
   ```

5. **Setup Claude hooks**
   ```bash
   # Create .claude directory structure
   mkdir -p .claude/hooks

   # Copy hook template from nf-ai-working-styles
   STYLES_REPO="/path/to/nf-ai-working-styles"
   cp "$STYLES_REPO/templates/claude/hooks/post-compaction" .claude/hooks/
   cp "$STYLES_REPO/templates/claude/current-user.example" .claude/

   # Make hook executable
   chmod +x .claude/hooks/post-compaction

   # Create current-user file (replace 'nf' with actual identifier)
   echo "nf" > .claude/current-user
   ```

6. **Update .gitignore for Claude config**
   ```bash
   # Add to .gitignore (append if not present)
   if ! grep -q ".claude/current-user" .gitignore; then
     echo "" >> .gitignore
     echo "# Claude Code per-user configuration" >> .gitignore
     echo ".claude/current-user" >> .gitignore
     echo ".claude/settings.local.json" >> .gitignore
   fi
   ```

7. **Verify installation** (see Verification section below)

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

2. **Follow Scenario A steps** (symlink, create CLAUDE.md, setup hooks, update .gitignore)

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

5. **Setup Claude hooks** (same as Scenario A)

6. **Update .gitignore** (same as Scenario A, careful append)

7. **Verify installation**

## Verification Steps

After bootstrap, verify the installation is correct:

### 0. Check User-Level Permissions
```bash
cat ~/.claude/settings.json | grep -A 5 additionalDirectories
# Should show: "/path/to/nf-ai-working-styles/working-styles"
```

Verify:
- User-level settings file exists at `~/.claude/settings.json`
- Contains `permissions.additionalDirectories` array
- Array includes absolute path to nf-ai-working-styles/working-styles
- Path matches where nf-ai-working-styles is actually located

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

### 7. Check Claude Hooks Configuration
```bash
# Check .claude directory exists
test -d .claude && echo "PASS" || echo "FAIL"

# Check post-compaction hook exists
test -f .claude/hooks/post-compaction && echo "PASS" || echo "FAIL"

# Check hook is executable
test -x .claude/hooks/post-compaction && echo "PASS" || echo "FAIL"

# Check current-user file exists
test -f .claude/current-user && echo "PASS" || echo "FAIL"

# Display current user
cat .claude/current-user
```

### 8. Verify User Reminders File
```bash
# Determine current user
CURRENT_USER=$(cat .claude/current-user | tr -d '\n\r')

# Check reminders file exists for current user
test -f "working-styles/$CURRENT_USER/reminders.md" && echo "PASS" || echo "FAIL"
```

### 9. Test Hook Execution
```bash
# Run the post-compaction hook
./.claude/hooks/post-compaction
```

Expected output:
- Banner with "CRITICAL REMINDERS AFTER COMPACTION"
- User-specific reminders from working-styles/{user}/reminders.md
- Project-specific reminders (if design/project-reminders.md exists)
- No errors

All commands should succeed without errors.

## Verification Report Format

After running verification, provide report:

```
Working Styles Installation Verification

0. User-level permissions configured: [PASS/FAIL]
   ~/.claude/settings.json has additionalDirectories with correct path

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

7. Claude hooks configured: [PASS/FAIL]
   .claude/hooks/post-compaction exists and is executable

8. Current user configured: [PASS/FAIL]
   .claude/current-user contains: [username]

9. User reminders file exists: [PASS/FAIL]
   working-styles/{username}/reminders.md exists

10. Hook execution test: [PASS/FAIL]
    Hook displays reminders correctly

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

### Post-compaction hook not found
- .claude/hooks/ directory missing or hook not created
- Fix: Create the hook from template
  ```bash
  mkdir -p .claude/hooks
  # Copy hook template from nf-ai-working-styles
  cp /path/to/nf-ai-working-styles/templates/claude/hooks/post-compaction .claude/hooks/
  chmod +x .claude/hooks/post-compaction
  ```

### Hook not executable
- File permissions incorrect
- Fix: Make hook executable
  ```bash
  chmod +x .claude/hooks/post-compaction
  ```

### Hook displays "NO USER-SPECIFIC REMINDERS"
- .claude/current-user file missing or empty
- Fix: Create current-user file with your identifier
  ```bash
  echo "nf" > .claude/current-user
  ```

### Reminders file not found
- working-styles/{user}/reminders.md doesn't exist
- Fix: Create reminders file
  ```bash
  # File should exist in nf-ai-working-styles/working-styles/{user}/
  # and be accessible via symlink
  ls -la working-styles/nf/reminders.md
  ```

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
# Add working-styles to .gitignore
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
