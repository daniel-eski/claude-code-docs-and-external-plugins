# Bootstrap Prompt for New Claude Code Sessions

> Copy-paste the prompt below into a new Claude Code terminal session.

---

## The Prompt

```
I need you to help improve and maintain this repository. This is a documentation + skills library for Claude Code.

**PHASE 1: ORIENTATION (Do this first, don't skip)**

Read these files in order to understand the current state:
1. /CLAUDE.md - Your primary guidance document
2. /_repo-maintenance/context-for-llm-agents.md - Continuation context from previous session
3. /_repo-maintenance/01-current-state.md - Repository inventory
4. /_repo-maintenance/02-challenges.md - Identified problems
5. /_repo-maintenance/03-recommendations.md - Prioritized solutions (P1/P2/P3)

If working with skills specifically, also read the folder /11-external-resources/_meta/, starting with the file:
- /11-external-resources/_meta/CONTINUATION-GUIDE.md

**PHASE 2: CRITICAL ANALYSIS**

After reading, I want you to:
1. Summarize what you understand about the current state
2. Identify which recommendations from 03-recommendations.md are most valuable, and consider enhancements suggested in /11-external-resources/_meta/ subfolder, and how they may interrelate
3. Consider if any recommendations are now outdated or if you see new opportunities, and consider how order of operations may effect your effectiveness as Claude Code agent
4. Propose your own prioritized implementation plan with rationale

**PHASE 3: WORKSPACE SETUP**

Before implementing, create a development workspace:
1. Create a TODO/progress tracking system for your work
2. Consider using Claude Code Skills if any are relevant (run `./11-external-resources/tools/deploy-all.sh` first if needed)
3. Break work into modular phases that can be completed incrementally
4. Document key decisions as you make them

**PHASE 4: IMPLEMENTATION**

Execute your plan, but:
- Complete one recommendation fully before starting the next
- Test/validate each change
- Update documentation as you go
- Log what you did in /_repo-maintenance/ for future sessions

Take your time on Phase 1-2. I'd rather you understand deeply than rush to implementation.
```

---

## Shorter Version (if you prefer brevity)

```
This repo has extensive maintenance documentation. Before doing anything:

1. Read /_repo-maintenance/context-for-llm-agents.md (critical context)
2. Read /_repo-maintenance/03-recommendations.md (prioritized action items)
3. Tell me what you think we should implement and why
4. Create a tracked implementation plan before executing

The repo already has analysis of what needs to be done - build on it, don't restart.
```

---

## Ultra-Short Version

```
Read /_repo-maintenance/context-for-llm-agents.md first, then tell me your implementation plan for the recommendations in 03-recommendations.md.
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

**To focus on a specific area:**
```
Let's focus on P1 recommendations only. Start with R1 (commit SHA tracking). Show me the changes before making them.
```

**To leverage Claude Code features:**
```
Before implementing, check if any of the deployed skills would help with this work. Also consider if any subtasks should use subagents.
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

