# Bootstrap Prompt for New Claude Code Sessions

> Copy-paste the prompt below into a new Claude Code terminal session.

**Last updated**: 2026-01-07 (P1+P2 complete, maintenance infrastructure built)

---

## The Prompt

```
Help me continue improving this Claude Code documentation + skills library repository.

**PHASE 1: ORIENTATION** (Read before acting)

Start with these files, in order:
1. /CLAUDE.md — Primary agent guidance
2. /_repo-maintenance/SESSION-2026-01-07-maintenance-infrastructure.md — Recent work summary
3. /_repo-maintenance/context-for-llm-agents.md — Continuation context
4. /_repo-maintenance/03-recommendations.md — Implementation status (P1+P2 done, P3 remaining)

For skills work, also read:
- /11-external-resources/_meta/CONTINUATION-GUIDE.md

**PHASE 2: CRITICAL ANALYSIS**

After reading:
1. Summarize what's already built vs. what remains
2. Review unimplemented items (R7, R9, R10 in 03-recommendations.md) and enhancements in /11-external-resources/_meta/FUTURE-ENHANCEMENTS.md
3. Identify new opportunities beyond the existing recommendations
4. Propose prioritized next steps with rationale

**PHASE 3: WORKSPACE SETUP**

Before implementing:
1. Use TodoWrite to track your work
2. Consider deploying skills (`./11-external-resources/tools/deploy-all.sh`) if helpful
3. Break work into modular phases
4. Use subagents for parallel or specialized tasks where appropriate

**PHASE 4: IMPLEMENTATION**

Execute iteratively:
- Complete one item fully before starting the next
- Test and validate each change
- Update documentation as you go
- Create/update a session log in /_repo-maintenance/

Take your time on Phases 1-2. Deep understanding beats rushed implementation.
```

---

## Shorter Version (if you prefer brevity)

```
This repo has maintenance infrastructure already built (P1+P2 complete). Before doing anything:

1. Read /_repo-maintenance/SESSION-2026-01-07-maintenance-infrastructure.md (what's done)
2. Read /_repo-maintenance/03-recommendations.md (remaining P3 items: R7, R9, R10)
3. Tell me what you think we should do next and why
4. Use TodoWrite to track your implementation plan

Build on existing work - don't restart the analysis.
```

---

## Ultra-Short Version

```
Read /_repo-maintenance/context-for-llm-agents.md, then propose next steps for the remaining recommendations (R7, R9, R10) or new opportunities you identify.
```

---

## Why This Structure Works

| Phase | Purpose |
|-------|---------|
| Orientation | Prevents the agent from redoing analysis that's already done |
| Critical Analysis | Ensures the agent thinks before acting; may find better approaches |
| Workspace Setup | Establishes tracking and structure for multi-step work |
| Implementation | Focused execution with documentation |

---

## Tips for Best Results

1. **Don't rush the agent** — Explicitly tell it to take time on analysis
2. **Ask for summaries** — Have the agent prove it understood before implementing
3. **Request rationale** — "Why this order?" forces better planning
4. **Mention capabilities** — Remind it about skills, subagents, TODO tracking
5. **Set checkpoints** — "After each recommendation, summarize what you did"

---

## Example Follow-Up Prompts

After the initial bootstrap, you might say:

**To focus on remaining recommendations:**
```
Let's implement R7 (check-links.sh) from 03-recommendations.md. Show me your plan before making changes.
```

**To explore new opportunities:**
```
Now that the maintenance infrastructure exists, what new capabilities or improvements would you suggest? Think beyond the original recommendations.
```

**To leverage Claude Code features:**
```
Before implementing, deploy the skills library and check if any would help. Also consider if subtasks should use subagents for parallel work.
```

**To ensure documentation:**
```
After each change, update the relevant documentation. I want a future agent to be able to continue seamlessly.
```

---

## What the Agent Should Produce

A good response to the bootstrap prompt should include:

1. **Confirmation of understanding** — Summary of repo state
2. **Analysis of recommendations** — Which ones, what order, why
3. **Implementation plan** — Specific steps with tracking
4. **Questions** — Any clarifications needed before starting

If the agent jumps straight to implementation without these, ask it to step back.

