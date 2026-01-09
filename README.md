# Claude Code Documentation

> Complete offline documentation for Claude Code, Anthropic's agentic coding tool, plus a curated library of community skills.

This repository combines:
- **Official documentation** from [code.claude.com](https://code.claude.com) for offline reference
- **32 production-ready skills** from the community, ready to deploy
- **Best practices** from Anthropic's engineering blog and platform docs
- **External resources guide** with curated links to tutorials, courses, and GitHub repos
- **Skill authoring guides** to help you create your own skills

**Last Updated:** January 6, 2026 (documentation) | January 9, 2026 (best practices & external resources)

---

## 🚀 Quick Start

### For Claude Code Documentation
- **Quick navigation**: See [docs-index.md](docs-index.md) for all 49 files with descriptions
- **By folder**: Browse folders `01-10` — each has a README.md with file descriptions
- **By task**: See CLAUDE.md "Finding Documentation by Task" section

### For Skills
```bash
# Deploy all 32 skills at once
cd 11-external-resources/tools/
./deploy-all.sh

# Or deploy a single skill
./deploy-skill.sh ../core-skills/obra-workflow/brainstorming
```

### For AI Agents
See [CLAUDE.md](CLAUDE.md) for comprehensive guidance on working with this repository.

---

## 📁 Repository Structure

### Core Documentation (folders 01-10)

Official Claude Code documentation mirrored from https://code.claude.com/docs. Each folder contains a README.md with one-line descriptions of all files.

| Folder | Topic | Files |
|--------|-------|-------|
| [01-getting-started/](01-getting-started/) | Installation, setup, first steps | 4 |
| [02-core-features/](02-core-features/) | Skills, memory, hooks, MCP, subagents | 7 |
| [03-ide-integration/](03-ide-integration/) | VS Code, JetBrains, Desktop, Chrome | 4 |
| [04-cloud-providers/](04-cloud-providers/) | AWS Bedrock, GCP Vertex AI, Azure | 4 |
| [05-enterprise/](05-enterprise/) | Security, IAM, monitoring, sandboxing | 6 |
| [06-cicd-automation/](06-cicd-automation/) | GitHub Actions, GitLab CI/CD, headless | 4 |
| [07-plugins/](07-plugins/) | Plugin creation, discovery, marketplaces | 4 |
| [08-configuration/](08-configuration/) | Settings, model config, terminal setup | 6 |
| [09-reference/](09-reference/) | CLI reference, troubleshooting, costs | 6 |
| [10-other/](10-other/) | Slack, changelog, legal, workflows | 4 |

### Skills Library (11-external-resources/)

Curated external skills ready to deploy.

| Directory | Description | Count |
|-----------|-------------|-------|
| [core-skills/](11-external-resources/core-skills/) | Production-ready skills | 28 |
| [extended-skills/](11-external-resources/extended-skills/) | Specialized AWS skills | 4 |
| [reference/](11-external-resources/reference/) | Links to community skills | 30+ |
| [tools/](11-external-resources/tools/) | Deployment & validation scripts | 5 |
| [meta/](11-external-resources/meta/) | Skill authoring documentation | 3 |
| [_meta/](11-external-resources/_meta/) | AI agent continuation docs | 4 |

**Key files:**
- [CATALOG.md](11-external-resources/CATALOG.md) — Complete skill inventory
- [QUICKSTART.md](11-external-resources/QUICKSTART.md) — 5-minute deployment guide
- [_meta/CONTINUATION-GUIDE.md](11-external-resources/_meta/CONTINUATION-GUIDE.md) — For AI agents working with skills

### Best Practices (12-best-practices/)

High-value documents saved locally from Anthropic's engineering blog and platform docs.

| Document | Description |
|----------|-------------|
| [claude-code-best-practices.md](12-best-practices/claude-code-best-practices.md) | Agentic coding workflows, CLAUDE.md, optimization |
| [building-effective-agents.md](12-best-practices/building-effective-agents.md) | Agent architecture patterns, when to use workflows vs agents |
| [context-engineering.md](12-best-practices/context-engineering.md) | Managing context for long-running agents |
| [multi-agent-research-system.md](12-best-practices/multi-agent-research-system.md) | Orchestrator-worker pattern, 90% performance gains |
| [writing-tools-for-agents.md](12-best-practices/writing-tools-for-agents.md) | Tool design principles, MCP best practices |
| [think-tool.md](12-best-practices/think-tool.md) | Intermediate reasoning for complex tasks |
| [claude-4-prompting.md](12-best-practices/claude-4-prompting.md) | Claude 4.x specific prompting techniques |
| [prompt-engineering-overview.md](12-best-practices/prompt-engineering-overview.md) | Core prompt engineering techniques |
| [tool-use-overview.md](12-best-practices/tool-use-overview.md) | Tool use fundamentals, client vs server tools |

### External Resources Guide (13-external-resources-guide/)

Curated links to official Anthropic documentation, organized by use case.

| File | Use Case |
|------|----------|
| [prompt-engineering.md](13-external-resources-guide/prompt-engineering.md) | Prompt engineering tutorials and guides |
| [agent-development.md](13-external-resources-guide/agent-development.md) | Building agents and subagents |
| [api-reference.md](13-external-resources-guide/api-reference.md) | Claude API documentation |
| [tutorials-courses.md](13-external-resources-guide/tutorials-courses.md) | Video courses and interactive tutorials |
| [mcp-resources.md](13-external-resources-guide/mcp-resources.md) | Model Context Protocol documentation |
| [github-repositories.md](13-external-resources-guide/github-repositories.md) | Official Anthropic GitHub repos |

### Repository Meta (_repo-maintenance/)

Analysis and recommendations for repository maintenance.

| Document | Purpose |
|----------|---------|
| [README.md](_repo-maintenance/README.md) | Overview and document index |
| [context-for-llm-agents.md](_repo-maintenance/context-for-llm-agents.md) | Continuation context for AI assistants |
| [01-current-state.md](_repo-maintenance/01-current-state.md) | Complete repository analysis |
| [03-recommendations.md](_repo-maintenance/03-recommendations.md) | Improvement recommendations |

---

## 📚 Documentation Details

### 01-getting-started/

| File | Description |
|------|-------------|
| [overview.md](01-getting-started/overview.md) | Learn about Claude Code, Anthropic's agentic coding tool that lives in your terminal |
| [quickstart.md](01-getting-started/quickstart.md) | Welcome to Claude Code! Get started in minutes |
| [setup.md](01-getting-started/setup.md) | Install, authenticate, and start using Claude Code on your development machine |
| [interactive-mode.md](01-getting-started/interactive-mode.md) | Complete reference for keyboard shortcuts, input modes, and interactive features |

### 02-core-features/

| File | Description |
|------|-------------|
| [memory.md](02-core-features/memory.md) | Learn how to manage Claude Code's memory across sessions |
| [skills.md](02-core-features/skills.md) | Create, manage, and share Skills to extend Claude's capabilities |
| [subagents.md](02-core-features/subagents.md) | Create and use specialized AI subagents for task-specific workflows |
| [slash-commands.md](02-core-features/slash-commands.md) | Control Claude's behavior during an interactive session |
| [hooks.md](02-core-features/hooks.md) | Reference documentation for implementing hooks in Claude Code |
| [hooks-guide.md](02-core-features/hooks-guide.md) | Learn how to customize and extend Claude Code's behavior |
| [mcp.md](02-core-features/mcp.md) | Connect Claude Code to your tools with the Model Context Protocol |

### 03-ide-integration/

| File | Description |
|------|-------------|
| [vs-code.md](03-ide-integration/vs-code.md) | Install and configure the Claude Code extension for VS Code |
| [jetbrains.md](03-ide-integration/jetbrains.md) | Use Claude Code with JetBrains IDEs including IntelliJ, PyCharm, WebStorm |
| [desktop.md](03-ide-integration/desktop.md) | Run Claude Code tasks locally or on secure cloud infrastructure |
| [chrome.md](03-ide-integration/chrome.md) | Connect Claude Code to your browser to test web apps and automate tasks |

### 04-cloud-providers/

| File | Description |
|------|-------------|
| [amazon-bedrock.md](04-cloud-providers/amazon-bedrock.md) | Configure Claude Code through Amazon Bedrock |
| [google-vertex-ai.md](04-cloud-providers/google-vertex-ai.md) | Configure Claude Code through Google Vertex AI |
| [microsoft-foundry.md](04-cloud-providers/microsoft-foundry.md) | Configure Claude Code through Microsoft Foundry |
| [llm-gateway.md](04-cloud-providers/llm-gateway.md) | Configure Claude Code to work with LLM gateway solutions |

### 05-enterprise/

| File | Description |
|------|-------------|
| [iam.md](05-enterprise/iam.md) | Configure user authentication, authorization, and access controls |
| [network-config.md](05-enterprise/network-config.md) | Configure for enterprise environments with proxy servers and custom CAs |
| [security.md](05-enterprise/security.md) | Learn about Claude Code's security safeguards and best practices |
| [sandboxing.md](05-enterprise/sandboxing.md) | Learn how Claude Code's sandboxed bash tool provides isolation |
| [monitoring-usage.md](05-enterprise/monitoring-usage.md) | Enable and configure OpenTelemetry for Claude Code |
| [third-party-integrations.md](05-enterprise/third-party-integrations.md) | Enterprise deployment overview with third-party services |

### 06-cicd-automation/

| File | Description |
|------|-------------|
| [github-actions.md](06-cicd-automation/github-actions.md) | Integrate Claude Code into your workflow with GitHub Actions |
| [gitlab-ci-cd.md](06-cicd-automation/gitlab-ci-cd.md) | Integrate Claude Code into your workflow with GitLab CI/CD |
| [headless.md](06-cicd-automation/headless.md) | Use the Agent SDK to run Claude Code programmatically |
| [claude-code-on-the-web.md](06-cicd-automation/claude-code-on-the-web.md) | Run Claude Code tasks asynchronously on secure cloud infrastructure |

### 07-plugins/

| File | Description |
|------|-------------|
| [plugins.md](07-plugins/plugins.md) | Create custom plugins to extend Claude Code |
| [plugins-reference.md](07-plugins/plugins-reference.md) | Complete technical reference for the plugin system |
| [discover-plugins.md](07-plugins/discover-plugins.md) | Discover and install prebuilt plugins through marketplaces |
| [plugin-marketplaces.md](07-plugins/plugin-marketplaces.md) | Create and distribute a plugin marketplace |

### 08-configuration/

| File | Description |
|------|-------------|
| [settings.md](08-configuration/settings.md) | Configure Claude Code with global and project-level settings |
| [model-config.md](08-configuration/model-config.md) | Learn about model configuration, including model aliases |
| [terminal-config.md](08-configuration/terminal-config.md) | Optimize your terminal setup for Claude Code |
| [statusline.md](08-configuration/statusline.md) | Create a custom status line to display contextual information |
| [output-styles.md](08-configuration/output-styles.md) | Adapt Claude Code for uses beyond software engineering |
| [checkpointing.md](08-configuration/checkpointing.md) | Automatically track and rewind Claude's edits |

### 09-reference/

| File | Description |
|------|-------------|
| [cli-reference.md](09-reference/cli-reference.md) | Complete reference for Claude Code command-line interface |
| [troubleshooting.md](09-reference/troubleshooting.md) | Solutions to common issues with installation and usage |
| [costs.md](09-reference/costs.md) | Track and optimize token usage and costs |
| [data-usage.md](09-reference/data-usage.md) | Learn about Anthropic's data usage policies |
| [analytics.md](09-reference/analytics.md) | View detailed usage insights and productivity metrics |
| [devcontainer.md](09-reference/devcontainer.md) | Development container for consistent, secure environments |

### 10-other/

| File | Description |
|------|-------------|
| [slack.md](10-other/slack.md) | Delegate coding tasks directly from your Slack workspace |
| [changelog.md](10-other/changelog.md) | Claude Code release notes and version history |
| [legal-and-compliance.md](10-other/legal-and-compliance.md) | Legal agreements, compliance certifications, and security information |
| [common-workflows.md](10-other/common-workflows.md) | Learn about common workflows with Claude Code |

---

## 🛠 Skills Library Highlights

### Workflow Skills (from Jesse Vincent's obra/superpowers)
- **brainstorming** — Generate and explore ideas systematically
- **writing-plans** — Create strategic documentation
- **dispatching-parallel-agents** — Coordinate multiple simultaneous agents
- **using-tmux-for-interactive-commands** — Control vim, git, and other interactive tools

### Development Skills
- **test-driven-development** — Write tests before implementing code (371 lines)
- **systematic-debugging** — Methodical problem-solving in code
- **git-commit** — Conventional Commits format with best practices
- **github-pr-creation** — Create well-structured pull requests

### Document Creation (from Anthropic)
- **docx** — Create, edit, and analyze Word documents
- **xlsx** — Create, edit, and analyze Excel spreadsheets
- **pptx** — Create, edit, and analyze PowerPoint presentations
- **pdf** — Extract text, create PDFs, and handle forms

See [11-external-resources/CATALOG.md](11-external-resources/CATALOG.md) for the complete list.

---

## 📊 Quick Stats

| Metric | Count |
|--------|-------|
| Documentation files | 49 |
| Categories | 13 |
| Best practices docs | 9 |
| External resource guides | 7 |
| Core skills | 28 |
| Extended skills | 4 |
| Community skill links | 30+ |
| Deployment tools | 5 |

---

## 🔗 Additional Resources

### Official
- **Claude Code**: https://code.claude.com
- **Documentation Index**: See [llms.txt](llms.txt)
- **Anthropic Skills Repo**: https://github.com/anthropics/skills

### Community
- **Awesome Claude Skills**: https://github.com/VoltAgent/awesome-claude-skills
- **obra/superpowers**: https://github.com/obra/superpowers

---

## 🤖 For AI Agents

If you're an AI assistant (Claude Code, etc.) working with this repository:

1. **Read [CLAUDE.md](CLAUDE.md)** for comprehensive guidance
2. **For structural changes**, read `_repo-maintenance/context-for-llm-agents.md` first
3. **The documentation is authoritative** as of the sync date in this file

---

## 📝 Maintenance

This repository is designed for both human use and AI agent consumption. Key maintenance resources:

- **Maintenance analysis**: [`_repo-maintenance/`](_repo-maintenance/) 
- **Skill update tool**: `11-external-resources/tools/update-sources.sh`
- **Validation**: `11-external-resources/tools/validate-skill.sh`

---

> To find navigation and other pages in this documentation, fetch the llms.txt file at: https://code.claude.com/docs/llms.txt
