# Agent Development Resources

> External resources for building agents, subagents, and agentic systems with Claude.

---

## Start Here (Local)

Before exploring external resources, check these locally-saved documents:
- `../12-best-practices/building-effective-agents.md` - Foundational agent patterns
- `../12-best-practices/context-engineering.md` - Managing context for long-running agents
- `../12-best-practices/multi-agent-research-system.md` - Multi-agent architecture case study
- `../12-best-practices/writing-tools-for-agents.md` - Tool design for agents
- `../12-best-practices/think-tool.md` - Intermediate reasoning during tool use

---

## Engineering Blog (anthropic.com/engineering)

### Core Agent Architecture

| Article | Description |
|---------|-------------|
| [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) | Workflows vs agents, when to use each |
| [Context Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) | Compaction, memory, multi-agent patterns |
| [Multi-Agent Research System](https://www.anthropic.com/engineering/multi-agent-research-system) | Orchestrator-worker pattern, 90% performance gains |
| [Building Agents with Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk) | SDK patterns, subagents, MCP integration |

### Tool Design

| Article | Description |
|---------|-------------|
| [Writing Tools for Agents](https://www.anthropic.com/engineering/writing-tools-for-agents) | Evaluation-driven tool development |
| [The "think" Tool](https://www.anthropic.com/engineering/claude-think-tool) | Intermediate reasoning for complex tasks |
| [Advanced Tool Use](https://www.anthropic.com/engineering/advanced-tool-use) | Tool Search, defer_loading |

### Long-Running Agents

| Article | Description |
|---------|-------------|
| [Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) | Memory management between sessions |
| [Claude Code Sandboxing](https://www.anthropic.com/engineering/claude-code-sandboxing) | 84% reduction in permission prompts |

---

## Official SDKs

### Claude Agent SDK

| Resource | Description |
|----------|-------------|
| [Agent SDK Python](https://github.com/anthropics/claude-agent-sdk-python) | Python SDK with MCP, hooks |
| [Agent SDK TypeScript](https://github.com/anthropics/claude-agent-sdk-typescript) | TypeScript/Node.js SDK |
| [Agent SDK Demos](https://github.com/anthropics/claude-agent-sdk-demos) | Multi-agent system examples |

### Platform Documentation

| Resource | Description |
|----------|-------------|
| [Agent Skills Overview](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) | Skills architecture for agents |
| [Agent Skills Best Practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) | Writing effective skills |

---

## Claude Code Resources

### Documentation

| Resource | Description |
|----------|-------------|
| [Claude Code Docs](https://code.claude.com/docs) | Official Claude Code documentation |
| [Subagents](https://code.claude.com/docs/en/core-features/subagents) | Built-in subagent types |

### Blog Posts

| Article | Description |
|---------|-------------|
| [How Anthropic Teams Use Claude Code](https://claude.com/blog/how-anthropic-teams-use-claude-code) | Internal usage patterns |
| [Introduction to Agentic Coding](https://claude.com/blog/introduction-to-agentic-coding) | Rakuten case study |
| [Scaling Agentic Coding](https://claude.com/blog/from-pilot-to-production) | Enterprise deployment |

---

## Courses

| Course | Description |
|--------|-------------|
| [Claude Code in Action](https://anthropic.skilljar.com/claude-code-in-action) | Workflow training |
| [Introduction to MCP](https://anthropic.skilljar.com/introduction-to-model-context-protocol) | Building MCP servers/clients |
| [Tool Use Course](https://anthropic.skilljar.com/) | Implementing tools in workflows |

---

## Cookbooks & Examples

| Resource | Description |
|----------|-------------|
| [Agent Patterns Cookbook](https://platform.claude.com/cookbook) | Basic workflows, ReAct agents |
| [Customer Support Agent](https://platform.claude.com/cookbook/tool-use-customer-service-agent) | Tool-integrated chatbot |
| [Claude Quickstarts](https://github.com/anthropics/claude-quickstarts) | Deployable application templates |

---

## Key Patterns Summary

| Pattern | When to Use |
|---------|-------------|
| **Single LLM + tools** | Most tasks - start simple |
| **Prompt chaining** | Fixed subtasks, accuracy over latency |
| **Routing** | Distinct categories needing specialization |
| **Parallelization** | Independent subtasks or voting |
| **Orchestrator-workers** | Complex tasks with unpredictable subtasks |
| **Evaluator-optimizer** | Iterative refinement with clear criteria |
| **Full agents** | Open-ended problems, unpredictable steps |
