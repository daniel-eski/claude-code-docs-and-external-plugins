# Plugin Marketplaces

A catalog of known Claude Code plugin marketplaces and distribution channels.

**Last Updated**: January 9, 2026

---

## What Is a Marketplace?

A marketplace is a Git repository that catalogs and distributes multiple plugins. Users add marketplaces to access their plugins:

```bash
/plugin marketplace add owner/repo-name
/plugin install plugin-name@marketplace-name
```

---

## Official Marketplace

### Anthropic Official

| Attribute | Value |
|-----------|-------|
| **Source** | Built into Claude Code |
| **Add command** | Pre-installed |
| **Install command** | `/plugin install [name]` |
| **Plugin count** | 13 |
| **Trust level** | High |

**Available plugins**: agent-sdk-dev, claude-opus-4-5-migration, code-review, commit-commands, explanatory-output-style, feature-dev, frontend-design, hookify, learning-output-style, plugin-dev, pr-review-toolkit, ralph-wiggum, security-guidance

**Note**: As of January 2026, the official LSP plugins are broken (missing plugin.json). Use community LSP marketplaces instead.

---

## Community Marketplaces

### Curated Collections

#### ccplugins/awesome-claude-code-plugins

| Attribute | Value |
|-----------|-------|
| **GitHub** | [ccplugins/awesome-claude-code-plugins](https://github.com/ccplugins/awesome-claude-code-plugins) |
| **Add command** | `/plugin marketplace add ccplugins/awesome-claude-code-plugins` |
| **Plugin count** | 150+ |
| **Categories** | 13 |
| **Website** | claudecodeplugins.dev |

**Focus**: Curated, categorized collection across all plugin types

---

#### quemsah/awesome-claude-plugins

| Attribute | Value |
|-----------|-------|
| **GitHub** | [quemsah/awesome-claude-plugins](https://github.com/quemsah/awesome-claude-plugins) |
| **Add command** | `/plugin marketplace add quemsah/awesome-claude-plugins` |
| **Plugin count** | 243 |
| **Repos indexed** | 2,401 |

**Focus**: Automated metrics collection, adoption tracking

**Notable feature**: Claims 100% compliance with Anthropic 2025 Skills schema

---

#### jeremylongshore/claude-code-plugins-plus-skills

| Attribute | Value |
|-----------|-------|
| **GitHub** | [jeremylongshore/claude-code-plugins-plus-skills](https://github.com/jeremylongshore/claude-code-plugins-plus-skills) |
| **Add command** | `/plugin marketplace add jeremylongshore/claude-code-plugins-plus-skills` |
| **Plugin count** | 280 |
| **Skill count** | 739 total |

**Focus**: Plugins with embedded AI skills, Jupyter tutorials

---

### Specialized Marketplaces

#### LSP Plugins

##### boostvolt/claude-code-lsps

| Attribute | Value |
|-----------|-------|
| **GitHub** | [boostvolt/claude-code-lsps](https://github.com/boostvolt/claude-code-lsps) |
| **Add command** | `/plugin marketplace add boostvolt/claude-code-lsps` |
| **Languages** | 23 |

**Languages supported**: Bash/Shell, C/C++/Objective-C, C#, Clojure, Dart/Flutter, Elixir, Gleam, Go, Java, Kotlin, Lua, Nix, OCaml, PHP, Python, Ruby, Rust, Swift, Terraform, TypeScript/JavaScript, YAML, Zig

**Recommended over**: Official LSP plugins (which are currently broken)

---

##### Piebald-AI/claude-code-lsps

| Attribute | Value |
|-----------|-------|
| **GitHub** | [Piebald-AI/claude-code-lsps](https://github.com/Piebald-AI/claude-code-lsps) |
| **Add command** | `/plugin marketplace add Piebald-AI/claude-code-lsps` |

**Focus**: Alternative LSP marketplace

---

#### Safety & Utilities

##### kenryu42/cc-marketplace

| Attribute | Value |
|-----------|-------|
| **GitHub** | [kenryu42/cc-marketplace](https://github.com/kenryu42/cc-marketplace) |
| **Add command** | `/plugin marketplace add kenryu42/cc-marketplace` |

**Notable plugins**: safety-net

---

#### Agents & Workflows

##### wshobson/agents

| Attribute | Value |
|-----------|-------|
| **GitHub** | [wshobson/agents](https://github.com/wshobson/agents) |
| **Add command** | `/plugin marketplace add wshobson/agents` |
| **Agents** | 99 |
| **Orchestrators** | 15 |
| **Skills** | 107 |
| **Tools** | 71 |
| **Plugins** | 67 |

**Focus**: Comprehensive agent system for intelligent automation

---

### Single-Plugin Repositories

Some plugins are distributed as standalone repos (not marketplaces):

| Plugin | Add Command |
|--------|-------------|
| claude-mem | `/plugin marketplace add thedotmack/claude-mem` |
| claude-hud | `/plugin marketplace add jarrodwatts/claude-hud` |
| Context-Engine | `/plugin marketplace add m1rl0k/Context-Engine` |
| browserbase | `/plugin marketplace add browserbase/agent-browse` |

---

## Web-Based Directories

Not marketplaces, but useful for discovery:

| Site | URL | Description |
|------|-----|-------------|
| claude-plugins.dev | https://claude-plugins.dev | Community registry with CLI |
| claudecodemarketplace.com | https://claudecodemarketplace.com | Plugin directory |
| claudecodeplugins.io | https://claudecodeplugins.io | Skills hub |

---

## Awesome Lists

Reference collections (not installable marketplaces, but useful for discovery):

| List | URL | Focus |
|------|-----|-------|
| awesome-claude-code | github.com/hesreallyhim/awesome-claude-code | General resources |
| awesome-claude-code | github.com/jmanhype/awesome-claude-code | Plugins, MCP, integrations |
| awesome-claude-code | github.com/jqueryscript/awesome-claude-code | Tools, frameworks |
| awesome-mcp-servers | Various | MCP server collection |

---

## Creating Your Own Marketplace

To create a marketplace for your team or community:

1. Create a Git repository
2. Add `.claude-plugin/marketplace.json`:

```json
{
  "name": "my-marketplace",
  "owner": {
    "name": "Your Name",
    "email": "you@example.com"
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

3. Add plugin directories with valid `plugin.json` files
4. Users add via: `/plugin marketplace add your-username/repo-name`

See official docs: https://code.claude.com/docs/en/plugin-marketplaces

---

## Marketplace Quality Indicators

When evaluating a marketplace:

| Indicator | Good Sign | Concern |
|-----------|-----------|---------|
| Maintenance | Regular updates | No updates in 60+ days |
| Curation | Clear categories, descriptions | Dump of random plugins |
| Quality control | Validated plugins | Broken or incomplete plugins |
| Documentation | Installation guide | No documentation |
| Community | Active issues/PRs | Abandoned |

---

## Adding New Marketplaces to This Document

When documenting a new marketplace:

```markdown
#### owner/repo-name

| Attribute | Value |
|-----------|-------|
| **GitHub** | [owner/repo-name](url) |
| **Add command** | `/plugin marketplace add owner/repo-name` |
| **Plugin count** | [number] |
| **Focus** | [specialty] |

**Notes**: [observations]
```

---

## Revision History

| Date | Change |
|------|--------|
| 2026-01-09 | Initial marketplace catalog |
