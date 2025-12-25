# AI Agent Working Styles Pattern

## Problem Statement

Different users have different preferences for how they work with AI agents:
- Communication style (verbose vs concise)
- Review processes (section-by-section vs wholesale)
- Decision-making approaches (ask vs infer)
- Session protocols (start/exit procedures)
- Specialized personas for different tasks

Currently, these preferences must be re-explained in every project and every session, leading to:
- Wasted time re-establishing preferences
- Inconsistent experiences across projects
- No reusability of working style definitions
- Difficulty onboarding new collaborators with different styles

## Solution: Portable Working Styles Pattern

Create a reusable working styles system that can be linked into any project:

1. **Source repository** (nf-ai-working-styles): Contains working style definitions
2. **Symlink pattern**: Projects link to working-styles/ directory
3. **Minimal template**: Projects use lightweight CLAUDE.md that references linked styles
4. **Discovery mechanism**: AI agent discovers available styles by directory listing
5. **Session protocols**: Defined in working styles themselves, not project files

## Architecture

```
nf-ai-working-styles/              (source repository)
├── claude.md                       (instructions for working IN this repo)
├── claude-template.md              (template for projects to copy)
├── BOOTSTRAP.md                    (human setup instructions)
├── design/                         (design docs)
│   └── working-styles-pattern.md   (this file)
└── working-styles/                 (portable working styles)
    ├── README.md                   (explains the system)
    └── nf/                         (nf's working style)
        ├── working-style.md        (core style + session protocols)
        ├── persona-*.md            (9 specialized personas)
        └── infinite-loop-refactoring-agent.md

project-repo/                       (any project using the pattern)
├── CLAUDE.md                       (from template - minimal, project-specific)
├── .gitignore                      (ignores working-styles/)
└── working-styles/ -> /path/to/nf-ai-working-styles/working-styles/
```

## Components

### 1. claude-template.md
Minimal template for project CLAUDE.md files. Contains only:
- Session start: identify user, load from working-styles/{user}/
- Directory discovery mechanism
- Reference to working-styles/README.md
- Everything else is project-specific

### 2. BOOTSTRAP.md
Human-readable instructions for setting up the pattern in a project. Covers three scenarios:
- **Scenario A**: Brand new empty repository
- **Scenario B**: Existing repository with no CLAUDE.md
- **Scenario C**: Existing repository with existing CLAUDE.md (merge/append)

Includes:
- Symlink creation commands (handling different starting locations)
- .gitignore modifications (careful appending, not overwriting)
- Verification steps (symlink exists, CLAUDE.md structure, .gitignore correct)
- Debugging guidance

### 3. working-styles/README.md
Explains the working styles system itself:
- What working styles are
- How they work
- How to add new styles
- Session flow overview

### 4. working-styles/nf/
Complete nf working style (copied from allthefish):
- working-style.md: Core style, session protocols, personas index
- All 9 persona files
- infinite-loop-refactoring-agent.md (moved from allthefish root)

### 5. claude.md (for nf-ai-working-styles repo)
Meta-level instructions for working IN this repository:
- Project structure explanation
- Reference to this design doc
- Reference to BOOTSTRAP.md
- Note: no verification script needed (manual steps in BOOTSTRAP.md)
- Standard working styles session protocols apply

## Key Design Decisions

### Symlinks in .gitignore
When using symlinked working-styles/, add to .gitignore:
```
# AI agent working styles (symlinked)
working-styles/
```

This prevents committing the symlink while allowing each developer to point to their own nf-ai-working-styles location.

### Dynamic Style Discovery
Template uses directory listing to discover available styles:
```
Available working styles: {ls working-styles/}
```

This allows multiple users to coexist (nf, jane, bob, etc.) without template changes.

### Minimal Template Philosophy
Keep claude-template.md minimal - only entrance protocol. This is meta-programming the AI agent interaction layer, not technical implementation. Projects add their own:
- Technology choices
- Development environment
- Project-specific rules
- Git workflows

### Exit Protocols in Working Styles
Session exit protocols (git status check, doc scan, reflection) belong in individual working-style.md files, not the template. This allows different users to have different exit preferences.

### Bootstrap Safety
BOOTSTRAP.md emphasizes careful handling of existing repos:
- Check for existing .gitignore before appending
- Check for existing CLAUDE.md before creating/merging
- Provide clear verification steps
- Include rollback instructions

## Implementation Checklist

### Phase 1: Core Structure
- [x] Create design/ directory
- [x] Create this design doc
- [x] Create initial claude.md referencing design

### Phase 2: Copy Working Styles
- [x] Create working-styles/nf/ directory
- [x] Copy all files from allthefish/working-styles/nf/
- [x] Copy infinite-loop-refactoring-agent.md from allthefish root to working-styles/nf/
- [x] Verify all 12 files copied correctly

### Phase 3: Documentation
- [x] Create working-styles/README.md (explain system)
- [x] Create claude-template.md (minimal entrance protocol)
- [x] Create BOOTSTRAP.md (all 3 scenarios + verification)

### Phase 4: Finalization
- [x] Create .gitignore for nf-ai-working-styles
- [x] Update claude.md with complete structure and references
- [x] Review all files for consistency

### Phase 4b: Extract Comprehensive Patterns
- [x] Scan allthefish for design patterns, checklists, git history
- [x] Integrate comprehensive patterns into working-style.md
- [x] Add 378 lines of detailed guidance (design, testing, component reuse, etc.)

### Phase 5: Testing (with allthefish)
- [ ] Create backup of allthefish/working-styles/
- [ ] Symlink nf-ai-working-styles/working-styles into allthefish
- [ ] Update allthefish/.gitignore
- [ ] Test: start new session, verify working style loads from symlink
- [ ] Test: verify persona files accessible
- [ ] Test: verify agent can work normally with linked files

## Verification Steps

After bootstrap, verify:
1. Symlink exists: `ls -la working-styles/`
2. Symlink target correct: `readlink working-styles`
3. CLAUDE.md exists and has working styles section
4. .gitignore includes working-styles/
5. Can list available styles: `ls working-styles/`
6. Can read style files: `cat working-styles/nf/working-style.md`

## Success Criteria

- nf-ai-working-styles contains complete, portable working styles
- Projects can link to working styles without copying
- Single source of truth for working style definitions
- New projects can bootstrap in under 5 minutes
- Pattern works for multiple users in same project
- AI agents can discover and load styles automatically
