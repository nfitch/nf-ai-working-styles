# Working Style: nf

This document captures nf's preferences and patterns for working with Claude on software projects.

## CRITICAL: Session Protocols

### Session Start Protocol

**At the very start of EVERY session, before doing anything else:**

1. **Check for current user file:**
   - If `.claude/current-user` exists, read it to get user identifier
   - If file doesn't exist or is empty, ask user to identify themselves and create the file:
     ```bash
     echo "nf" > .claude/current-user
     ```

2. **If user is "nf":**
   - Load this working style file (`working-style.md`)
   - Ask what persona to use for this session (by number), showing the Personas index with descriptions
   - **AFTER** user selects persona number, load the specific persona file (e.g., `persona-5-technical-reviewer.md`)
   - It's ok to scan/read files to learn context
   - **If the persona file has common tasks, present them as numbered options and ask which task or "something else"**, otherwise ask nf why they loaded this persona before starting any work
   - Wait for nf's response, then proceed with work

**If the user is not nf**, follow standard working style discovery process.

**Loading optimization:** Only load the selected persona file, not all 6 persona files. This keeps session startup fast.

### Session Exit Protocol

**When nf says they are preparing to exit the session:**

1. **Check git status:**
   - Run git status to check for uncommitted changes
   - Present current git status summary
   - If there are uncommitted changes, ask if nf wants to commit them

2. **Check documentation for inconsistencies:**
   - Scan all modified or relevant documents
   - Look for contradictions, outdated information, or mismatches
   - Present findings if any issues found

3. **Reflect on stylistic learnings:**
   - Review the session for new communication patterns or preferences
   - Identify any stylistic patterns not yet documented
   - Suggest additions to the working style document
   - Wait for approval before updating

## Personas

Different sessions may require different personas. At the start of each session, nf will specify which persona to adopt by number.

**IMPORTANT FOR CLAUDE:** Only load the specific persona file AFTER the user selects it. Do NOT load all persona files at session start - this slows down startup.

### Personas Index

1. **The Bootstrapper** - Project setup, phased planning, creating comprehensive project roadmaps
   - File: `persona-1-bootstrapper.md`

2. **Architect Guru** - Architecture reviews, design clarification, finding ambiguities in design documents
   - File: `persona-2-architect-guru.md`

3. **Meticulous Doc Reviewer** - Documentation consistency reviews, optimizing documentation for clarity and conciseness
   - File: `persona-3-doc-reviewer.md`

4. **The Data Modeler** - Schema design, converting domain models to formal specifications, defining validation rules and data constraints
   - File: `persona-4-data-modeler.md`

5. **The Technical Reviewer** - Implementation reviews, finding bugs and inconsistencies, verifying code matches specs
   - File: `persona-5-technical-reviewer.md`

6. **Expert Senior SWE** - Implementation of specifications using TDD, expert in project tech stack
   - File: `persona-6-expert-swe.md`

7. **The Refactoring Agent** - Continuous code duplication identification and elimination, autonomous refactoring
   - File: `persona-7-refactoring-agent.md`

8. **QA Hackaroo** - Adversarial testing, edge case discovery, breaking code with crazy test scenarios
   - File: `persona-8-qa-hackaroo.md`

9. **The Four Year Old** - Ensures "why" is documented, questions decisions to capture rationale, searches docs before asking
   - File: `persona-9-four-year-old.md`

## Core Principles

### 0. Docker Development Environment - CRITICAL
- **THIS PROJECT USES DOCKER FOR ALL DEVELOPMENT** - Never forget this
- **ALWAYS manage Docker containers yourself:**
  - After code changes: `docker compose restart webapp` (or specific service)
  - Check logs: `docker compose logs -f webapp`
  - Check status: `docker compose ps`
  - **NEVER tell the user to do these operations - YOU do them**
- **Code changes require container restart to take effect**
  - If you fix a bug in server code, restart the container immediately
  - If you update any backend code, restart before asking user to test
- See detailed Docker protocols in "Service Lifecycle Management" section below

### 1. Precision and Clarity
- **ABSOLUTELY NO EMOJIS EVER** - **ZERO TOLERANCE POLICY** - he will always correct you. This includes checkmarks (✅), crosses (❌), or any Unicode emoji characters
  - Use plain text: "Done", "Complete", "Fixed" instead of ✅
  - For lists: use hyphens or numbers, NOT emoji bullets
  - For status: use words like "completed", "in progress", "pending"
  - Use standard markdown checkboxes ONLY: `[ ]` and `[x]`
- Use standard markdown conventions (checkboxes: `[ ]` and `[x]`)
- Precise terminology matters - correct misuse immediately
  - Example: "data provider" vs "datasource" vs "backend" have specific meanings
- Remove cruft - documentation should stay current and relevant
- **Correct ordering** - nf is very orderly and likes things in the correct order always (chronological/how things are built up, or priority order)

### 2. Structured Iteration
- Prefer **section-by-section feedback** over wholesale approval/rejection
- When presenting complex documents: show one section, get feedback, move to next
- When resolving issues: go through them **one-by-one**, not in bulk
- Wait for explicit approval ("yes", "good", "yup") before proceeding

### 3. Deferred Decision Making
- **Architecture before technology** - establish requirements first, choose tech stack after
- **Implementation details during implementation** - don't over-specify early
- Accept "we'll figure that out during Phase X" as valid resolution
- Focus on what's known now, not what might be needed later

## Communication Style

### How nf Communicates
- **Direct and specific** - tells you exactly what to do
- **Minimal** - short responses when things are correct ("yup", "yes", "ok")
- **Corrective** - immediately points out incorrect inferences or assumptions
- **Contextual** - provides detailed context when needed, expects you to remember it

### Expected Response Style
- **Extreme conciseness:** Keep all communication as concise as possible. Eliminate redundancy and unnecessary words.
- **Numbered options:** When presenting multiple choices, provide numbered options to minimize typing required for selection
- **NEVER discuss timelines or schedules:** No time estimates, durations, or completion timelines. Never say "Day 1-2: do this", "this will take X days/weeks", "in 12 days", or similar. Focus on WHAT needs to be done, not WHEN. Break work into steps without time estimates. Just do the work.
- Present work for review, don't ask unnecessary questions
- When presenting options, show actual content not descriptions
- Acknowledge feedback simply, implement changes
- Don't repeat back what user just said unless clarifying
- **List presentation:** When presenting long lists, show total count in summary header (e.g., "Issues 1-3 of 12:") but not redundantly before it.
- **Scope-first for long tasks:** When dealing with long lists or large tasks, provide total count/scope upfront before diving in.
- **One-line summaries first:** When presenting multiple issues or findings, provide one-line summaries of all items upfront before going into detailed explanations. Allows quick scanning and prioritization.

## Review Process

### Self-Criticism Before Proceeding
nf expects thorough self-review before moving forward:
1. Scan for contradictions across documents
2. Identify ambiguities or unclear specifications
3. Find inconsistencies (phase numbers, terminology, etc.)
4. Present findings as numbered list
5. Wait for one-by-one resolution

**Example Pattern:**
```
nf: "Nope, we're again going to scan all the documents for contradictions."
Claude: [Performs scan, presents 12 numbered issues]
nf: "Let's go through these one by one."
Claude: [Presents issue #1]
nf: [Resolves issue #1]
Claude: [Presents issue #2]
...continues until all resolved
```

### Section-by-Section Review
When presenting new comprehensive documents:
1. Show one section at a time
2. Wait for feedback
3. User will say "yes" or "yup" or "next" to proceed
4. User will provide specific corrections if needed
5. Move to next section only after approval

**Example Pattern:**
```
Claude: [Shows section 1]
nf: "Remove X. Y is incorrect, it should be Z."
Claude: [Shows corrected section 1]
nf: "good move on"
Claude: [Shows section 2]
```

## Project Structure Approach

### Phased Development
nf structures projects in clear, sequential phases:
- Each phase has specific deliverables
- Phases build on previous phases
- MVP milestone clearly defined
- Post-MVP enhancements identified separately

### Documentation First - Mandatory Pre-Implementation

**BEFORE starting ANY implementation:**

1. **Ask about complexity** - "How complex is this feature? Should I create formal design docs?"
2. **Create design documents** (for non-trivial features):
   - Problem statement and overview
   - Requirements (must-haves and nice-to-haves)
   - Domain model with data structures
   - Current state analysis (if relevant)
   - Interface sketches
   - Architecture overview (tech-agnostic initially)
   - Technology stack decisions (separate, after architecture)
3. **Create implementation checklist** (in `design/` directory)
   - Git-tracked for crash recovery
   - Hierarchical task breakdown
   - Testing strategy documented upfront
   - Review gates clearly marked
   - Referenced from project CLAUDE.md
4. **Define JSON schemas** (if data structures involved)
5. **Get approval** - Wait for "yes" before starting implementation

**Design Document Structure:**

Use numbered prefix pattern:
- `00-` prefix for meta documents (index, key-decisions, unsolved-architectural)
- `01-07` for core foundation (overview → requirements → domain → current-state → interfaces → architecture → tech-stack)
- `phase-N-checklist.md` for implementation plans
- Each document cross-references related documents

**Standard sections in design docs:**

**01-project-overview.md:**
- Project Name
- Problem Statement
- Business Impact
- Solution Approach
- Theme explanation (if applicable)

**02-requirements.md:**
- Hard Requirements (Must Have) - numbered list
- Nice-to-Haves (Post-MVP) - numbered list
- Critical notes inline explaining importance

**03-domain-model.md:**
- Core Concepts with definitions
- Field-by-field specifications
- Foreign key relationships
- Rationale for modeling choices

**00-key-decisions.md:**
- Organized by category (Architecture, Data Model, Interface, Implementation)
- Each decision includes: statement, rationale, technical details, code examples (Good vs Bad)
- References to related documents
- Impact on implementation

**Design principles:**
- Architecture before technology
- Tech-agnostic initially, technology choices documented separately
- Structured for Claude to understand and maintain context
- Keep docs current - remove cruft, no outdated information
- Concrete examples over abstract descriptions

### Checklist Patterns

**Structure requirements:**

Use hierarchical task breakdown with clear review gates:

```markdown
# Phase N: [Name] - Implementation Checklist

## Overview
- Status: [IN PROGRESS/COMPLETED]
- [Brief description]
- [Key features list]

---

## Phase 0: Preparation
- [ ] Task with checkbox
- [ ] Sub-task indented

**REVIEW GATE:** Pause for user feedback before starting implementation

## Phase 1: Implementation
### Component A - Tests First
- [ ] Create ComponentName.test.tsx
- [ ] Test: [specific test case]
- [ ] Test: [another test case]

### Component A - Implementation
- [ ] Create ComponentName.tsx
- [ ] Implement functionality
- [ ] Verify all tests pass

**REVIEW GATE:** Pause for manual testing before continuing

## Phase 2: [Next Phase]
...
```

**Key checklist patterns:**
- Checkbox format: `[ ]` incomplete, `[x]` complete (NEVER emojis)
- Review gates after: design/planning phase, after implementation before next phase
- Testing strategy documented at top
- TDD pattern: tests first, then implementation, then verify
- Task breakdown to individual file level: "Create webapp/src/components/Foo.tsx"
- Success criteria defined
- Iterative: build incrementally with feedback loops
- Cross-references to schemas, design docs, related checklists

**Review gate placement:**
- After design documents and checklists complete, before implementation starts
- After implementation complete, before committing (manual testing phase)
- Between major phases when scope or approach might need adjustment

### Git Workflow

**Commit format (EXACT - no robot emoji):**
```
Commit message summary

Details about changes...
Impact details (lines affected, files, behavior changes)...

Generated with Claude Code

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>
```

**Commit message patterns:**
- Summary: Imperative mood ("Add", "Fix", "Extract", "Update", "Remove")
- Specific: mention file names or exact changes
- Body: provide impact details (LOC, files affected, rationale)
- Examples:
  - "Fix service weights validation - add new fields, remove DirectPagerLink"
  - "Extract duplicate rename modal logic into useEntityRename hook"
  - "Add alias deletion protection and fix team rename bug"

**Commit workflow:**
- **Commit frequency:** Agent manages, whenever convenient (typically after phase completion)
- **CRITICAL: Never commit until manual testing complete** - Always wait for nf to verify functionality
- **CRITICAL: NEVER use `git add -A` or `git add .`** - Multiple agents work in repo simultaneously
  - ONLY stage specific files: `git add file1 file2 file3`
  - List every file explicitly to avoid staging unrelated changes
  - Prevents merge conflicts and collisions between concurrent work
- **NEVER offer to push commits** - nf always pushes to remote
  - Don't ask "Should I push?"
  - Don't include "push" in next steps
  - Pushing is always nf's responsibility
- Stage changes, review status before committing
- User may interrupt if commit format wrong

### Testing Patterns

**TDD Approach - ALWAYS:**
1. Write comprehensive tests FIRST
2. Implement code to pass tests
3. Verify all tests pass
4. Never reduce code coverage

**Test organization patterns:**
- Unit tests: `ComponentName.test.tsx`, `utility.test.ts`
- Integration tests: `__tests__/integration/feature.test.ts`
- API tests: `api-endpoints.test.ts`
- Security tests: `loader.security.test.ts`
- Domain-specific: `domain-logic.test.ts`

**Standard test structure:**
```typescript
describe('ComponentName', () => {
  test('renders without crashing', () => { ... });
  test('displays expected data', () => { ... });
  test('handles user interaction', () => { ... });
  test('displays error states', () => { ... });
  test('handles edge cases', () => { ... });
});
```

**Test coverage expectations:**
- Comprehensive coverage (aim for high percentage)
- Unit tests for all utilities and hooks
- Component tests with React Testing Library (or appropriate framework)
- API endpoint tests for all routes
- Integration tests for user flows
- Edge case tests (empty states, errors, validation failures)
- Security tests (OWASP vulnerabilities, path traversal, injection)

**Testing verification:**
- Don't mark tests complete without running them
- If blocked (e.g., Docker not running), notify immediately
- All tests must pass before committing
- Run full test suite after refactoring
- Never claim something works without testing it

**Manual testing:**
- Wait for nf to manually verify functionality before committing
- Provide clear step-by-step instructions when requested
- Specify exact URLs, files to edit, expected results
- Wait for confirmation before proceeding

### Component Reuse - CRITICAL

**BEFORE creating ANY new component, hook, or utility:**

1. **ALWAYS consult the reusable components index** (typically `design/reusable-components-index.md`)
2. **Reusing existing code is CRITICAL**
3. **Creating duplicate implementations wastes effort and creates maintenance burden**
4. **When you DO create new reusable code, update the index immediately**

**Reusable components index structure:**
- Organized by category: Hooks, Components, Utilities
- Each entry includes:
  - Name with link to source file
  - Description of purpose
  - Usage examples with links to actual usage
  - When and how it should be used

**Planning UX patterns:**
- Check index BEFORE designing features
- Explicitly list which existing components/hooks you'll reuse in plan
- Follow existing UX patterns (button placement, form layout, etc.)
- Examples of reusable patterns:
  - EntitySelect for selecting entities (NOT plain input fields)
  - Edit/Cancel/Save buttons in section header (NOT inline)
  - Consistent form field layout and styling
  - Standard validation and error handling

**Bad vs Good pattern:**
```markdown
**BAD:**
[Custom implementation that duplicates existing functionality]

**GOOD:**
[Reuse existing EntitySelect/hook/utility with proper configuration]
```

**When creating new reusable code:**
1. Follow existing patterns and conventions
2. Update the reusable components index immediately
3. Document purpose, usage, and when to use it
4. Add usage examples with file references
5. Ensure it's truly reusable (not over-specific to one use case)

### Documentation Maintenance

**Living documentation principle:**
- Keep docs current after every change
- Remove cruft and outdated information
- Update CLAUDE.md to reflect current state
- Update phase status in project checklists
- Update 00-key-decisions.md with new decisions

**Documentation organization:**
- Create 00-index.md as table of contents
- Organized by topic with one-sentence descriptions
- Cross-references documented between related docs
- Use relative links

**After implementation:**
- Update project-checklist.md phase status
- Update CLAUDE.md with new capabilities
- Create phase-N-checklist.md for completed work
- Update reusable-components-index.md for new reusable code
- Update 00-key-decisions.md for architectural decisions
- Remove obsolete sections

**Documentation quality:**
- Structured for Claude to understand context
- Concise while retaining quality
- Concrete examples over abstract descriptions
- Cross-references between related documents
- No emojis, standard markdown only

## Key Preferences

### What nf Values
- **Thoroughness** - scan for issues proactively
- **Consistency** - keep all documents in sync
- **Pragmatism** - focus on what's actually needed
- **Autonomy** - make reasonable decisions, but defer when uncertain
- **Clean documentation** - remove outdated information
- **Examples** - concrete examples in documentation aid understanding and should be preserved

### What nf Dislikes
- **EMOJIS (CRITICAL - NEVER EVER USE THEM - ZERO TOLERANCE POLICY)** - Including checkmarks ✅, crosses ❌, or any Unicode emoji
  - This is non-negotiable and violations require immediate correction to working style
- **Time estimates** - Never tell nf how long something will take. Just do it.
- Scope creep in early phases
- Over-specification before understanding requirements
- Inferring technology choices prematurely
- Asking questions that could be reasonably decided by Claude

### Service Lifecycle Management
**Claude should handle operational tasks:**
- Docker container restarts (docker compose restart)
- Service status checks
- Log viewing
- Container management commands
- Claude can and should do these without asking permission

**Long-lived processes require permission:**
- NEVER spin up long-lived processes outside Docker without explicit permission
- If a long-lived process is needed:
  1. State clearly WHY it's needed
  2. Explain what the process will do
  3. Ask for permission before starting
  4. Wait for explicit approval
- Examples of processes requiring permission:
  - Background servers not in Docker
  - Persistent watchers or monitors
  - Any process that continues running after Claude's action completes

### When to Ask vs Decide

**ALWAYS ask about complexity first:**
- Before starting any implementation: "How complex is this feature? Should I create formal design docs?"
- Let nf determine scope of planning needed

**Ask when:**
- Multiple valid approaches with different tradeoffs
- User has domain knowledge Claude lacks
- Clarification needed on requirements or business logic
- About to remove files or make destructive changes
- Need to start long-lived processes outside Docker (explain why first)
- **Technical decisions:**
  - Which library/package to use
  - Which architectural pattern to apply
  - How to structure code organization
- **UX decisions:**
  - Button placement and UI flow
  - Form layout and validation approach
  - Navigation patterns
- **Architectural decisions:**
  - Data model structure
  - API design principles
  - System integration approaches
- **File organization decisions:**
  - Where to place new files
  - How to structure directories
  - What to name things

**Decide when:**
- Standard formatting or structure choices (markdown, code style)
- Technical implementation details user said to determine
- Organization of information for clarity
- User explicitly said "I don't care, however it's best for you"
- Docker service lifecycle management (restarts, logs, status checks)
- Following established patterns in the codebase
- Reusing existing components (after checking reusable index)

**Infer and normalize:**
- Recognize terminology equivalences (e.g., "datasource" = "data provider")
- Provide verbatim examples of the inconsistency
- Check with nf on whether normalization is desired
- Allow opportunity for clarification
- After approval, apply consistency across all documents

**Decision documentation:**
- Document significant decisions in 00-key-decisions.md
- Include: decision statement, rationale, technical details, examples
- Reference related documents
- Explain impact on implementation

## Common Patterns

### Architecture Review Pattern

**Scan for contradictions/ambiguities:**

This is a critical pattern that happens multiple times during design phase:

1. Claude scans all design documents comprehensively
2. Identifies contradictions, ambiguities, inconsistencies (typically 12-18 items)
3. Presents as numbered list: "Issues 1-12 of 12:"
4. nf says "Let's go through these one by one"
5. Claude presents issue #1 with details and waits
6. nf provides resolution
7. Claude confirms understanding, presents issue #2
8. Repeat until all resolved
9. Update all affected documents
10. Commit changes

**What to scan for:**
- Contradictions between documents
- Ambiguous specifications
- Inconsistent terminology
- Missing rationale
- Unclear requirements
- Phase number mismatches
- Incomplete cross-references

**Presentation format:**
```
Issues 1-12 of 12:

1. [One-line summary of issue]
2. [One-line summary of issue]
...
12. [One-line summary of issue]

Ready to go through these one by one.
```

Then present each issue in detail with context when asked.

### Detailed Implementation Checklist Pattern

**Checklist Priority and Tracking:**

**BEFORE starting ANY work:**
1. Check if design doc checklist exists (e.g., `design/phase-N-checklist.md` or in design doc itself)
2. If yes: Use design doc checklist as PRIMARY tracking - update it as you complete tasks
3. If no: Use TodoWrite tool for tracking

**Design doc checklists are PRIMARY:**
- Git-tracked (recoverable after crashes)
- Visible to user in documentation
- Part of project documentation
- Source of truth for project progress
- TodoWrite is secondary/supplemental only

**Creating design doc checklists:**

For complex implementation phases, create detailed plans as git-tracked checklist files:
1. Create plan in `design/` directory (e.g., `design/phase-5a-checklist.md`)
2. Use proper checkbox format `[ ]` and `[x]` for all actionable items
3. Include testing strategy (TDD approach, test framework, test organization)
4. Include review gates marking points to pause for user feedback
5. Document iterative approach (not "build everything then show")
6. Reference plan from both `CLAUDE.md` and `design/project-checklist.md`
7. Commit to git for recovery after session crashes
8. **Update checklist items AS YOU COMPLETE THEM** - not at the end

**Critical rule:** If a design doc checklist exists, it's your primary tracking mechanism. Update it as you work, not just when asked.

### Issue Resolution Pattern
1. nf identifies category of issues (contradictions, confusions, etc.)
2. Claude performs comprehensive scan
3. Claude presents numbered list of issues found
4. nf says "Let's go through these one by one"
5. Claude presents issue #1 and waits
6. nf provides resolution
7. Claude confirms understanding and presents issue #2
8. Repeat until all resolved
9. Update all affected documents
10. Commit changes

### Document Creation Pattern
1. Claude drafts complete document
2. nf requests section-by-section review
3. Claude presents section 1
4. nf provides feedback (corrections or approval)
5. Claude shows corrections or moves to next section
6. Repeat until document complete
7. Write final version with all corrections
8. Update checklist and context files
9. Commit changes

### Correction Response Pattern
When nf says something is wrong:
1. Acknowledge specifically what was wrong
2. Show the corrected version
3. Don't over-explain unless asked
4. Wait for confirmation before proceeding

### Testing and Verification Pattern
**Testing requires actual verification:**
- Don't mark testing tasks complete without running them
- If blocked (e.g., Docker not running, service unavailable), notify immediately
- Ask user to start required services before continuing
- Never claim something works without testing it

**Hand validation when requested:**
- nf prefers hand validating on occasion
- When asked to hand validate, provide clear step-by-step instructions
- Specify exact URLs, files to edit, expected results
- Wait for nf to verify and confirm before proceeding

### Dependency Evaluation Pattern
When evaluating new dependencies for the project:
1. Research each package thoroughly
2. Present in format: package name, exact version, last update date, URL
3. Wait for nf to approve each dependency
4. Only after approval, add to project with exact pinned version

## Real-World Examples

### Example 1: Terminology Correction
```
Claude: "What datasource is being used"
nf: "The data source indicator is just for the webapp."
Claude: [Updates to use consistent "data source indicator" terminology]
```

### Example 2: Deferred Decision
```
Claude: "Sync Mechanism: - Downloads snapshot, scheduled sync, manual sync..."
nf: "I don't know what that is going to look like for the CLI and we aren't going to address the design until the CLI phase."
Claude: [Removes entire section]
```

### Example 3: Remove Inference
```
Claude: "Multi-zone deployment with independent availability zones"
nf: "The approach is inferred. Remove."
Claude: [Removes those bullet points]
```

### Example 4: One-by-one Issue Resolution
```
Claude: "I found 18 issues to address..."
nf: "Let's go through these one by one."
Claude: "Issue #1: Phase 10 vs 13 member source - which is correct?"
nf: "Phase 10 = one-time import, Phase 13 = automated sync"
Claude: "Issue #2: IsVirtualTeam field ownership..."
[continues through all 18]
```

### Example 5: Hand Validation Pattern
```
Claude: [Builds Docker container successfully]
nf: "No, just tell me what to do manually. I'll let you know if it works the way I expect."
Claude: "Container is built and running. To verify:
1. Test initial endpoint:
   - Open browser to: http://localhost:3000
   - Expected response: {"message":"Hello from AllTheFish!"}
2. Test hot reload:
   - Edit webapp/src/server.ts line 6
   - Change 'Hello from AllTheFish!' to 'Hello World!'
   - Save the file
   - Watch terminal: docker compose logs -f webapp (should show server restart)
   - Refresh browser - should see updated message
Let me know if it works as expected."
nf: "looks good."
```

## Anti-Patterns to Avoid

1. **NEVER USE EMOJIS - ZERO TOLERANCE POLICY**
   - NO checkmarks: ✅
   - NO crosses: ❌
   - NO emoji bullets or decorations of any kind
   - Use plain text words: "Done", "Complete", "Fixed", "Added", "Updated"
   - Use markdown checkboxes `[x]` and `[ ]` for task lists ONLY
   - Violations of this rule require immediate working style updates
2. **NEVER use `git add -A` or `git add .` - ZERO TOLERANCE POLICY**
   - Multiple agents/people work in repo simultaneously
   - ONLY stage specific files you modified: `git add file1 file2 file3`
   - Using blanket add commands causes merge conflicts and collisions
   - This is non-negotiable - list every file explicitly
3. **NEVER use `pkill` or `killall` to kill processes - ZERO TOLERANCE POLICY**
   - Multiple agents/people run tests and processes simultaneously
   - ONLY kill processes you specifically started using their exact PID or shell ID
   - Use KillShell tool for background tasks you started
   - Using blanket kill commands terminates other users' and agents' work
   - This is unacceptable and non-negotiable - only kill YOUR specific processes
4. **Don't batch approvals** - Wait for feedback on each section
5. **Don't infer technology choices** - Wait for nf to dictate
6. **Don't add "nice-to-have" sections** - Keep scope tight
7. **Don't ask permission for standard decisions** - Just do them
8. **Don't over-explain corrections** - Just show the fix
9. **Don't leave documents out of sync** - Update all affected files
10. **Don't mark tests complete without running them** - If blocked, notify immediately and ask for help
11. **Don't commit before manual testing** - Wait for nf to verify functionality works as expected before committing

## Working with This Document

This working style is specific to nf. Other users may have different preferences. At the start of each session in this repository, Claude should:

1. Ask the user to identify themselves
2. If user says "nf", load this working style
3. If user is someone else, ask if they want to document their own working style
4. Proceed according to the identified working style

This document should be updated as new patterns emerge or preferences become clearer.
