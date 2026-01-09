# Model Context Protocol (MCP) Resources

> External resources for understanding and implementing MCP.

---

## What is MCP?

The Model Context Protocol (MCP) is an open standard created by Anthropic (now hosted by the Linux Foundation) for connecting AI applications to external systems. It enables Claude to interact with tools, data sources, and services through a standardized interface.

---

## Official Documentation

### MCP Specification

| Resource | Description |
|----------|-------------|
| [modelcontextprotocol.io](https://modelcontextprotocol.io) | Official MCP documentation |
| [MCP Specification](https://modelcontextprotocol.io/docs/concepts) | Protocol concepts and design |
| [MCP Quickstart](https://modelcontextprotocol.io/docs/quickstart) | Getting started guide |

### Platform Documentation

| Resource | Description |
|----------|-------------|
| [MCP in Claude](https://platform.claude.com/docs/en/build-with-claude/mcp) | Using MCP with Claude API |
| [MCP Connector](https://platform.claude.com/docs/en/agents-and-tools/mcp-connector) | Connect to MCP servers from API |

### Claude Code Documentation

| Resource | Description |
|----------|-------------|
| [MCP in Claude Code](https://code.claude.com/docs/en/core-features/mcp) | Configuring MCP servers |

---

## SDKs

### Official MCP SDKs

| SDK | Repository |
|-----|------------|
| Python | [modelcontextprotocol/python-sdk](https://github.com/modelcontextprotocol/python-sdk) |
| TypeScript | [modelcontextprotocol/typescript-sdk](https://github.com/modelcontextprotocol/typescript-sdk) |
| C# | Community SDK available |
| Java | Community SDK available |

---

## Pre-Built Servers

### Official Reference Servers

| Server | Description |
|--------|-------------|
| [MCP Servers](https://github.com/modelcontextprotocol/servers) | Reference implementations |

Available servers include:
- **Google Drive** - Document access
- **Slack** - Messaging integration
- **GitHub** - Repository access
- **Git** - Local git operations
- **Postgres** - Database queries
- **Puppeteer** - Browser automation
- **Filesystem** - Local file access

### Finding More Servers

| Resource | Description |
|----------|-------------|
| [MCP Server Registry](https://modelcontextprotocol.io/docs/registry) | Discoverable MCP servers |
| [Claude Code Plugins](https://code.claude.com/docs/en/plugins/discover-plugins) | Plugin/MCP discovery |

---

## Courses

| Course | Description |
|--------|-------------|
| [Introduction to MCP](https://anthropic.skilljar.com/introduction-to-model-context-protocol) | Building servers/clients in Python |
| [Advanced MCP Topics](https://anthropic.skilljar.com/) | Advanced patterns |

---

## Engineering Blog

| Article | Description |
|---------|-------------|
| [Code Execution with MCP](https://www.anthropic.com/engineering/code-execution-with-mcp) | 98.7% context overhead reduction |
| [Desktop Extensions](https://www.anthropic.com/engineering/desktop-extensions) | One-click MCP server installation |

---

## Getting Started with Claude Code

### Adding an MCP Server

```bash
claude mcp add <name> <command> [args...]
```

### Configuration Files

- **Project-specific**: `.mcp.json` in project root
- **Global**: `~/.claude/mcp.json`

### Debugging

```bash
claude --mcp-debug
```

---

## MCP Concepts

### Primitives

| Primitive | Description |
|-----------|-------------|
| **Tools** | Functions Claude can call |
| **Resources** | Data Claude can read |
| **Prompts** | Reusable prompt templates |

### Server Types

| Type | Description |
|------|-------------|
| **Stdio** | Runs as subprocess, communicates via stdin/stdout |
| **SSE** | HTTP-based, server-sent events |

---

## Best Practices

From `../12-best-practices/writing-tools-for-agents.md`:

1. **Clear tool descriptions** - Describe as you would to a new team member
2. **Namespacing** - Group related tools (e.g., `asana_search`, `jira_search`)
3. **Token efficiency** - Return only high-signal information
4. **Error handling** - Provide actionable error messages
5. **Evaluation-driven** - Test tools with real scenarios

---

## Related Local Documents

- `../12-best-practices/writing-tools-for-agents.md` - Tool design principles
- `../12-best-practices/tool-use-overview.md` - Tool use fundamentals
- `../02-core-features/mcp.md` - Claude Code MCP configuration
