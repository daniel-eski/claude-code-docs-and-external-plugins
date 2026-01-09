# Plugins for Future Consideration

A running list of plugins, ideas, and resources to investigate. This is an append-only scratchpad for tracking interesting finds before they're fully evaluated.

**Last Updated**: January 9, 2026

---

## How to Use This Document

- **Add freely**: When you encounter an interesting plugin, add it here
- **Don't over-research**: Brief notes are fine; deep evaluation goes in REGISTRY.md
- **Date your entries**: The ecosystem moves fast
- **Move when ready**: Once fully evaluated, move to REGISTRY.md or delete if not relevant

---

## Format

```markdown
### Plugin Name
**Added**: [date]
**Source**: [url]
**What it might do**: [brief description]
**Why investigate**: [what caught your attention]
**Status**: [To investigate | Investigating | Evaluated → moved to REGISTRY]
```

---

## Plugins to Investigate

### Continuous Claude (Braintrust extension)

**Added**: January 9, 2026
**Source**: Mentioned on X/Twitter by @ankrgyl
**What it might do**: Extends Braintrust Claude Code plugin to help Claude review its own work
**Why investigate**: Meta use-case—Claude reviewing Claude's output; could help with code quality
**Status**: To investigate

---

### Redpanda Connect Plugin

**Added**: January 9, 2026
**Source**: [Redpanda Data announcement](https://x.com/redpandadata/status/2001730149780255138)
**What it might do**: Design, author, and debug data pipelines with natural language
**Why investigate**: If you work with data pipelines, this could be valuable
**Status**: To investigate

---

### Claude Code Workflow Studio

**Added**: January 9, 2026
**Source**: VS Code extension, mentioned in Medium articles
**What it might do**: Visual interface for building AI automation workflows
**Why investigate**: Could make workflow creation accessible to non-programmers
**Status**: To investigate

---

### cclint

**Added**: January 9, 2026
**Source**: [carlrannaberg/cclint](https://github.com/carlrannaberg/cclint)
**What it might do**: Linter for Claude Code project files (settings, hooks, commands)
**Why investigate**: CI/CD integration for validating Claude Code configurations
**Status**: To investigate

---

### cc-tools (Go implementation)

**Added**: January 9, 2026
**Source**: Josh Symonds
**What it might do**: High-performance Go implementation of Claude Code hooks/utilities
**Why investigate**: If you need low-latency hooks, Go might outperform shell scripts
**Status**: To investigate

---

### iOS Development Plugin

**Added**: January 9, 2026
**Source**: Community mention (December 18, 2025)
**What it might do**: Modular MCPs for Xcode/IDB tools, mindful of token/context usage
**Why investigate**: If you do iOS development
**Status**: To investigate

---

### Claude-Code-Workflow (CCW)

**Added**: January 9, 2026
**Source**: [catlog22/Claude-Code-Workflow](https://github.com/catlog22/Claude-Code-Workflow)
**What it might do**: JSON-driven multi-agent framework with CLI orchestration (Gemini/Qwen/Codex)
**Why investigate**: Multi-model orchestration could be interesting for cost optimization
**Status**: To investigate

---

## Ideas for Plugin Types to Research

These aren't specific plugins, but categories worth exploring:

### Database Integration Plugins
- MCP servers for PostgreSQL, MongoDB, etc.
- Query generation and optimization

### Testing Automation
- Plugins that auto-generate tests after code changes
- Integration with pytest, jest, etc.

### Documentation Generation
- Auto-doc plugins that create/update README, API docs
- Changelog generators

### Cost Optimization
- Plugins that track token usage across sessions
- Suggestions for reducing context

### Team Collaboration
- Plugins for sharing context across team members
- Project-level memory systems

---

## Recently Mentioned (Not Yet Categorized)

Quick captures from recent research—may be duplicates or not plugins at all:

- Jesse Vincent's superpowers customizations (mentioned by Simon Willison)
- Boris Cherny's subagent patterns (code-simplifier, verify-app)
- Multi-Agent Intelligence Marketplace (19 plugins for trading, swarm intelligence)
- Advanced RAG Marketplace (FAISS, HyDE optimization)

---

## Rejected / Not Pursuing

Document why something wasn't worth pursuing (saves future research time):

| Plugin | Reason | Date |
|--------|--------|------|
| (none yet) | | |

---

## Promoted to Registry

Track what moved to REGISTRY.md:

| Plugin | Promoted Date | Registry Section |
|--------|---------------|------------------|
| claude-mem | 2026-01-09 | Context Management |
| claude-hud | 2026-01-09 | Visibility |
| claude-code-safety-net | 2026-01-09 | Safety |
| boostvolt/claude-code-lsps | 2026-01-09 | Code Quality |

---

## Revision History

| Date | Change |
|------|--------|
| 2026-01-09 | Initial creation with research backlog |
