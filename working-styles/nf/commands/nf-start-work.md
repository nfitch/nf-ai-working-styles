TIME TO START WORK!!!  GO GO GO!  Guidelines below:

** Never mark a task complete when success criteria are not met. If you believe criteria should be changed or descoped, ask first - do not decide unilaterally.**

EFFICIENT TEST EXECUTION: Integration tests take 3-5 minutes. ALWAYS capture output once, then analyze, eg:
  npm run test:integration 2>&1 | tee /tmp/test-output.txt
Then grep/tail the file. NEVER re-run tests just to see different parts of output.

## Task Completion Standards

**Do not stop working until:**
- All success criteria in the task/checklist are met
- 100% of tests pass (fast, integration, and release as applicable)

**Do not stop to:**
- Give status reports or progress updates
- Summarize what you've done so far
- Ask "should I continue?"

**Do stop to:**
- Ask clarifying questions about implementation direction
- Resolve ambiguity in requirements before proceeding
- Get approval before descoping, deferring, or simplifying any success criteria

**Never:**
- Mark a task complete when success criteria are not met
- Decide unilaterally to take a "simpler approach" or skip items
- Rationalize incomplete work as "done enough"
- Defer items without explicit user approval

If you believe success criteria should be changed or descoped, ask first - do not decide unilaterally.
