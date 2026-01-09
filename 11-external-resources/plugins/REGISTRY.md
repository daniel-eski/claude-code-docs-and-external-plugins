# Plugin Registry

A catalog of evaluated Claude Code plugins with objective notes. This is a living document—add new findings as you discover them.

**Last Updated**: January 9, 2026
**Plugins Documented**: 25+

---

## How to Read This Registry

Each entry includes:
- **What it does**: Objective description
- **GitHub**: Source repository
- **Stars**: Community adoption indicator (at time of documentation)
- **Last Active**: Most recent update
- **Components**: What the plugin provides (commands, agents, hooks, etc.)
- **Notes**: Observations, limitations, considerations

**This registry does not make recommendations.** It provides information for decision-making.

---

## Table of Contents

1. [Context Management](#context-management)
2. [Code Quality & LSP](#code-quality--lsp)
3. [Safety & Guards](#safety--guards)
4. [Workflow Automation](#workflow-automation)
5. [Visibility & Monitoring](#visibility--monitoring)
6. [Browser Automation](#browser-automation)
7. [Official Anthropic Plugins](#official-anthropic-plugins)
8. [Large Plugin Collections](#large-plugin-collections)

---

## Context Management

Plugins that address context window limitations, cross-session memory, and codebase search.

### claude-mem

| Attribute | Value |
|-----------|-------|
| **What it does** | Persistent memory across Claude Code sessions. Automatically captures tool usage, compresses observations with AI, injects relevant context into future sessions |
| **GitHub** | [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) |
| **Stars** | ~12,800 |
| **Last Active** | January 8, 2026 (v9.0.1) |
| **Components** | MCP server, background workers, web UI |
| **Install** | `/plugin marketplace add thedotmack/claude-mem` then `/plugin install claude-mem` |

**Notes**:
- Adds 60-90 seconds latency per tool call for observation generation
- Uses ChromaDB for vector storage
- Includes experimental "Endless Mode" for extended sessions
- Requires restart after installation
- Web viewer UI at `http://localhost:37777`
- Configurable via `~/.claude-mem/settings.json`

**Considerations**:
- High value for complex, multi-session projects
- Latency tradeoff may not suit quick tasks
- Experimental—expect occasional issues

---

### claude-context (Zilliz)

| Attribute | Value |
|-----------|-------|
| **What it does** | Semantic code search via MCP. Indexes codebase, provides hybrid search (BM25 + dense vector), reduces context window usage by ~40% |
| **GitHub** | [zilliztech/claude-context](https://github.com/zilliztech/claude-context) |
| **Stars** | ~5,000 |
| **Last Active** | Active |
| **Components** | MCP server |
| **Install** | Via MCP configuration |

**Notes**:
- AST-based intelligent code chunking
- Incremental indexing via Merkle trees (only re-indexes changed files)
- Supports TypeScript, JavaScript, Python, Java, C++, and 9+ languages
- Integrates with Zilliz Cloud for scalable vector storage
- Multi-provider embeddings: OpenAI, VoyageAI, Ollama, Gemini

**Considerations**:
- Requires initial indexing time for large codebases
- Cloud integration optional but available for scale

---

### Context-Engine

| Attribute | Value |
|-----------|-------|
| **What it does** | MCP retrieval stack with hybrid search, micro-chunking, and local LLM prompt enhancement |
| **GitHub** | [m1rl0k/Context-Engine](https://github.com/m1rl0k/Context-Engine) |
| **Stars** | Moderate |
| **Last Active** | Active |
| **Components** | MCP server, Qdrant integration |
| **Install** | `/plugin marketplace add m1rl0k/Context-Engine` then `/plugin install context-engine` |

**Notes**:
- ReFRAG micro-chunking: 5-50 line precise spans vs entire files
- Hybrid search: semantic + lexical + cross-encoder reranking
- Auto-syncing: watches for changes, re-indexes automatically
- Memory system for team knowledge storage
- Compatible with Cursor, Windsurf, Cline, etc.

---

## Code Quality & LSP

Plugins that provide code intelligence, linting, type checking, and diagnostics.

### boostvolt/claude-code-lsps

| Attribute | Value |
|-----------|-------|
| **What it does** | LSP server integrations for 23 programming languages |
| **GitHub** | [boostvolt/claude-code-lsps](https://github.com/boostvolt/claude-code-lsps) |
| **Stars** | ~46 |
| **Last Active** | Active |
| **Components** | LSP configurations |
| **Install** | `/plugin marketplace add boostvolt/claude-code-lsps` then `/plugin install [language]@claude-code-lsps` |

**Supported Languages**:
Bash/Shell, C/C++/Objective-C, C#, Clojure, Dart/Flutter, Elixir, Gleam, Go, Java, Kotlin, Lua, Nix, OCaml, PHP, Python (pyright), Ruby, Rust, Swift, Terraform, TypeScript/JavaScript (vtsls), YAML, Zig

**Notes**:
- **Critical**: LSP plugins only configure Claude Code—you must install the actual language server binaries separately
- TypeScript: `npm install -g @vtsls/language-server typescript`
- Python: `pip install pyright` or `brew install pyright`
- Requires Claude Code v2.1.0+ (LSP broken in v2.0.69-2.0.x)
- Provides: go-to-definition, find-references, hover, diagnostics

**Why this over official?**:
- Official marketplace LSPs (`@claude-plugins-official`) are broken—missing plugin.json files
- This marketplace provides working configurations

---

### Piebald-AI/claude-code-lsps

| Attribute | Value |
|-----------|-------|
| **What it does** | Alternative LSP marketplace with similar coverage |
| **GitHub** | [Piebald-AI/claude-code-lsps](https://github.com/Piebald-AI/claude-code-lsps) |
| **Components** | LSP configurations |

**Notes**:
- Another option if boostvolt doesn't have your language
- Same installation pattern

---

## Safety & Guards

Plugins that prevent destructive operations and enforce safety policies.

### claude-code-safety-net

| Attribute | Value |
|-----------|-------|
| **What it does** | Catches destructive git and filesystem commands before execution |
| **GitHub** | [kenryu42/claude-code-safety-net](https://github.com/kenryu42/claude-code-safety-net) |
| **Stars** | ~704 |
| **Last Active** | January 8, 2026 (v0.4.1) |
| **Components** | PreToolUse hook |
| **Install** | `/plugin marketplace add kenryu42/cc-marketplace` then `/plugin install safety-net@cc-marketplace` |

**Blocked Operations**:

| Category | Commands Blocked |
|----------|------------------|
| **Git** | `checkout -- <files>`, `restore` (without --staged), `reset --hard/--merge`, `clean -f`, `push --force` (without --force-with-lease), `branch -D`, `stash drop/clear` |
| **Filesystem** | `rm -rf` outside cwd (except /tmp), `rm -rf` when cwd is $HOME, `rm -rf /` or `~`, `find -delete` |
| **Piped** | `xargs rm -rf`, `parallel rm -rf` |

**Notes**:
- Uses semantic command analysis, not simple prefix matching
- Recursively analyzes shell wrappers (blocks `bash -c 'rm -rf /'`)
- Custom rules via `.safety-net.json` files
- Paranoid mode available: `export SAFETY_NET_PARANOID=1`
- Low overhead, high value

**Rationale** (from author):
> After Claude Code silently wiped out hours of progress with a single `rm -rf`, it became evident that "soft" rules in CLAUDE.md cannot replace hard technical constraints.

---

## Workflow Automation

Plugins that streamline development workflows, git operations, and planning.

### wshobson/agents

| Attribute | Value |
|-----------|-------|
| **What it does** | Comprehensive system of 99 agents, 15 orchestrators, 107 skills, and 71 tools across 67 plugins |
| **GitHub** | [wshobson/agents](https://github.com/wshobson/agents) |
| **Stars** | ~24,800 |
| **Last Active** | Active |
| **Components** | Agents, skills, commands |
| **Install** | `/plugin marketplace add wshobson/agents` then selectively install needed plugins |

**Notes**:
- Very large collection—don't install everything
- Organized into focused, single-purpose plugins
- Cherry-pick specific agents/skills you need
- Well-documented with guides

---

### claude-code-workflows

| Attribute | Value |
|-----------|-------|
| **What it does** | Production-ready development workflows with 17 specialized agents |
| **GitHub** | [shinpr/claude-code-workflows](https://github.com/shinpr/claude-code-workflows) |
| **Components** | Agents, commands |

**Agents include**:
- code-reviewer, investigator, verifier, solver, task-executor

---

## Visibility & Monitoring

Plugins that provide real-time feedback about Claude Code sessions.

### claude-hud

| Attribute | Value |
|-----------|-------|
| **What it does** | Real-time statusline HUD showing model, context usage, git status, tools, agents, todos |
| **GitHub** | [jarrodwatts/claude-hud](https://github.com/jarrodwatts/claude-hud) |
| **Stars** | Moderate |
| **Last Active** | Active |
| **Components** | Statusline command |
| **Install** | `/plugin marketplace add jarrodwatts/claude-hud` then `/plugin install claude-hud` then `/claude-hud:setup` |

**Notes**:
- Requires Node.js 18+ or Bun
- Setup command auto-configures `~/.claude/settings.json`
- Configurable display options via `/claude-hud:configure`
- Presets: Full, Essential, Minimal
- Appears below input without requiring tmux or separate window

---

## Browser Automation

Plugins that enable Claude to interact with web browsers for testing and verification.

### dev-browser

| Attribute | Value |
|-----------|-------|
| **What it does** | Browser automation using your existing Chrome with logged-in sessions |
| **GitHub** | [SawyerHood/dev-browser](https://github.com/SawyerHood/dev-browser) |
| **Components** | Skill |

**Notes**:
- Uses Chrome extension to control existing browser
- Access to logged-in sessions, bookmarks, extensions
- Good for testing authenticated workflows

---

### browserbase/agent-browse

| Attribute | Value |
|-----------|-------|
| **What it does** | Cloud browser automation via Browserbase |
| **GitHub** | [browserbase/agent-browse](https://github.com/browserbase/agent-browse) |
| **Components** | Skill, MCP server |
| **Install** | `/plugin marketplace add browserbase/agent-browse` |

**Notes**:
- Browser runs in cloud, not locally
- Powered by Stagehand v3.0
- Consistent performance across environments

---

### playwright-skill

| Attribute | Value |
|-----------|-------|
| **What it does** | Claude writes and executes Playwright automation on-the-fly |
| **GitHub** | [lackeyjb/playwright-skill](https://github.com/lackeyjb/playwright-skill) |
| **Components** | Skill |

**Notes**:
- Model-invoked—Claude autonomously writes custom automation
- From simple page tests to complex multi-step flows

---

## Official Anthropic Plugins

Maintained by Anthropic at `anthropics/claude-code/plugins`. High trust level.

| Plugin | Type | Description |
|--------|------|-------------|
| **agent-sdk-dev** | Command + Agents | `/new-sdk-app` for Agent SDK project setup |
| **claude-opus-4-5-migration** | Skill | Automated migration to Opus 4.5 |
| **code-review** | Command + Agents | PR review with 5 parallel Sonnet agents |
| **commit-commands** | Commands | `/commit`, `/commit-push-pr`, `/clean_gone` |
| **feature-dev** | Command + Agents | 7-phase feature development workflow |
| **frontend-design** | Skill | Production-grade UI guidance |
| **hookify** | Command | Create custom hooks from patterns |
| **plugin-dev** | Command + Skills | Plugin development toolkit |
| **pr-review-toolkit** | Command + Agents | 6-agent comprehensive PR review |
| **security-guidance** | Hook | Security pattern warnings (injection, XSS, eval, etc.) |

**Install**: `/plugin install [name]` (from default marketplace)

---

## Large Plugin Collections

Marketplaces and collections with many plugins.

### ccplugins/awesome-claude-code-plugins

| Attribute | Value |
|-----------|-------|
| **What it does** | Curated collection of 150+ plugins across 13 categories |
| **GitHub** | [ccplugins/awesome-claude-code-plugins](https://github.com/ccplugins/awesome-claude-code-plugins) |
| **Stars** | ~314 |
| **Website** | claudecodeplugins.dev |

**Categories**: Code Quality Testing (16), Development Engineering (15), Git Workflow (14), Project Management (10), Workflow Orchestration (8), Business/Sales (8), Design/UX (8), Documentation (8), Marketing (7), Security/Legal (7), Automation/DevOps (5), Data Analytics (5)

---

### quemsah/awesome-claude-plugins

| Attribute | Value |
|-----------|-------|
| **What it does** | Automated metrics collection across 243 plugins |
| **GitHub** | [quemsah/awesome-claude-plugins](https://github.com/quemsah/awesome-claude-plugins) |

**Notes**:
- 175 plugins with Agent Skills v1.2.0
- Claims 100% compliance with Anthropic 2025 Skills schema
- Uses n8n workflows for automated tracking
- 2,401 repositories indexed

---

### jeremylongshore/claude-code-plugins-plus-skills

| Attribute | Value |
|-----------|-------|
| **What it does** | 739 total skills (500 standalone + 239 embedded) with Jupyter tutorials |
| **GitHub** | [jeremylongshore/claude-code-plugins-plus-skills](https://github.com/jeremylongshore/claude-code-plugins-plus-skills) |
| **Stars** | ~920 |

**Categories**: DevOps, Security, ML, Data, API, and 15+ more

---

## Adding New Entries

When documenting a new plugin:

```markdown
### plugin-name

| Attribute | Value |
|-----------|-------|
| **What it does** | [Objective description] |
| **GitHub** | [link](url) |
| **Stars** | [count] |
| **Last Active** | [date] |
| **Components** | [commands/agents/hooks/skills/MCP] |
| **Install** | [commands] |

**Notes**:
- [Observation 1]
- [Observation 2]

**Considerations**:
- [Tradeoff or limitation]
```

---

## Revision History

| Date | Change |
|------|--------|
| 2026-01-09 | Initial registry with 25+ plugins documented |
