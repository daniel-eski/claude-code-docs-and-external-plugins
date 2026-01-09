# Claude Code Plugin Ecosystem Guide

A comprehensive explanation of how the Claude Code plugin system works, its architecture, capabilities, and the surrounding ecosystem.

**Last Updated**: January 9, 2026

---

## Table of Contents

1. [What Are Plugins?](#what-are-plugins)
2. [Plugin Architecture](#plugin-architecture)
3. [Plugin Components](#plugin-components)
4. [Installation & Management](#installation--management)
5. [Marketplaces](#marketplaces)
6. [Official vs Community Plugins](#official-vs-community-plugins)
7. [Recent Developments](#recent-developments)
8. [Known Limitations](#known-limitations)
9. [Key Resources](#key-resources)

---

## What Are Plugins?

Claude Code plugins are **shareable packages** that bundle customizations into single installable units. They solve the problem of messy, manual setups by providing:

- **Portability**: Share configurations across machines, teams, and projects
- **Versioning**: Plugins can be updated independently
- **Discovery**: Marketplaces make finding useful extensions easy
- **Composability**: Plugins can be combined for compound functionality

**Released**: October 9, 2025 (public beta)
**Marketplace added**: December 16, 2025

---

## Plugin Architecture

### Directory Structure

Plugins live in `~/.claude/plugins/` with this structure:

```
~/.claude/plugins/
├── cache/                    # Downloaded plugin files
│   └── [marketplace]/
│       └── [plugin-name]/
│           └── [version]/    # Plugin source files
└── [Other internal files]
```

### Plugin Package Structure

A valid plugin contains:

```
plugin-name/
├── .claude-plugin/
│   ├── plugin.json          # Required: Plugin metadata
│   └── marketplace.json     # Optional: If this is a marketplace
├── commands/                 # Optional: Slash commands (*.md files)
├── agents/                   # Optional: Custom agents (*.md files)
├── skills/                   # Optional: Skills (SKILL.md files)
├── hooks/                    # Optional: Lifecycle hooks
├── mcp/                      # Optional: MCP server configs
└── [source files]            # Implementation (if applicable)
```

### plugin.json Schema

```json
{
  "name": "plugin-name",
  "description": "What this plugin does",
  "version": "1.0.0",
  "author": {
    "name": "Author Name",
    "url": "https://github.com/author"
  },
  "homepage": "https://github.com/author/plugin-name",
  "repository": "https://github.com/author/plugin-name",
  "license": "MIT",
  "keywords": ["relevant", "tags"]
}
```

---

## Plugin Components

Plugins can bundle any combination of these components:

### 1. Slash Commands

Custom commands invoked with `/command-name`.

**Location**: `commands/*.md`

**Format**:
```markdown
---
description: What this command does
argument-hint: [optional arguments]
allowed-tools: Bash, Read, Edit  # Optional: restrict tools
---

# Command instructions

$ARGUMENTS  # Placeholder for user input
```

**Example**: `/commit`, `/code-review`, `/feature-dev`

### 2. Custom Agents (Subagents)

Specialized AI personas for specific tasks.

**Location**: `agents/*.md`

**Format**:
```markdown
---
name: agent-name
description: When to invoke this agent
model: sonnet  # Optional: specify model
---

# Agent instructions and context
```

**Example**: `code-reviewer`, `security-auditor`, `test-writer`

### 3. Skills

Domain knowledge that Claude auto-invokes based on context.

**Location**: `skills/*/SKILL.md`

**Format**:
```markdown
---
name: skill-name
description: Trigger phrases and context for auto-invocation
---

# Knowledge and instructions
```

**Key difference from commands**: Skills are invoked automatically when relevant, not explicitly by the user.

### 4. Hooks

Shell commands that execute at specific lifecycle events.

**Events**:
| Hook | Trigger | Use Case |
|------|---------|----------|
| `PreToolUse` | Before any tool runs | Security checks, validation |
| `PostToolUse` | After tool completes | Logging, notifications |
| `SessionStart` | Session begins | Context injection |
| `Stop` | Session ends | Cleanup, summaries |

**Format** (in settings.json):
```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{"type": "command", "command": "./safety-check.sh $INPUT"}]
    }]
  }
}
```

### 5. MCP Servers

Model Context Protocol servers that extend Claude's capabilities.

**Location**: `mcp/*.json` or configured in plugin

**Use cases**: Database access, API integrations, specialized tools

---

## Installation & Management

### Core Commands

| Command | Purpose |
|---------|---------|
| `/plugin` | Open plugin management menu |
| `/plugin marketplace add <source>` | Add a marketplace |
| `/plugin install <name>` | Install from added marketplace |
| `/plugin install <name>@<marketplace>` | Install from specific marketplace |
| `/plugin uninstall <name>` | Remove a plugin |
| `/plugin list` | List installed plugins |

### Installation Scopes

Plugins can be installed at different scopes:

| Scope | Location | Use Case |
|-------|----------|----------|
| **User** | `~/.claude/` | Personal plugins across all projects |
| **Project** | `./.claude/` | Project-specific plugins (shared with team) |
| **Local** | Session only | Temporary testing |

### Configuration Storage

Plugin settings live in:
- **Global**: `~/.claude/settings.json`
- **Project**: `./.claude/settings.json`

Example:
```json
{
  "enabledPlugins": {
    "claude-hud@claude-hud": true,
    "safety-net@cc-marketplace": true
  },
  "statusLine": {
    "type": "command",
    "command": "..."
  }
}
```

---

## Marketplaces

### What Is a Marketplace?

A marketplace is a Git repository that catalogs multiple plugins. It contains:

```
marketplace-repo/
├── .claude-plugin/
│   └── marketplace.json     # Catalog of available plugins
└── [plugin directories]
```

### marketplace.json Format

```json
{
  "name": "marketplace-name",
  "owner": {
    "name": "Maintainer",
    "email": "email@example.com"
  },
  "plugins": [
    {
      "name": "plugin-name",
      "source": "./plugin-directory",
      "description": "What it does",
      "category": "category-name",
      "tags": ["tag1", "tag2"]
    }
  ]
}
```

### Major Marketplaces (as of January 2026)

| Marketplace | Focus | Notable Plugins |
|-------------|-------|-----------------|
| `anthropics/claude-code` | Official plugins | code-review, commit-commands, security-guidance |
| `ccplugins/awesome-claude-code-plugins` | Curated collection | 150+ community plugins |
| `boostvolt/claude-code-lsps` | LSP servers | 23 language servers |
| `kenryu42/cc-marketplace` | Safety & utilities | safety-net |
| `thedotmack/claude-mem` | Memory system | claude-mem |
| `wshobson/agents` | Agent collection | 99 agents, 107 skills |

See [MARKETPLACES.md](MARKETPLACES.md) for a complete list.

---

## Official vs Community Plugins

### Official Plugins (Anthropic)

Maintained by Anthropic at `anthropics/claude-code/plugins`:

| Plugin | Type | Description |
|--------|------|-------------|
| `agent-sdk-dev` | Command + Agents | Agent SDK project setup |
| `claude-opus-4-5-migration` | Skill | Model migration automation |
| `code-review` | Command + Agents | PR review with 5 parallel agents |
| `commit-commands` | Commands | Git workflow shortcuts |
| `explanatory-output-style` | Hook | Educational output mode |
| `feature-dev` | Command + Agents | 7-phase feature development |
| `frontend-design` | Skill | Production-grade UI guidance |
| `hookify` | Command | Custom hook creation |
| `learning-output-style` | Hook | Interactive learning mode |
| `plugin-dev` | Command + Skills | Plugin development toolkit |
| `pr-review-toolkit` | Command + Agents | Comprehensive PR review |
| `ralph-wiggum` | Command + Hook | Iterative development loops |
| `security-guidance` | Hook | Security pattern warnings |

**Trust level**: High — maintained by Anthropic, integrated with Claude Code releases.

### Community Plugins

Everything else. Quality varies from production-ready (20k+ stars) to experimental.

**Trust level**: Variable — apply [EVALUATION-CRITERIA.md](EVALUATION-CRITERIA.md) before adopting.

---

## Recent Developments

### December 2025

- **LSP Support** (v2.0.74): Language Server Protocol integration for real-time code intelligence
- **Plugin Marketplace** (Dec 16): First-party marketplace discovery
- **Hot Reload** (v2.1.0): Skills update without session restart

### January 2026

- **LSP Bug Fixes** (v2.1.0+): Fixed race condition breaking LSP in v2.0.69-2.0.x
- **Forked Contexts**: Skills can run in isolated contexts via `context: fork`

### Known Issues (as of January 2026)

1. **Official LSP plugins broken**: Plugins from `@claude-plugins-official` marketplace missing `plugin.json`. Use community alternatives like `boostvolt/claude-code-lsps`.

2. **LSP requires external binaries**: Installing an LSP plugin doesn't install the language server itself. You must install separately (e.g., `npm install -g @vtsls/language-server`).

---

## Known Limitations

### Plugin System Limitations

| Limitation | Impact | Workaround |
|------------|--------|------------|
| No dependency management | Plugins can't declare dependencies on other plugins | Manual installation order |
| No version pinning | Can't lock to specific plugin versions | Use Git commit SHAs |
| Limited sandboxing | Plugins can access filesystem, network | Review code before installing |
| No auto-updates | Plugins don't update automatically | Periodic manual updates |

### Hook Limitations

| Limitation | Impact |
|------------|--------|
| Shell-only | Hooks must be shell commands |
| Timeout | Long-running hooks can timeout |
| No async | Hooks block until complete |
| Limited context | Hooks receive limited information about the triggering event |

### Marketplace Limitations

| Limitation | Impact |
|------------|--------|
| No ratings/reviews | Can't see community feedback in CLI |
| No download counts | Hard to gauge popularity |
| Manual trust assessment | Must review repos yourself |

---

## Key Resources

### Official Documentation

| Resource | URL |
|----------|-----|
| Plugin Overview | https://code.claude.com/docs/en/plugins |
| Plugin Reference | https://code.claude.com/docs/en/plugin-reference |
| Marketplace Discovery | https://code.claude.com/docs/en/discover-plugins |
| Creating Marketplaces | https://code.claude.com/docs/en/plugin-marketplaces |

### Community Resources

| Resource | URL | Description |
|----------|-----|-------------|
| awesome-claude-code-plugins | github.com/ccplugins/awesome-claude-code-plugins | 150+ curated plugins |
| awesome-claude-plugins | github.com/quemsah/awesome-claude-plugins | 243 plugins with metrics |
| claude-plugins.dev | claude-plugins.dev | Community registry with CLI |
| Claude Developers Discord | discord.gg/anthropic | Community discussion |

### Awesome Lists

| List | Focus |
|------|-------|
| awesome-claude-code | General Claude Code resources |
| awesome-mcp-servers | MCP server collection |
| awesome-claude-code-agents | Agent-focused plugins |

---

## Revision History

| Date | Change |
|------|--------|
| 2026-01-09 | Initial creation based on ecosystem research |
