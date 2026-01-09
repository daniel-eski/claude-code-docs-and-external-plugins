# Documentation Index

> Complete index of all 49 Claude Code documentation files with one-line descriptions.

---

## Quick Navigation by Task

| I want to... | Start here |
|--------------|------------|
| Install Claude Code | `01-getting-started/setup.md` |
| Learn keyboard shortcuts | `01-getting-started/interactive-mode.md` |
| Create a skill | `02-core-features/skills.md` |
| Add MCP server | `02-core-features/mcp.md` |
| Create subagents | `02-core-features/subagents.md` |
| Use in VS Code | `03-ide-integration/vs-code.md` |
| Configure AWS Bedrock | `04-cloud-providers/amazon-bedrock.md` |
| Set up GitHub Actions | `06-cicd-automation/github-actions.md` |
| Run headless/programmatically | `06-cicd-automation/headless.md` |
| Create a plugin | `07-plugins/plugins.md` |
| Change settings | `08-configuration/settings.md` |
| Fix an error | `09-reference/troubleshooting.md` |
| Track costs | `09-reference/costs.md` |

---

## Complete File Index

### 01-getting-started/ (4 files)

| File | Description |
|------|-------------|
| [overview.md](01-getting-started/overview.md) | What Claude Code is, key capabilities, how it works |
| [quickstart.md](01-getting-started/quickstart.md) | 5-minute guide to your first Claude Code session |
| [setup.md](01-getting-started/setup.md) | Full installation, authentication, and initial configuration |
| [interactive-mode.md](01-getting-started/interactive-mode.md) | Keyboard shortcuts, input modes, slash commands reference |

### 02-core-features/ (7 files)

| File | Description |
|------|-------------|
| [skills.md](02-core-features/skills.md) | Create, manage, and share reusable skill packages |
| [memory.md](02-core-features/memory.md) | CLAUDE.md files, memory persistence across sessions |
| [hooks.md](02-core-features/hooks.md) | Reference for hook events and configuration schema |
| [hooks-guide.md](02-core-features/hooks-guide.md) | Practical guide to implementing custom hooks |
| [mcp.md](02-core-features/mcp.md) | Connect Claude Code to external tools via Model Context Protocol |
| [slash-commands.md](02-core-features/slash-commands.md) | Built-in commands: /help, /compact, /clear, etc. |
| [subagents.md](02-core-features/subagents.md) | Create specialized AI subagents for complex workflows |

### 03-ide-integration/ (4 files)

| File | Description |
|------|-------------|
| [vs-code.md](03-ide-integration/vs-code.md) | Extension installation, inline diffs, @-mentions, shortcuts |
| [jetbrains.md](03-ide-integration/jetbrains.md) | IntelliJ, PyCharm, WebStorm, and other JetBrains IDEs |
| [desktop.md](03-ide-integration/desktop.md) | Claude desktop app for local and cloud task execution |
| [chrome.md](03-ide-integration/chrome.md) | Browser integration for web testing and automation |

### 04-cloud-providers/ (4 files)

| File | Description |
|------|-------------|
| [amazon-bedrock.md](04-cloud-providers/amazon-bedrock.md) | AWS Bedrock setup, IAM configuration, troubleshooting |
| [google-vertex-ai.md](04-cloud-providers/google-vertex-ai.md) | GCP Vertex AI setup, authentication, region configuration |
| [microsoft-foundry.md](04-cloud-providers/microsoft-foundry.md) | Azure Foundry integration and configuration |
| [llm-gateway.md](04-cloud-providers/llm-gateway.md) | Generic LLM gateway configuration for custom endpoints |

### 05-enterprise/ (6 files)

| File | Description |
|------|-------------|
| [security.md](05-enterprise/security.md) | Security safeguards, permission model, best practices |
| [iam.md](05-enterprise/iam.md) | User authentication, authorization, access controls |
| [monitoring-usage.md](05-enterprise/monitoring-usage.md) | OpenTelemetry integration, usage tracking |
| [network-config.md](05-enterprise/network-config.md) | Proxy servers, custom CAs, mTLS configuration |
| [sandboxing.md](05-enterprise/sandboxing.md) | Bash sandbox for filesystem and network isolation |
| [third-party-integrations.md](05-enterprise/third-party-integrations.md) | Enterprise deployment with external services |

### 06-cicd-automation/ (4 files)

| File | Description |
|------|-------------|
| [github-actions.md](06-cicd-automation/github-actions.md) | Claude Code GitHub Action for PRs and issues |
| [gitlab-ci-cd.md](06-cicd-automation/gitlab-ci-cd.md) | GitLab CI/CD pipeline integration |
| [headless.md](06-cicd-automation/headless.md) | Run Claude Code programmatically via Agent SDK |
| [claude-code-on-the-web.md](06-cicd-automation/claude-code-on-the-web.md) | Async task execution on cloud infrastructure |

### 07-plugins/ (4 files)

| File | Description |
|------|-------------|
| [plugins.md](07-plugins/plugins.md) | Create plugins with slash commands, agents, hooks, MCP servers |
| [plugins-reference.md](07-plugins/plugins-reference.md) | Technical reference: schemas, CLI commands, specifications |
| [discover-plugins.md](07-plugins/discover-plugins.md) | Find and install plugins from marketplaces |
| [plugin-marketplaces.md](07-plugins/plugin-marketplaces.md) | Build and host plugin marketplaces for distribution |

### 08-configuration/ (6 files)

| File | Description |
|------|-------------|
| [settings.md](08-configuration/settings.md) | Global, project, local, and enterprise settings scopes |
| [model-config.md](08-configuration/model-config.md) | Model aliases (opus, sonnet, haiku), custom model selection |
| [terminal-config.md](08-configuration/terminal-config.md) | Terminal optimization, shell configuration, tmux/screen |
| [statusline.md](08-configuration/statusline.md) | Custom status line with git, context, and tool info |
| [checkpointing.md](08-configuration/checkpointing.md) | Track edits, rewind changes, recovery from unwanted modifications |
| [output-styles.md](08-configuration/output-styles.md) | Adapt Claude Code output for non-engineering uses |

### 09-reference/ (6 files)

| File | Description |
|------|-------------|
| [cli-reference.md](09-reference/cli-reference.md) | Complete CLI command reference with all flags and options |
| [troubleshooting.md](09-reference/troubleshooting.md) | Common issues, solutions, and diagnostic steps |
| [costs.md](09-reference/costs.md) | Token usage tracking, cost optimization strategies |
| [analytics.md](09-reference/analytics.md) | Usage insights, productivity metrics for organizations |
| [data-usage.md](09-reference/data-usage.md) | Anthropic's data usage policies for Claude |
| [devcontainer.md](09-reference/devcontainer.md) | Development container for consistent environments |

### 10-other/ (4 files)

| File | Description |
|------|-------------|
| [changelog.md](10-other/changelog.md) | Release notes, version history, new features |
| [common-workflows.md](10-other/common-workflows.md) | Practical examples: debugging, refactoring, testing |
| [slack.md](10-other/slack.md) | Delegate coding tasks from Slack workspace |
| [legal-and-compliance.md](10-other/legal-and-compliance.md) | Legal agreements, compliance certifications, security info |

---

## Other Documentation

### Best Practices (12-best-practices/)

Locally-saved high-value documents from Anthropic engineering blog and platform docs.

| File | Description |
|------|-------------|
| [claude-code-best-practices.md](12-best-practices/claude-code-best-practices.md) | Agentic coding workflows, CLAUDE.md configuration |
| [building-effective-agents.md](12-best-practices/building-effective-agents.md) | Agent architecture patterns, workflows vs agents |
| [context-engineering.md](12-best-practices/context-engineering.md) | Managing context for long-running agents |
| [multi-agent-research-system.md](12-best-practices/multi-agent-research-system.md) | Orchestrator-worker pattern, 90% performance gains |
| [writing-tools-for-agents.md](12-best-practices/writing-tools-for-agents.md) | Tool design principles, MCP best practices |
| [think-tool.md](12-best-practices/think-tool.md) | Intermediate reasoning for complex tasks |
| [claude-4-prompting.md](12-best-practices/claude-4-prompting.md) | Claude 4.x specific prompting techniques |
| [prompt-engineering-overview.md](12-best-practices/prompt-engineering-overview.md) | Core prompt engineering techniques |
| [tool-use-overview.md](12-best-practices/tool-use-overview.md) | Tool use fundamentals, client vs server tools |

### External Resources Guide (13-external-resources-guide/)

Curated links to official documentation organized by use case.

| File | Description |
|------|-------------|
| [prompt-engineering.md](13-external-resources-guide/prompt-engineering.md) | Links to prompt engineering resources |
| [agent-development.md](13-external-resources-guide/agent-development.md) | Links to agent building resources |
| [api-reference.md](13-external-resources-guide/api-reference.md) | Links to Claude API documentation |
| [tutorials-courses.md](13-external-resources-guide/tutorials-courses.md) | Links to video courses and tutorials |
| [mcp-resources.md](13-external-resources-guide/mcp-resources.md) | Links to MCP documentation |
| [github-repositories.md](13-external-resources-guide/github-repositories.md) | Links to official Anthropic repos |

### Skills Library (11-external-resources/)

See [11-external-resources/CATALOG.md](11-external-resources/CATALOG.md) for the complete skills inventory.
