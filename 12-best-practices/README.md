# Best Practices Documentation

> Locally saved high-value documents from official Anthropic sources.

This folder contains key best practices and guidance documents that are frequently referenced when working with Claude Code. These documents are saved locally for offline access and full-text search.

## Documents

### Engineering Blog Articles

| Document | Description | Source |
|----------|-------------|--------|
| [claude-code-best-practices.md](claude-code-best-practices.md) | Comprehensive guide to agentic coding workflows, CLAUDE.md configuration, and optimization | anthropic.com/engineering |
| [building-effective-agents.md](building-effective-agents.md) | Foundational patterns for agent architecture: workflows vs agents, when to use each | anthropic.com/engineering |
| [context-engineering.md](context-engineering.md) | Managing context windows, compaction, memory, and multi-agent architectures | anthropic.com/engineering |
| [multi-agent-research-system.md](multi-agent-research-system.md) | How Anthropic built their Research feature with orchestrator-worker pattern | anthropic.com/engineering |
| [writing-tools-for-agents.md](writing-tools-for-agents.md) | Tool design principles, evaluation-driven development, MCP best practices | anthropic.com/engineering |
| [think-tool.md](think-tool.md) | The "think" tool for intermediate reasoning during complex tool use | anthropic.com/engineering |

### Platform Documentation

| Document | Description | Source |
|----------|-------------|--------|
| [claude-4-prompting.md](claude-4-prompting.md) | Claude 4.x specific prompting techniques, parallel tools, state management | platform.claude.com |
| [prompt-engineering-overview.md](prompt-engineering-overview.md) | Core prompt engineering techniques and when to use them | platform.claude.com |
| [tool-use-overview.md](tool-use-overview.md) | Tool use fundamentals, client vs server tools, MCP integration | platform.claude.com |

## Source Tracking

Each document has a corresponding `.source` file containing:
- `source_url`: Original URL
- `fetched_at`: When the content was retrieved
- `content_hash`: MD5 hash for change detection

Run `cat *.source` to see all source metadata.

## When to Use These Documents

- **Setting up Claude Code** → Start with `claude-code-best-practices.md`
- **Building agents** → Read `building-effective-agents.md` then `context-engineering.md`
- **Designing tools** → See `writing-tools-for-agents.md` and `tool-use-overview.md`
- **Prompting Claude 4.x** → Read `claude-4-prompting.md`
- **Multi-agent systems** → See `multi-agent-research-system.md`

## Freshness

These documents are point-in-time snapshots. Check the `.source` files for fetch dates. For the latest content, visit the source URLs directly.
