# AI Agent Working Styles System

## What Are Working Styles?

Working styles are personalized preference profiles that define how different users want to interact with AI agents. They capture:

- **Communication preferences**: Verbose vs concise, formal vs casual
- **Review processes**: Section-by-section vs wholesale review
- **Decision-making patterns**: When to ask vs when to infer
- **Session protocols**: How to start and end work sessions
- **Specialized personas**: Different modes for different tasks (implementation, review, refactoring, etc.)

## Why Working Styles?

Without working styles, preferences must be re-explained in every project and session, leading to:
- Wasted time establishing context
- Inconsistent experiences
- No reusability across projects

With working styles, AI agents automatically adapt to each user's preferences by loading their style at session start.

## How It Works

### For AI Agents (Claude)

At the start of each session:

1. **Identify the user** - Ask "Please identify yourself" or similar
2. **Discover available styles** - List directories in `working-styles/`:
   ```bash
   ls working-styles/
   ```
3. **Load the user's style** - Read `working-styles/{user-id}/working-style.md`
4. **Follow the protocols** - The working-style.md file contains:
   - Session start protocol (personas, task selection, etc.)
   - Session exit protocol (git status, doc checks, reflection)
   - Communication preferences
   - Review processes
   - All other user-specific preferences

### For Users (Humans)

When you want to work with an AI agent that supports working styles:

1. The project's CLAUDE.md will prompt the agent to ask for your identity
2. Respond with your user ID (e.g., "nf", "jane", "bob")
3. The agent loads your working style and adapts to your preferences
4. Work proceeds according to your defined protocols

### Directory Structure

```
working-styles/
├── README.md           (this file)
└── {user-id}/          (one directory per user)
    ├── working-style.md (core preferences and protocols)
    └── persona-*.md     (optional specialized personas)
```

## Available Working Styles

Current working styles in this repository:

- **nf**: Concise communication, section-by-section review, deferred decisions, 9 specialized personas

## Creating Your Own Working Style

To add your own working style to a project:

1. Create a directory with your identifier: `working-styles/{your-id}/`
2. Create `working-style.md` in that directory documenting:
   - How you prefer to communicate
   - Your review process preferences
   - When you want to be asked vs when agent should decide
   - Session start protocol (if you use personas or other patterns)
   - Session exit protocol (git checks, reflections, etc.)
3. Add any additional files (personas, checklists, etc.) as needed

The AI agent will automatically discover your style directory and offer it as an option.

## Session Flow Example

1. **Session starts**
   ```
   Agent: "Please identify yourself"
   User: "nf"
   ```

2. **Agent discovers and loads**
   ```
   Agent: (reads working-styles/nf/working-style.md)
   Agent: (follows nf's session start protocol)
   Agent: "Which persona would you like to use? 1. Bootstrapper, 2. Architect Guru..."
   User: "6"
   ```

3. **Agent loads persona and works**
   ```
   Agent: (reads working-styles/nf/persona-6-expert-swe.md)
   Agent: (follows TDD workflow, writes tests first, etc.)
   ```

4. **Session ends**
   ```
   User: "I'm preparing to exit"
   Agent: (follows nf's exit protocol: git status check, doc scan, reflection)
   ```

## For Project Maintainers

To use this working styles system in your project:

1. **Symlink** this directory into your project:
   ```bash
   ln -s /path/to/nf-ai-working-styles/working-styles ./working-styles
   ```

2. **Add to .gitignore**:
   ```
   # AI agent working styles (symlinked)
   working-styles/
   ```

3. **Update project CLAUDE.md** to include session start:
   ```markdown
   ## Working Styles

   At the start of each session:
   1. Ask the user to identify themselves
   2. List available styles: `ls working-styles/`
   3. Load their working style from `working-styles/{user-id}/working-style.md`
   4. Follow the protocols defined in their working style

   See working-styles/README.md for more information.
   ```

See the project's BOOTSTRAP.md for detailed setup instructions.

## Benefits

- **Consistency**: Same preferences across all projects
- **Efficiency**: No re-explaining preferences each session
- **Portability**: Single source of truth, linked into projects
- **Collaboration**: Multiple users with different styles in same project
- **Discoverability**: Agent automatically discovers available styles
- **Extensibility**: Easy to add new styles or personas
