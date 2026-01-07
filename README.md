# Claude Code Documentation

> Complete offline documentation for Claude Code, Anthropic's agentic coding tool.

This repository contains a local copy of all Claude Code documentation from [code.claude.com](https://code.claude.com), organized for easy navigation and LLM consumption.

**Last Updated:** January 6, 2026

---

## 📁 Documentation Structure

### 01-getting-started/
Get up and running with Claude Code quickly.

| File | Description |
|------|-------------|
| [overview.md](01-getting-started/overview.md) | Learn about Claude Code, Anthropic's agentic coding tool that lives in your terminal |
| [quickstart.md](01-getting-started/quickstart.md) | Welcome to Claude Code! Get started in minutes |
| [setup.md](01-getting-started/setup.md) | Install, authenticate, and start using Claude Code on your development machine |
| [interactive-mode.md](01-getting-started/interactive-mode.md) | Complete reference for keyboard shortcuts, input modes, and interactive features |

### 02-core-features/
Deep dives into Claude Code's core capabilities.

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
Integrate Claude Code with your development environment.

| File | Description |
|------|-------------|
| [vs-code.md](03-ide-integration/vs-code.md) | Install and configure the Claude Code extension for VS Code |
| [jetbrains.md](03-ide-integration/jetbrains.md) | Use Claude Code with JetBrains IDEs including IntelliJ, PyCharm, WebStorm |
| [desktop.md](03-ide-integration/desktop.md) | Run Claude Code tasks locally or on secure cloud infrastructure |
| [chrome.md](03-ide-integration/chrome.md) | Connect Claude Code to your browser to test web apps and automate tasks |

### 04-cloud-providers/
Configure Claude Code with various cloud AI providers.

| File | Description |
|------|-------------|
| [amazon-bedrock.md](04-cloud-providers/amazon-bedrock.md) | Configure Claude Code through Amazon Bedrock |
| [google-vertex-ai.md](04-cloud-providers/google-vertex-ai.md) | Configure Claude Code through Google Vertex AI |
| [microsoft-foundry.md](04-cloud-providers/microsoft-foundry.md) | Configure Claude Code through Microsoft Foundry |
| [llm-gateway.md](04-cloud-providers/llm-gateway.md) | Configure Claude Code to work with LLM gateway solutions |

### 05-enterprise/
Enterprise deployment and security configurations.

| File | Description |
|------|-------------|
| [iam.md](05-enterprise/iam.md) | Configure user authentication, authorization, and access controls |
| [network-config.md](05-enterprise/network-config.md) | Configure for enterprise environments with proxy servers and custom CAs |
| [security.md](05-enterprise/security.md) | Learn about Claude Code's security safeguards and best practices |
| [sandboxing.md](05-enterprise/sandboxing.md) | Learn how Claude Code's sandboxed bash tool provides isolation |
| [monitoring-usage.md](05-enterprise/monitoring-usage.md) | Enable and configure OpenTelemetry for Claude Code |
| [third-party-integrations.md](05-enterprise/third-party-integrations.md) | Enterprise deployment overview with third-party services |

### 06-cicd-automation/
Automate Claude Code in CI/CD pipelines and programmatic usage.

| File | Description |
|------|-------------|
| [github-actions.md](06-cicd-automation/github-actions.md) | Integrate Claude Code into your workflow with GitHub Actions |
| [gitlab-ci-cd.md](06-cicd-automation/gitlab-ci-cd.md) | Integrate Claude Code into your workflow with GitLab CI/CD |
| [headless.md](06-cicd-automation/headless.md) | Use the Agent SDK to run Claude Code programmatically |
| [claude-code-on-the-web.md](06-cicd-automation/claude-code-on-the-web.md) | Run Claude Code tasks asynchronously on secure cloud infrastructure |

### 07-plugins/
Extend Claude Code with the plugin system.

| File | Description |
|------|-------------|
| [plugins.md](07-plugins/plugins.md) | Create custom plugins to extend Claude Code |
| [plugins-reference.md](07-plugins/plugins-reference.md) | Complete technical reference for the plugin system |
| [discover-plugins.md](07-plugins/discover-plugins.md) | Discover and install prebuilt plugins through marketplaces |
| [plugin-marketplaces.md](07-plugins/plugin-marketplaces.md) | Create and distribute a plugin marketplace |

### 08-configuration/
Configure Claude Code settings and behavior.

| File | Description |
|------|-------------|
| [settings.md](08-configuration/settings.md) | Configure Claude Code with global and project-level settings |
| [model-config.md](08-configuration/model-config.md) | Learn about model configuration, including model aliases |
| [terminal-config.md](08-configuration/terminal-config.md) | Optimize your terminal setup for Claude Code |
| [statusline.md](08-configuration/statusline.md) | Create a custom status line to display contextual information |
| [output-styles.md](08-configuration/output-styles.md) | Adapt Claude Code for uses beyond software engineering |
| [checkpointing.md](08-configuration/checkpointing.md) | Automatically track and rewind Claude's edits |

### 09-reference/
Reference documentation and troubleshooting guides.

| File | Description |
|------|-------------|
| [cli-reference.md](09-reference/cli-reference.md) | Complete reference for Claude Code command-line interface |
| [troubleshooting.md](09-reference/troubleshooting.md) | Solutions to common issues with installation and usage |
| [costs.md](09-reference/costs.md) | Track and optimize token usage and costs |
| [data-usage.md](09-reference/data-usage.md) | Learn about Anthropic's data usage policies |
| [analytics.md](09-reference/analytics.md) | View detailed usage insights and productivity metrics |
| [devcontainer.md](09-reference/devcontainer.md) | Development container for consistent, secure environments |

### 10-other/
Additional resources and integrations.

| File | Description |
|------|-------------|
| [slack.md](10-other/slack.md) | Delegate coding tasks directly from your Slack workspace |
| [changelog.md](10-other/changelog.md) | Claude Code release notes and version history |
| [legal-and-compliance.md](10-other/legal-and-compliance.md) | Legal agreements, compliance certifications, and security information |
| [common-workflows.md](10-other/common-workflows.md) | Learn about common workflows with Claude Code |

### 11-external-resources/
Curated external skills and resources for skill development.

| File/Directory | Description |
|----------------|-------------|
| [README.md](11-external-resources/README.md) | Overview and navigation guide |
| [CATALOG.md](11-external-resources/CATALOG.md) | Complete indexed catalog of all skills |
| [QUICKSTART.md](11-external-resources/QUICKSTART.md) | 5-minute deployment guide |
| [core-skills/](11-external-resources/core-skills/) | 28 production-ready skills (obra, fvadicamo, anthropics) |
| [extended-skills/](11-external-resources/extended-skills/) | 4 AWS skills + context engineering links |
| [reference/](11-external-resources/reference/) | Links to community skills and learning resources |
| [tools/](11-external-resources/tools/) | Deployment and validation scripts |
| [meta/](11-external-resources/meta/) | Skill authoring documentation |

---

## 📊 Quick Stats

- **Total Documentation Files:** 49
- **Categories:** 11 (including external resources)
- **External Skills:** 32 (28 core + 4 extended)
- **Source:** https://code.claude.com/docs

## 🔗 Additional Resources

- **Official Documentation:** https://code.claude.com
- **LLMs.txt Index:** See [llms.txt](llms.txt) for the official documentation index

---

> To find navigation and other pages in this documentation, fetch the llms.txt file at: https://code.claude.com/docs/llms.txt

