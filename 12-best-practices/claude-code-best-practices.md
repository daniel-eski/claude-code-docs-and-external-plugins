# Claude Code: Best Practices for Agentic Coding

> Source: https://www.anthropic.com/engineering/claude-code-best-practices
> Published: April 18, 2025

---

## Overview

Claude Code is Anthropic's command-line tool for agentic coding, designed as a low-level, unopinionated platform that provides direct model access without enforcing specific workflows. This creates flexibility but requires developers to establish their own best practices.

## 1. Customize Your Setup

### Create CLAUDE.md Files

`CLAUDE.md` files automatically integrate into conversations and should document:
- Common bash commands
- Core files and utility functions
- Code style guidelines
- Testing instructions
- Repository etiquette
- Developer environment setup
- Project-specific warnings or behaviors

The format is flexible but should remain concise and human-readable.

**Placement options:**
- Repository root (shared with team via git)
- Parent directories (useful for monorepos)
- Child directories (pulled on-demand)
- Home folder (`~/.claude/CLAUDE.md` for all sessions)

### Tune CLAUDE.md Files

Treat documentation like frequently-used prompts—iterate for effectiveness. Use the `#` key to add content that Claude will incorporate automatically. Some teams run files through prompt improvers and add emphasis keywords like "IMPORTANT" or "YOU MUST" to improve adherence.

### Curate Allowed Tools

By default, Claude Code requests permission for system-modifying actions. Customize permissions through:
- "Always allow" responses during sessions
- `/permissions` command to manage allowlists
- Manual editing of `.claude/settings.json`
- `--allowedTools` CLI flag for session-specific access

### Install gh CLI

Claude understands GitHub's command-line interface for issue creation, pull requests, and comments. Without it, Claude can still use the GitHub API or MCP servers.

## 2. Give Claude More Tools

### Bash Tools

Claude inherits your bash environment. Make custom tools discoverable by:
1. Providing tool names with usage examples
2. Instructing Claude to run `--help`
3. Documenting frequently-used tools in `CLAUDE.md`

### MCP Integration

Claude Code functions as both MCP server and client, connecting to multiple servers through:
- Project-specific config
- Global config
- Checked-in `.mcp.json` files (shared across team)

Use `--mcp-debug` flag to troubleshoot configuration issues.

### Custom Slash Commands

Store prompt templates as Markdown files in `.claude/commands/`. These appear in the slash menu and support the `$ARGUMENTS` keyword for parameters.

Example: A `/project:fix-github-issue` command could analyze and fix specific issues automatically.

## 3. Common Workflows

### Explore, Plan, Code, Commit

This versatile approach suits many problems:

1. **Research phase**: Ask Claude to read files/images/URLs without writing code
2. **Planning phase**: Request a plan using "think" to trigger extended thinking mode
3. **Implementation**: Ask Claude to code, verifying solutions as it proceeds
4. **Finalization**: Commit results and update documentation

The planning step is crucial—it prevents Claude from jumping straight to coding and significantly improves outcomes for complex problems.

### Test-Driven Development

Harness TDD with agentic coding:

1. Write tests based on expected inputs/outputs
2. Run tests and confirm failure
3. Commit tests
4. Write code to pass tests (iterate until success)
5. Commit passing code

Claude performs best with clear targets—tests provide concrete goals for iteration.

### Write, Screenshot, Iterate

For UI development:

1. Give Claude screenshot capability (Puppeteer MCP, iOS simulator MCP, or manual screenshots)
2. Provide visual design mocks
3. Have Claude implement the design, screenshot results, and iterate
4. Commit when satisfied

Multiple iterations typically yield significantly better visual results than first attempts.

### Safe YOLO Mode

Use `claude --dangerously-skip-permissions` to bypass permission checks. **Important**: Only use in isolated containers without internet access to prevent data loss or exfiltration. Reference implementations using Docker Dev Containers are available.

### Codebase Q&A

Use Claude as an onboarding tool:
- "How does logging work?"
- "How do I make a new API endpoint?"
- "What does `async move { ... }` do on line 134?"
- "What edge cases does CustomerOnboardingFlowImpl handle?"

No special prompting needed—Claude will search the codebase for answers.

### Git Interaction

Claude effectively handles:
- Searching git history to answer design questions
- Writing commit messages with full context
- Complex git operations (reverting, rebasing, patching)

### GitHub Integration

Claude manages:
- Creating pull requests
- Fixing code review comments
- Handling failing builds/linting warnings
- Categorizing and triaging issues

### Jupyter Notebooks

Researchers use Claude Code to read, write, and improve notebooks. Claude interprets outputs including images for fast data exploration. Suggesting "aesthetically pleasing" results reminds Claude of the human viewing experience.

## 4. Optimize Your Workflow

### Be Specific in Instructions

Success improves significantly with detailed directions upfront, reducing correction loops.

**Poor example**: "add tests for foo.py"
**Better example**: "write test case for foo.py, covering edge case where user is logged out. avoid mocks"

### Provide Images

Claude excels with visual input through:
- Screenshots (Mac: `cmd+ctrl+shift+4` to clipboard, then `ctrl+v`)
- Drag-and-drop images
- File paths

Especially useful for design mocks and visual debugging.

### Mention Specific Files

Use tab-completion to quickly reference files/folders Claude should examine or modify.

### Provide URLs

Paste URLs for Claude to fetch and read. Use `/permissions` to allow domain access without repeated prompts.

### Course Correct Early and Often

Active collaboration yields better results than hands-off supervision. Four helpful tools:

1. **Request plans before coding**—confirm approach before implementation
2. **Press Escape to interrupt**—pause at any phase while preserving context
3. **Double-tap Escape to rewind**—edit previous prompts and explore alternatives
4. **Ask Claude to undo changes**—take different approaches as needed

### Use /clear Between Tasks

During long sessions, context fills with irrelevant conversation. Clear frequently to maintain performance.

### Checklists for Complex Workflows

For large tasks (migrations, multiple lint fixes), have Claude write errors to a Markdown checklist and work through systematically, checking items off as completed.

### Pass Data Into Claude

Multiple approaches:
- Copy and paste directly (most common)
- Pipe into Claude Code: `cat foo.txt | claude`
- Request Claude pull data via bash/MCP tools
- Ask Claude to read files or fetch URLs

Most sessions combine these methods.

## 5. Headless Mode for Automation

Claude Code includes headless mode (`-p` flag) for non-interactive contexts like CI, pre-commit hooks, and build scripts. Use `--output-format stream-json` for streaming JSON output. Note: headless mode doesn't persist between sessions.

### Issue Triage

Automate GitHub event handlers to inspect issues and assign labels automatically.

### Code Review

Claude provides subjective reviews beyond traditional linting, catching typos, stale comments, and misleading names.

## 6. Multi-Claude Workflows

### Separate Writing and Review

Have one Claude write code while another reviews or tests it. Separate context often yields better results than single-instance workflows.

### Multiple Checkouts

Maintain 3-4 git checkouts in separate terminal tabs, running Claude in each with different tasks. Cycle through to monitor progress.

### Git Worktrees

Lighter-weight alternative: use `git worktree add ../project-feature-a feature-a` to check out multiple branches with isolated files but shared history. Each worktree gets its own Claude session on an independent task.

**Tips:**
- Use consistent naming conventions
- Maintain one terminal tab per worktree
- Set up iTerm2 notifications (Mac) for Claude's attention
- Use separate IDE windows for different worktrees
- Clean up: `git worktree remove ../project-feature-a`

### Headless with Custom Harness

Two patterns:

**Fanning out** (large migrations/analyses):
1. Have Claude generate a task list (e.g., 2000 files to migrate)
2. Loop through tasks, calling Claude with `claude -p "<task>" --allowedTools Edit Bash(git commit:*)`
3. Refine prompts across multiple runs

**Pipelining** (data processing):
`claude -p "<prompt>" --json | your_command` integrates Claude into existing pipelines with structured JSON output.

Use `--verbose` flag for debugging; disable in production for cleaner output.

---

**Written by**: Boris Cherny, drawing on insights from the broader Claude Code user community and Anthropic engineers including Daisy Hollman, Ashwin Bhat, Cat Wu, Sid Bidasaria, Cal Rueb, Nodir Turakulov, Barry Zhang, and Drew Hodun.
