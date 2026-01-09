# Building Effective Agents

> Source: https://www.anthropic.com/engineering/building-effective-agents
> Published: December 19, 2024

---

## Introduction

Anthropic has worked extensively with teams developing LLM agents across industries. A consistent finding emerges: "the most successful implementations weren't using complex frameworks or specialized libraries. Instead, they were building with simple, composable patterns."

## What Are Agents?

"Agent" encompasses varying definitions. Some organizations describe agents as fully autonomous systems operating independently over extended periods using various tools. Others prefer more prescriptive implementations following predefined workflows.

Anthropic categorizes these as **agentic systems** but distinguishes two types:

- **Workflows**: LLMs and tools orchestrated through predefined code paths
- **Agents**: Systems where LLMs dynamically direct their own processes and tool usage

## When (and When Not) to Use Agents

The foundational principle: find the simplest solution first, adding complexity only when necessary. Agentic systems trade latency and cost for improved performance—evaluate whether this tradeoff makes sense for your use case.

Workflows offer predictability for well-defined tasks. Agents excel when flexibility and model-driven decision-making matter at scale. For many applications, "optimizing single LLM calls with retrieval and in-context examples is usually enough."

## Framework Considerations

Available frameworks include:
- Claude Agent SDK
- Strands Agents SDK by AWS
- Rivet (drag-and-drop GUI builder)
- Vellum (GUI tool for complex workflows)

While frameworks simplify implementation, they introduce abstraction layers obscuring underlying prompts and responses. Start with LLM APIs directly—many patterns require only "a few lines of code." If using frameworks, understand the underlying mechanics thoroughly.

## Building Blocks and Patterns

### The Augmented LLM

The foundational component combines an LLM with augmentations: retrieval, tools, and memory. Focus on tailoring these capabilities to your specific use case and ensuring clear documentation for the LLM interface.

### Prompt Chaining

Decompose tasks into sequential steps where each LLM call processes previous output. Add programmatic checks at intermediate steps to maintain course.

**Best for:** Tasks decomposable into fixed subtasks, prioritizing accuracy over latency.

**Examples:**
- Generate marketing copy, then translate it
- Write document outline, verify it meets criteria, then write the full document

### Routing

Classify input and direct it to specialized follow-up tasks, enabling separation of concerns and specialized prompts.

**Best for:** Complex tasks with distinct categories handled separately, using accurate classification.

**Examples:**
- Direct customer service queries (general, refunds, technical support) to appropriate downstream processes
- Route easy questions to smaller models like Claude Haiku 4.5; hard questions to Claude Sonnet 4.5

### Parallelization

Run LLMs simultaneously on tasks and aggregate outputs programmatically. Two variations exist:

- **Sectioning**: Break tasks into independent parallel subtasks
- **Voting**: Run identical tasks multiple times for diverse outputs

**Best for:** Tasks benefiting from parallel speedup or multiple perspectives for higher confidence.

**Examples:**
- Implement guardrails with separate screening instance
- Review code for vulnerabilities across multiple prompts
- Evaluate content appropriateness with multiple evaluation angles

### Orchestrator-Workers

A central LLM dynamically breaks down tasks, delegates to worker LLMs, and synthesizes results. Unlike parallelization, subtasks are determined based on specific input rather than predefined.

**Best for:** Complex tasks with unpredictable subtask requirements.

**Examples:**
- Coding products making complex multi-file changes
- Search tasks gathering and analyzing information from multiple sources

### Evaluator-Optimizer

One LLM generates responses while another evaluates and provides feedback in loops.

**Best for:** Tasks with clear evaluation criteria where iterative refinement demonstrably improves outcomes.

**Examples:**
- Literary translation capturing nuanced meanings
- Complex search tasks requiring multiple research-analysis rounds

## Agents

Agents emerge in production as LLMs mature in key capabilities: understanding complex inputs, reasoning, planning, reliable tool use, and error recovery.

Agents begin with user commands or discussion, then operate independently after clarifying the task. They gain crucial environmental feedback at each step—tool results, code execution—to assess progress. They pause for human feedback at checkpoints or when blocked.

**Core implementation insight:** "They are typically just LLMs using tools based on environmental feedback in a loop."

**Best for:** Open-ended problems with unpredictable step counts and fixed paths unavailable.

**Considerations:** Higher costs and potential for compounding errors require extensive sandboxed testing and appropriate guardrails.

**Examples:**
- Coding agents resolving GitHub issues (SWE-bench tasks)
- Computer use reference implementation

## Three Core Principles

1. **Simplicity**: Maintain uncomplicated agent designs
2. **Transparency**: Explicitly display agent planning steps
3. **ACI Documentation**: Craft agent-computer interfaces through thorough tool documentation and testing

"Frameworks can help you get started quickly, but don't hesitate to reduce abstraction layers and build with basic components as you move to production."

## Tool Design Best Practices

Tool definitions require equal prompt engineering attention as overall prompts.

**Format selection principles:**
- Provide sufficient tokens for the model to "think" before committing
- Keep formats close to naturally occurring internet text
- Eliminate formatting overhead like maintaining line counts

**Interface optimization:**
- Include example usage, edge cases, requirements, and clear boundaries
- Use parameter names suggesting obvious usage
- Test extensively to identify and fix model mistakes
- Apply Poka-yoke principles making errors harder

During SWE-bench agent development, Anthropic "spent more time optimizing tools than the overall prompt." When discovering relative filepath mistakes post-directory changes, requiring absolute filepaths eliminated the error entirely.

## Practical Applications

### Customer Support

Combines chatbot interfaces with tool integration naturally:
- Conversations requiring external information access
- Tools pulling customer data, order history, knowledge bases
- Programmatic actions: refunds, ticket updates
- Measurable success through defined resolutions

Some companies use usage-based pricing charging only for successful resolutions, demonstrating confidence in agent effectiveness.

### Coding Agents

Software development shows remarkable LLM potential:
- Solutions verifiable through automated tests
- Agents iterating using test result feedback
- Well-defined, structured problem spaces
- Objectively measurable output quality

Agents now solve real GitHub issues in SWE-bench Verified benchmarks from pull request descriptions alone, though human review remains essential for broader system alignment.

## Summary

Success means building the right system for your needs, not the most sophisticated. Start simple: optimize individual LLM calls with retrieval and in-context examples. Add multi-step agentic systems only when simpler approaches fall short. Through measurement, iteration, and thoughtful design of the human-computer interface, you create agents that are powerful, reliable, maintainable, and trustworthy.
