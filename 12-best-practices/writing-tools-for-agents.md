# Writing Effective Tools for Agents — With Agents

> Source: https://www.anthropic.com/engineering/writing-tools-for-agents
> Published: 2025

---

## Introduction

Agents are only as effective as the tools we give them. This article shares techniques for writing high-quality tools and evaluations, and how you can use Claude to optimize its own tools.

The Model Context Protocol (MCP) can empower LLM agents with potentially hundreds of tools to solve real-world tasks. However, maximizing tool effectiveness requires a systematic approach.

## Key Process Steps

1. **Build and test prototypes** of your tools
2. **Create and run comprehensive evaluations** of your tools with agents
3. **Collaborate with agents** like Claude Code to automatically increase performance
4. **Apply principles** for writing high-quality tools

## What is a Tool?

Tools represent a new kind of software—a contract between deterministic systems and non-deterministic agents. Unlike traditional functions that always produce identical outputs, agents can generate varied responses even with the same input.

For example, when asked "Should I bring an umbrella today?", an agent might call a weather tool, answer from general knowledge, or ask clarifying questions. Agents may occasionally hallucinate or fail to understand how to use a tool properly.

This requires fundamentally rethinking software development: design tools for agents, not like traditional APIs for developers.

## How to Write Tools

### Building a Prototype

Start by standing up a quick prototype. If using Claude Code to write tools, provide documentation for relevant SDKs and APIs. LLM-friendly documentation often appears in `llms.txt` files on official sites.

Wrap tools in a local MCP server or Desktop extension to connect and test them in Claude Code or the Claude Desktop app.

**To connect to Claude Code:**
```
claude mcp add <name> <command> [args...]
```

**To connect to Claude Desktop:**
Navigate to Settings > Developer or Settings > Extensions

Tools can also be passed directly into Anthropic API calls for programmatic testing.

Test tools personally to identify rough edges and collect user feedback about expected use cases.

### Running an Evaluation

Measure how well Claude uses your tools by running evaluations. Start by generating numerous evaluation tasks grounded in real-world scenarios. Collaborate with agents to analyze results and determine improvements.

#### Generating Evaluation Tasks

Claude Code can quickly explore tools and create dozens of prompt-response pairs. Prompts should reflect realistic use cases and data sources (internal knowledge bases, microservices). Avoid overly simplistic "sandbox" environments.

**Strong evaluation tasks:**
- Schedule a meeting with Jane next week to discuss the latest Acme Corp project. Attach notes from our last planning meeting and reserve a conference room.
- Customer ID 9182 reported being charged three times for one purchase. Find all relevant log entries and determine if other customers were affected.
- Customer Sarah Chen submitted a cancellation request. Prepare a retention offer including: (1) why they're leaving, (2) the most compelling retention offer, and (3) risk factors.

**Weaker tasks:**
- Schedule a meeting with jane@acme.corp next week.
- Search payment logs for `purchase_complete` and `customer_id=9182`.
- Find the cancellation request by Customer ID 45892.

Each prompt should pair with a verifiable response or outcome. Verifiers can range from simple string matching to Claude-based judgment. Avoid overly strict verifiers that reject correct responses due to formatting or punctuation differences.

#### Running the Evaluation

Use simple agentic loops (while-loops alternating between LLM API and tool calls)—one loop per task. Each agent should receive a single task prompt and your tools.

Instruct agents to output reasoning and feedback blocks *before* tool calls to trigger chain-of-thought behaviors, potentially increasing effectiveness.

Enable "interleaved thinking" with Claude to probe why agents do or don't call certain tools and highlight specific improvement areas.

Collect metrics beyond accuracy: individual tool call runtime, total tool calls, token consumption, and tool errors. These reveal common workflows and consolidation opportunities.

#### Analyzing Results

Agents are helpful partners for spotting issues—from contradictory descriptions to inefficient implementations. However, remember that what agents *omit* can be more important than what they include.

Observe where agents get confused. Read through reasoning and feedback (or chain-of-thought) to identify rough edges. Review raw transcripts to catch behaviors not explicitly described in the agent's reasoning. Remember evaluation agents don't necessarily know correct answers and strategies.

Analyze tool calling metrics. Redundant tool calls suggest pagination or token limit adjustments. Parameter errors suggest clearer descriptions or examples are needed.

### Collaborating with Agents

Let agents analyze results and improve tools. Concatenate evaluation transcripts and paste them into Claude Code. Claude excels at analyzing transcripts and refactoring multiple tools simultaneously—ensuring implementations and descriptions remain self-consistent during changes.

Most guidance in this article came from repeatedly optimizing internal tool implementations with Claude Code, using real projects, documents, and messages. Held-out test sets prevented overfitting to training evaluations.

## Principles for Writing Effective Tools

### Choosing the Right Tools for Agents

More tools don't always yield better outcomes. A common mistake is wrapping existing software functionality without considering whether tools suit agents.

Agents have distinct "affordances" compared to traditional software—different ways of perceiving potential actions. LLM agents have limited context, while computer memory is cheap and abundant.

For searching a contact in an address book: traditional software efficiently processes contacts one at a time. An agent using a tool returning *all* contacts must read each token-by-token, wasting limited context on irrelevant information. The better approach is filtering first (alphabetically, by name).

**Build thoughtful tools targeting high-impact workflows matching evaluation tasks:**

Instead of `list_users`, `list_events`, and `create_event`, implement `schedule_event` which finds availability and schedules.

Instead of `read_logs`, implement `search_logs` returning only relevant lines and surrounding context.

Instead of `get_customer_by_id`, `list_transactions`, and `list_notes`, implement `get_customer_context` compiling all recent, relevant customer information at once.

Each tool must have a clear, distinct purpose. Tools should enable agents to subdivide and solve tasks similarly to humans with the same resources, simultaneously reducing intermediate output context consumption.

Too many or overlapping tools distract agents from efficient strategies. Selective tool planning yields significant benefits.

### Namespacing Your Tools

Agents gain access to dozens of MCP servers and hundreds of tools, including those from other developers. Overlapping tools with vague purposes confuse agents.

Namespacing (grouping related tools under common prefixes) delineates boundaries. Examples include:
- By service: `asana_search`, `jira_search`
- By resource: `asana_projects_search`, `asana_users_search`

Prefix versus suffix-based namespacing has non-trivial evaluation effects. Choose schemes based on your evaluations.

Agents might call wrong tools, use correct tools with wrong parameters, call too few tools, or process responses incorrectly. Implementing tools whose names reflect natural task subdivisions simultaneously reduces context-loaded tools and descriptions, offloading computation from agent context into tool calls themselves.

### Returning Meaningful Context From Your Tools

Tool implementations should return only high-signal information. Prioritize contextual relevance over flexibility; avoid low-level technical identifiers (`uuid`, `256px_image_url`, `mime_type`). Prefer fields like `name`, `image_url`, `file_type`—more likely to inform downstream actions.

Agents grapple with natural language names, terms, or identifiers significantly better than cryptic identifiers. Resolving arbitrary alphanumeric UUIDs to semantically meaningful language or 0-indexed IDs significantly improves Claude's retrieval precision by reducing hallucinations.

Agents sometimes need both natural language and technical identifiers (e.g., `search_user(name='jane')` → `send_message(id=12345)`). Enable this by exposing a simple `response_format` enum parameter allowing agents to control whether tools return `"concise"` or `"detailed"` responses.

```
enum ResponseFormat {
   DETAILED = "detailed",
   CONCISE = "concise"
}
```

**Detailed responses** include comprehensive information and IDs needed for follow-up tool calls (206 tokens in the example).

**Concise responses** return only essential content, excluding IDs (72 tokens in the example).

Even tool response structure—XML, JSON, or Markdown—impacts evaluation performance. Optimal structures vary by task and agent. Select based on your evaluation.

### Optimizing Tool Responses for Token Efficiency

Quality context matters; so does quantity. Implement pagination, range selection, filtering, and/or truncation with sensible defaults for responses consuming significant context. Claude Code restricts tool responses to 25,000 tokens by default.

If truncating responses, steer agents with helpful instructions. Encourage pursuing token-efficient strategies like many small, targeted searches instead of one broad search. If errors occur, prompt-engineer responses to clearly communicate specific, actionable improvements rather than opaque error codes.

**Example truncated response guidance:**
"Results limited to 10 entries. Use filters or pagination for specific data."

**Poor error response:**
`Error: ValidationError on line 42`

**Helpful error response:**
"Parameter 'user_id' must be numeric. Example: `user_id=12345`. Received: `user_id=abc`"

Tool truncation and error responses steer agents toward token-efficient behaviors or provide correctly formatted input examples.

### Prompt-Engineering Your Tool Descriptions

Prompt-engineering tool descriptions and specs is one of the most effective improvement methods. These descriptions load into agent context, collectively steering agents toward effective behaviors.

Describe tools as you would to a new team hire. Make implicit context explicit—specialized query formats, niche terminology, resource relationships. Avoid ambiguity by clearly describing expected inputs and outputs with strict data models. Use unambiguous parameter names: `user_id` instead of `user`.

Your evaluation measures prompt engineering impact with confidence. Even small refinements yield dramatic improvements. Claude Sonnet 3.5 achieved state-of-the-art SWE-bench Verified performance after precise tool description refinements, dramatically reducing errors and improving completion rates.

## Looking Ahead

Building effective agent tools requires reorienting software development from predictable, deterministic patterns to non-deterministic approaches.

Through iterative, evaluation-driven processes, consistent patterns emerge: effective tools are intentionally and clearly defined, use agent context judiciously, combine in diverse workflows, and intuitively enable agents to solve real-world tasks.

Future mechanisms for agent-world interaction will evolve—from MCP protocol updates to underlying LLM upgrades. Systematic, evaluation-driven tool improvement ensures agents and their tools evolve together.

---

*Written by Ken Aizawa with contributions from Research, MCP, Product Engineering, Marketing, Design, and Applied AI teams.*
