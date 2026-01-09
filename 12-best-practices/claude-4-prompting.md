# Claude 4.x Prompting Best Practices

> Source: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices
> Retrieved: January 2026

---

This guide provides specific prompt engineering techniques for Claude 4.x models, with specific guidance for Sonnet 4.5, Haiku 4.5, and Opus 4.5. These models have been trained for more precise instruction following than previous generations of Claude models.

## General Principles

### Be Explicit with Your Instructions

Claude 4.x models respond well to clear, explicit instructions. Being specific about your desired output can help enhance results. Customers who desire the "above and beyond" behavior from previous Claude models might need to more explicitly request these behaviors with newer models.

**Less effective:**
```
Create an analytics dashboard
```

**More effective:**
```
Create an analytics dashboard. Include as many relevant features and interactions as possible. Go beyond the basics to create a fully-featured implementation.
```

### Add Context to Improve Performance

Providing context or motivation behind your instructions, such as explaining to Claude why such behavior is important, can help Claude 4.x models better understand your goals and deliver more targeted responses.

**Less effective:**
```
NEVER use ellipses
```

**More effective:**
```
Your response will be read aloud by a text-to-speech engine, so never use ellipses since the text-to-speech engine will not know how to pronounce them.
```

Claude is smart enough to generalize from the explanation.

### Be Vigilant with Examples & Details

Claude 4.x models pay close attention to details and examples as part of their precise instruction following capabilities. Ensure that your examples align with the behaviors you want to encourage and minimize behaviors you want to avoid.

### Long-Horizon Reasoning and State Tracking

Claude 4.5 models excel at long-horizon reasoning tasks with exceptional state tracking capabilities. It maintains orientation across extended sessions by focusing on incremental progress—making steady advances on a few things at a time rather than attempting everything at once.

#### Context Awareness and Multi-Window Workflows

Claude 4.5 models feature context awareness, enabling the model to track its remaining context window (i.e. "token budget") throughout a conversation. This enables Claude to execute tasks and manage context more effectively by understanding how much space it has to work.

**Managing context limits:**

If you are using Claude in an agent harness that compacts context or allows saving context to external files (like in Claude Code), add this information to your prompt:

```
Your context window will be automatically compacted as it approaches its limit, allowing you to continue working indefinitely from where you left off. Therefore, do not stop tasks early due to token budget concerns. As you approach your token budget limit, save your current progress and state to memory before the context window refreshes. Always be as persistent and autonomous as possible and complete tasks fully, even if the end of your budget is approaching. Never artificially stop any task early regardless of the context remaining.
```

#### Multi-Context Window Workflows

For tasks spanning multiple context windows:

1. **Use a different prompt for the very first context window**: Use the first context window to set up a framework (write tests, create setup scripts), then use future context windows to iterate on a todo-list.

2. **Have the model write tests in a structured format**: Ask Claude to create tests before starting work and keep track of them in a structured format (e.g., `tests.json`).

3. **Set up quality of life tools**: Encourage Claude to create setup scripts (e.g., `init.sh`) to gracefully start servers, run test suites, and linters.

4. **Starting fresh vs compacting**: When a context window is cleared, consider starting with a brand new context window rather than using compaction. Claude 4.5 models are extremely effective at discovering state from the local filesystem.

5. **Provide verification tools**: As the length of autonomous tasks grows, Claude needs to verify correctness without continuous human feedback.

6. **Encourage complete usage of context**: Prompt Claude to efficiently complete components before moving on.

#### State Management Best Practices

- **Use structured formats for state data**: When tracking structured information (like test results or task status), use JSON or other structured formats
- **Use unstructured text for progress notes**: Freeform progress notes work well for tracking general progress
- **Use git for state tracking**: Git provides a log of what's been done and checkpoints that can be restored
- **Emphasize incremental progress**: Explicitly ask Claude to keep track of its progress and focus on incremental work

### Communication Style

Claude 4.5 models have a more concise and natural communication style compared to previous models:

- **More direct and grounded**: Provides fact-based progress reports rather than self-celebratory updates
- **More conversational**: Slightly more fluent and colloquial, less machine-like
- **Less verbose**: May skip detailed summaries for efficiency unless prompted otherwise

## Guidance for Specific Situations

### Balance Verbosity

Claude 4.5 models tend toward efficiency and may skip verbal summaries after tool calls. If you want Claude to provide updates as it works:

```
After completing a task that involves tool use, provide a quick summary of the work you've done.
```

### Tool Usage Patterns

Claude 4.5 models benefit from explicit direction to use specific tools. If you say "can you suggest some changes," it will sometimes provide suggestions rather than implementing them.

**Less effective (Claude will only suggest):**
```
Can you suggest some changes to improve this function?
```

**More effective (Claude will make the changes):**
```
Change this function to improve its performance.
```

To make Claude more proactive about taking action by default:

```
By default, implement changes rather than only suggesting them. If the user's intent is unclear, infer the most useful likely action and proceed, using tools to discover any missing details instead of guessing.
```

### Control the Format of Responses

1. **Tell Claude what to do instead of what not to do**
   - Instead of: "Do not use markdown in your response"
   - Try: "Your response should be composed of smoothly flowing prose paragraphs."

2. **Use XML format indicators**
   - Try: "Write the prose sections of your response in \<smoothly_flowing_prose_paragraphs\> tags."

3. **Match your prompt style to the desired output**

4. **Use detailed prompts for specific formatting preferences**

### Subagent Orchestration

Claude 4.5 models demonstrate significantly improved native subagent orchestration capabilities. These models can recognize when tasks would benefit from delegating work to specialized subagents and do so proactively without requiring explicit instruction.

### Optimize Parallel Tool Calling

Claude 4.x models excel at parallel tool execution, with Sonnet 4.5 being particularly aggressive in firing off multiple operations simultaneously.

```
If you intend to call multiple tools and there are no dependencies between the tool calls, make all of the independent tool calls in parallel. Prioritize calling tools simultaneously whenever the actions can be done in parallel rather than sequentially.
```

### Reduce File Creation in Agentic Coding

Claude 4.x models may sometimes create new files for testing and iteration purposes. If you'd prefer to minimize net new file creation:

```
If you create any temporary new files, scripts, or helper files for iteration, clean up these files by removing them at the end of the task.
```

### Avoid Overeagerness and File Creation

Claude Opus 4.5 has a tendency to overengineer by creating extra files, adding unnecessary abstractions, or building in flexibility that wasn't requested:

```
Avoid over-engineering. Only make changes that are directly requested or clearly necessary. Keep solutions simple and focused.

Don't add features, refactor code, or make "improvements" beyond what was asked. A bug fix doesn't need surrounding code cleaned up. A simple feature doesn't need extra configurability.

Don't add error handling, fallbacks, or validation for scenarios that can't happen. Trust internal code and framework guarantees. Only validate at system boundaries (user input, external APIs).

Don't create helpers, utilities, or abstractions for one-time operations. Don't design for hypothetical future requirements.
```

### Frontend Design

Claude 4.x models excel at building complex, real-world web applications with strong frontend design. To create distinctive, creative frontends:

- **Typography**: Choose fonts that are beautiful, unique, and interesting. Avoid generic fonts like Arial and Inter.
- **Color & Theme**: Commit to a cohesive aesthetic. Use CSS variables for consistency.
- **Motion**: Use animations for effects and micro-interactions.
- **Backgrounds**: Create atmosphere and depth rather than defaulting to solid colors.

### Encouraging Code Exploration

Claude Opus 4.5 is highly capable but can be overly conservative when exploring code:

```
ALWAYS read and understand relevant files before proposing code edits. Do not speculate about code you have not inspected. If the user references a specific file/path, you MUST open and inspect it before explaining or proposing fixes.
```

### Minimizing Hallucinations in Agentic Coding

```
Never speculate about code you have not opened. If the user references a specific file, you MUST read the file before answering. Make sure to investigate and read relevant files BEFORE answering questions about the codebase.
```

## Migration Considerations

When migrating to Claude 4.5 models:

1. **Be specific about desired behavior**: Consider describing exactly what you'd like to see in the output.

2. **Frame your instructions with modifiers**: Adding modifiers that encourage Claude to increase the quality and detail of its output can help better shape Claude's performance.

3. **Request specific features explicitly**: Animations and interactive elements should be requested explicitly when desired.
