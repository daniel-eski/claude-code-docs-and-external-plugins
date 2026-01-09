# Claude Code Community Plugins

A curated knowledge base of the Claude Code plugin ecosystem, designed for reference by humans and AI agents.

**Last Updated**: January 9, 2026
**Plugin System Released**: October 9, 2025
**Ecosystem Status**: Rapidly evolving (expect weekly changes)

---

## Quick Navigation

| Document | Purpose | Use When... |
|----------|---------|-------------|
| [ECOSYSTEM-GUIDE.md](ECOSYSTEM-GUIDE.md) | How the plugin system works | Understanding architecture, capabilities, limitations |
| [REGISTRY.md](REGISTRY.md) | Evaluated plugins catalog | Looking for specific plugins by category |
| [EVALUATION-CRITERIA.md](EVALUATION-CRITERIA.md) | How to assess plugins | Evaluating new plugins for adoption |
| [MARKETPLACES.md](MARKETPLACES.md) | Known plugin marketplaces | Finding where plugins are distributed |
| [CONSIDERATIONS.md](CONSIDERATIONS.md) | Ideas for future research | Tracking plugins to investigate |

---

## For AI Agents

This directory is optimized for AI agent consumption:

- **Start with the task at hand**: If searching for plugins, go to [REGISTRY.md](REGISTRY.md)
- **For understanding context**: Read [ECOSYSTEM-GUIDE.md](ECOSYSTEM-GUIDE.md) first
- **When evaluating new plugins**: Apply criteria from [EVALUATION-CRITERIA.md](EVALUATION-CRITERIA.md)
- **To add new findings**: Append to [CONSIDERATIONS.md](CONSIDERATIONS.md) or update [REGISTRY.md](REGISTRY.md)

### Key Context for Agents

1. **The plugin ecosystem moves fast** — A plugin that was "new" 2 weeks ago may now be mature or abandoned
2. **Official vs community** — Anthropic maintains ~13 official plugins; everything else is community
3. **Quality varies widely** — Some community plugins have 20k+ stars; others are weekend experiments
4. **Version compatibility** — LSP features require Claude Code v2.1.0+; some plugins require specific versions

---

## What This Is (and Isn't)

**This IS:**
- A reference guide explaining the plugin ecosystem
- A registry of evaluated plugins with notes
- Criteria for assessing new plugins
- A place to track plugins for future consideration

**This is NOT:**
- A prescriptive "install these plugins" list
- A guarantee of plugin quality or security
- An exhaustive list of all plugins (the ecosystem is too dynamic)
- Official Anthropic documentation

---

## Plugin Categories Overview

| Category | What It Solves | Example Plugins |
|----------|----------------|-----------------|
| **Context Management** | Memory across sessions, semantic code search | claude-mem, claude-context |
| **Code Quality** | Linting, type checking, security scanning | LSP plugins, security-guidance |
| **Workflow Automation** | Git workflows, PR review, planning | commit-commands, code-review |
| **Safety & Guards** | Preventing destructive operations | claude-code-safety-net |
| **Visibility** | Real-time session info, progress tracking | claude-hud |
| **Browser Automation** | Testing, verification, web interaction | dev-browser, browserbase |
| **Domain-Specific** | iOS dev, data pipelines, infrastructure | Various specialized plugins |

---

## How to Use This Resource

### Scenario 1: "I want to find a plugin for X"
1. Go to [REGISTRY.md](REGISTRY.md)
2. Find the relevant category
3. Read the notes on evaluated plugins
4. Check the "Last Verified" date (ecosystem moves fast)

### Scenario 2: "I found a new plugin, should I trust it?"
1. Apply criteria from [EVALUATION-CRITERIA.md](EVALUATION-CRITERIA.md)
2. Check activity level, code quality, security implications
3. Document findings in [REGISTRY.md](REGISTRY.md) or [CONSIDERATIONS.md](CONSIDERATIONS.md)

### Scenario 3: "I want to understand how plugins work"
1. Read [ECOSYSTEM-GUIDE.md](ECOSYSTEM-GUIDE.md) for architecture
2. Reference official docs at `07-plugins/` in the parent repository

### Scenario 4: "I want to add information to this resource"
1. For evaluated plugins: Update [REGISTRY.md](REGISTRY.md)
2. For plugins to investigate: Append to [CONSIDERATIONS.md](CONSIDERATIONS.md)
3. For ecosystem changes: Update [ECOSYSTEM-GUIDE.md](ECOSYSTEM-GUIDE.md)

---

## Contributing

When adding new plugins or updating existing entries:

1. **Include source URL** — Always link to the GitHub repo
2. **Note the date** — The ecosystem changes rapidly
3. **Be objective** — Document facts, not recommendations
4. **Note limitations** — Every plugin has tradeoffs
5. **Check recency** — A plugin from 2 months ago may be outdated

---

## Local Plugins in This Repository

These plugins are included directly in this repository:

| Plugin | Description | Commands |
|--------|-------------|----------|
| [claude-code-advisor](claude-code-advisor/) | Strategic advisor for Claude Code architecture with 10 specialized agents, 22 reference docs | `/cc-advisor`, `/cc-analyze`, `/cc-verify`, `/cc-design` |
| [context-introspection](context-introspection/) | Generates reports of all context sources influencing your session | `/context-introspection:report` |

**To install:**
```bash
# From this repository
claude plugin add ./11-external-resources/plugins/claude-code-advisor
claude plugin add ./11-external-resources/plugins/context-introspection
```

---

## Related Resources

| Resource | Location | Description |
|----------|----------|-------------|
| Official Plugin Docs | `07-plugins/` | Anthropic's plugin documentation |
| Official Skills | `anthropics/claude-code/plugins` | 13 official Anthropic plugins |
| Awesome Lists | See [MARKETPLACES.md](MARKETPLACES.md) | Community-curated collections |
| Claude Code Changelog | `10-other/changelog.md` | Feature release history |

---

## Revision History

| Date | Change |
|------|--------|
| 2026-01-09 | Initial creation with ecosystem research |
