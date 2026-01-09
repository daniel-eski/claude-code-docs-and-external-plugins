# Tool Use with Claude

> Source: https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview
> Retrieved: January 2026

---

Claude is capable of interacting with tools and functions, allowing you to extend Claude's capabilities to perform a wider variety of tasks.

## How Tool Use Works

Claude supports two types of tools:

1. **Client tools**: Tools that execute on your systems, which include:
   - User-defined custom tools that you create and implement
   - Anthropic-defined tools like computer use and text editor that require client implementation

2. **Server tools**: Tools that execute on Anthropic's servers, like the web search and web fetch tools. These tools must be specified in the API request but don't require implementation on your part.

### Client Tools Workflow

1. **Provide Claude with tools and a user prompt**
   - Define client tools with names, descriptions, and input schemas in your API request
   - Include a user prompt that might require these tools

2. **Claude decides to use a tool**
   - Claude assesses if any tools can help with the user's query
   - If yes, Claude constructs a properly formatted tool use request
   - For client tools, the API response has a `stop_reason` of `tool_use`

3. **Execute the tool and return results**
   - Extract the tool name and input from Claude's request
   - Execute the tool code on your system
   - Return the results in a new `user` message containing a `tool_result` content block

4. **Claude uses tool result to formulate a response**
   - Claude analyzes the tool results to craft its final response

### Server Tools Workflow

1. **Provide Claude with tools and a user prompt**
   - Server tools, like web search and web fetch, have their own parameters

2. **Claude executes the server tool**
   - Claude assesses if a server tool can help with the user's query
   - If yes, Claude executes the tool, and the results are automatically incorporated

3. **Claude uses the server tool result to formulate a response**
   - No additional user interaction is needed for server tool execution

## Using MCP Tools with Claude

If you're building an application that uses the Model Context Protocol (MCP), you can use tools from MCP servers directly with Claude's Messages API. MCP tool definitions use a schema format that's similar to Claude's tool format. You just need to rename `inputSchema` to `input_schema`.

### Converting MCP Tools to Claude Format

When you build an MCP client and call `list_tools()` on an MCP server, you'll receive tool definitions with an `inputSchema` field. To use these tools with Claude, convert them:

```python
async def get_claude_tools(mcp_session):
    """Convert MCP tools to Claude's tool format."""
    mcp_tools = await mcp_session.list_tools()

    claude_tools = []
    for tool in mcp_tools.tools:
        claude_tools.append({
            "name": tool.name,
            "description": tool.description or "",
            "input_schema": tool.inputSchema  # Rename inputSchema to input_schema
        })

    return claude_tools
```

## Tool Use Patterns

### Single Tool Example

Provide a tool like `get_weather` with a description and input schema. Claude will call it when relevant and return results.

### Parallel Tool Use

Claude can call multiple tools in parallel within a single response, which is useful for tasks that require multiple independent operations. All `tool_use` blocks are included in a single assistant message, and all corresponding `tool_result` blocks must be provided in the subsequent user message.

### Multiple Tools

You can provide Claude with multiple tools to choose from in a single request. Claude may use them sequentially or in parallel depending on the task.

### Sequential Tools

Some tasks may require calling multiple tools in sequence, using the output of one tool as the input to another. Claude will call one tool at a time in such cases.

### Missing Information

If the user's prompt doesn't include enough information to fill all the required parameters for a tool, Claude Opus is much more likely to recognize that a parameter is missing and ask for it. Claude Sonnet may also ask, especially when prompted to think before outputting a tool request.

## Chain of Thought Tool Use

By default, Claude Opus is prompted to think before it answers a tool use query. For Sonnet or Haiku to better assess the user query before making tool calls:

```
Answer the user's request using relevant tools (if they are available). Before calling a tool, do some analysis. First, think about which of the provided tools is the relevant tool to answer the user's request. Second, go through each of the required parameters of the relevant tool and determine if the user has directly provided or given enough information to infer a value. When deciding if the parameter can be inferred, carefully consider all the context to see if it supports a specific value. If all of the required parameters are present or can be reasonably inferred, proceed with the tool call. BUT, if one of the values for a required parameter is missing, DO NOT invoke the function (not even with fillers for the missing params) and instead, ask the user to provide the missing parameters.
```

## Pricing

Tool use requests are priced based on:
1. The total number of input tokens sent to the model (including in the `tools` parameter)
2. The number of output tokens generated
3. For server-side tools, additional usage-based pricing

The additional tokens from tool use come from:
- The `tools` parameter in API requests (tool names, descriptions, and schemas)
- `tool_use` content blocks in API requests and responses
- `tool_result` content blocks in API requests

When you use `tools`, a special system prompt for the model is automatically included which enables tool use. The number of tool use tokens required varies by model (typically 264-530 tokens depending on model and tool choice).

## Best Practices

- **Clear tool descriptions**: Describe tools as you would to a new team member
- **Strict input schemas**: Use unambiguous parameter names and types
- **Error handling**: Return helpful error messages that guide correction
- **Token efficiency**: Implement pagination and filtering for large responses
- **Namespacing**: Group related tools under common prefixes (e.g., `asana_search`, `jira_search`)

## Next Steps

Explore ready-to-implement tool use code examples in the cookbooks:
- Calculator Tool
- Customer Service Agent
- JSON Extractor
