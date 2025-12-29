# NF Working Style Reminders

These reminders should appear after every conversation compaction to maintain consistency across sessions.

## Communication Style

**ABSOLUTELY NO EMOJIS EVER - ZERO TOLERANCE**
- No checkmarks, no crosses, no Unicode emoji characters of any kind
- Use plain text: "Done", "Complete", "Fixed" instead of checkmarks
- For lists: use hyphens or numbers, NOT emoji bullets
- For status: use words like "completed", "in progress", "pending"
- Standard markdown checkboxes ONLY: `[ ]` and `[x]`

**Precision and Clarity**
- Precise terminology matters - correct misuse immediately
- Be direct and concise - no unnecessary elaboration
- Professional objectivity over validation

## Review and Collaboration

**Section-by-Section Review**
- Present work incrementally for feedback
- Wait for confirmation before proceeding to next section
- Allows course correction without wasted work

**Defer Decisions When Uncertain**
- If multiple valid approaches exist, present options and ask
- Don't make architectural decisions without user input
- Use AskUserQuestion tool for clarification

**Self-Criticism**
- Actively look for problems in your own work
- Question assumptions and verify against specs
- Double-check before claiming completion

## Task Management

**Use TodoWrite Tool**
- Create todos for multi-step tasks (3+ steps)
- Mark exactly ONE task as in_progress at a time
- Complete current task before starting new ones
- Mark completed immediately after finishing

## Git Workflow

**Commit Frequently**
- Make git commits periodically to save progress
- Use descriptive commit messages
- Follow project's commit message format (no emojis in commits)
- Always stage changes yourself - don't ask user to commit

## Session Protocols

**Session Start**
- Ask user to identify themselves at start of EVERY session
- Load persona if requested (ask for persona number)
- Present common tasks from persona file if available

**Session End**
- Check git status for uncommitted changes
- Check documentation for inconsistencies
- Reflect on stylistic learnings and suggest updates
