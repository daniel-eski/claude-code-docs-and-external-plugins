# Effective Context Engineering for AI Agents

> Source: https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
> Published: 2025

---

## Introduction

After years of prompt engineering dominating applied AI discussions, **context engineering** has emerged as the new focus area. Rather than optimizing individual prompts, this approach addresses the broader question: "what configuration of context is most likely to generate our model's desired behavior?"

Context represents "the set of tokens included when sampling from a large-language model." The engineering challenge involves maximizing token utility within LLM constraints to consistently achieve desired outcomes. This requires **thinking in context**—considering the holistic state available to the model and potential behaviors it might produce.

## Context Engineering vs. Prompt Engineering

Anthropic views context engineering as the natural evolution of prompt engineering. While prompt engineering focuses on writing and organizing instructions for optimal outcomes, context engineering encompasses strategies for curating and maintaining optimal token sets during inference, including information beyond traditional prompts.

Early LLM engineering centered on prompt optimization for one-shot tasks. As agent capabilities expand across multiple inference turns and longer timeframes, engineers need strategies managing entire context states: system instructions, tools, Model Context Protocol (MCP) connections, external data, and message history.

Agents operating in loops generate increasingly relevant data. Context engineering represents "the art and science of curating what will go into the limited context window from that constantly evolving universe of possible information."

## Why Context Engineering Matters

LLMs experience performance degradation similar to humans losing focus. Research on "needle-in-a-haystack" benchmarking revealed **context rot**: as context tokens increase, the model's information recall accuracy decreases.

Context functions as a finite resource with diminishing returns. Like humans with limited working memory, LLMs possess an "attention budget" that depletes with each new token, necessitating careful curation.

### Architectural Constraints

LLMs employ transformer architecture, enabling every token to attend to every other token, creating n² pairwise relationships. This attention mechanism becomes stretched thin as context lengthens, creating tension between context size and focus.

Models develop attention patterns from training data where shorter sequences predominate. Consequently, they possess less experience and fewer specialized parameters for context-wide dependencies.

Techniques like position encoding interpolation allow models to handle longer sequences, though with degradation in position understanding. This creates a performance gradient rather than cliff—models remain capable at longer contexts but show reduced precision for retrieval and reasoning versus shorter contexts.

## Anatomy of Effective Context

Good context engineering finds "the smallest possible set of high-signal tokens that maximize the likelihood of some desired outcome."

### System Prompts

System prompts should employ "simple, direct language that presents ideas at the right altitude for the agent." This represents the Goldilocks zone between two failure modes:

- **Extreme 1**: Hardcoded complex logic creating fragile, maintenance-heavy prompts
- **Extreme 2**: Vague guidance failing to provide concrete behavioral signals

The optimal approach balances specificity with flexibility, offering concrete guidance while allowing strong heuristics.

Organize prompts into distinct sections using XML tags or Markdown headers: `<background_information>`, `<instructions>`, `## Tool guidance`, `## Output description`. Strive for minimal information fully outlining expected behavior—"minimal does not necessarily mean short; you still need to give the agent sufficient information."

Start with minimal prompts using the best available model, then iteratively add instructions based on failure modes discovered during testing.

### Tools

Tools enable agents to operate within their environment and inject new context dynamically. Because tools define the agent-environment contract, they must promote efficiency both through token-efficient returns and encouraging efficient agent behaviors.

Tools should be self-contained, error-robust, and extremely clear regarding intended use. Input parameters must be descriptive, unambiguous, and aligned with model strengths.

A common failure: bloated tool sets covering excessive functionality or creating ambiguous selection points. "If a human engineer can't definitively say which tool should be used in a given situation, an AI agent can't be expected to do better."

### Examples (Few-Shot Prompting)

Providing diverse, canonical examples effectively demonstrates expected behavior. "For an LLM, examples are the 'pictures' worth a thousand words."

Rather than exhaustive edge case lists, curate examples showing expected behavior across scenarios.

## Context Retrieval and Agentic Search

The field converges on a simple agent definition: LLMs autonomously using tools in a loop. As underlying models improve, agent autonomy scales proportionally.

### "Just-in-Time" Context

Rather than pre-processing all relevant data, agents employing "just-in-time" approaches maintain lightweight identifiers (file paths, stored queries, web links) and dynamically load data into context via tools at runtime.

Claude Code exemplifies this approach for complex database analysis. The model writes targeted queries, stores results, and uses Bash commands like head and tail to analyze large datasets without loading full objects into context. This mirrors human cognition: we don't memorize entire corpuses but leverage external organization systems like file systems and bookmarks.

### Metadata as Behavioral Guidance

Reference metadata efficiently refines behavior. File naming conventions, folder hierarchies, and timestamps provide signals helping both humans and agents understand information utilization.

### Progressive Disclosure

Autonomous navigation enables progressive discovery—agents incrementally discover relevant context through exploration. Each interaction informs subsequent decisions: file sizes suggest complexity, naming conventions hint purpose, timestamps proxy relevance. Agents assemble understanding layer-by-layer, maintaining only necessary working memory while leveraging note-taking for persistence.

### Trade-offs and Hybrid Approaches

Runtime exploration trades speed for autonomy. Without proper engineering, agents waste context through tool misuse, dead-end chasing, or missing key information.

The most effective agents often employ hybrid strategies: retrieving some data upfront for speed while pursuing further autonomous exploration discretionally. Task characteristics determine the optimal autonomy level. Claude Code demonstrates this: CLAUDE.md files drop into context initially, while primitives like glob and grep enable just-in-time navigation.

As model capabilities improve, agentic design trends toward "letting intelligent models act intelligently, with progressively less human curation."

## Context Engineering for Long-Horizon Tasks

Tasks spanning tens of minutes to multiple hours of continuous work (like large codebase migrations or comprehensive research) exceed context windows. LLMs require specialized techniques working around this limitation.

Rather than waiting for larger context windows, address context pollution and relevance concerns directly through three approaches:

### Compaction

Compaction summarizes conversations nearing context limits and reinitializes new windows with summaries. This serves as the primary context engineering lever for long-term coherence, distilling contents in high-fidelity fashion.

Claude Code implements this by passing message history to the model for summarization and compression of critical details. The model preserves architectural decisions, unresolved bugs, and implementation details while discarding redundant outputs. The agent continues with compressed context plus recently accessed files, providing continuity without window limitations.

Compaction artistry lies in selecting what to keep versus discard. Overly aggressive approaches lose subtle but critical context. Recommend carefully tuning prompts on complex agent traces: maximize recall first ensuring capture of relevant information, then iterate improving precision by eliminating superfluous content.

Tool result clearing represents the safest, lightest-touch compaction form, recently launched as a Claude Developer Platform feature.

### Structured Note-Taking (Agentic Memory)

Agents regularly write notes persisted outside context windows, later retrieving them when needed. This provides persistent memory with minimal overhead.

Like Claude Code creating to-do lists or custom agents maintaining NOTES.md files, this pattern enables progress tracking across complex tasks while maintaining critical context and dependencies otherwise lost across dozens of tool calls.

Claude playing Pokémon demonstrates memory's transformative power beyond coding domains. The agent maintains precise tallies across thousands of steps—tracking specific objectives with exact metrics. Without explicit memory prompting, it develops maps of explored regions, remembers unlocked achievements, and maintains strategic combat notes helping identify effective attack strategies.

After context resets, agents read their notes and continue multi-hour sequences. This coherence across summarization steps enables long-horizon strategies impossible when keeping everything in-context.

As part of the Sonnet 4.5 launch, Anthropic released a memory tool in public beta enabling agents to store and consult information outside context through file-based systems. This allows agents building knowledge bases over time, maintaining project state across sessions, and referencing previous work without complete context retention.

### Sub-Agent Architectures

Rather than one agent maintaining state across entire projects, specialized sub-agents handle focused tasks with clean context windows. The main agent coordinates high-level planning while sub-agents perform deep technical work. Each explores extensively using tens of thousands of tokens, returning condensed 1,000-2,000 token summaries.

This achieves clear separation of concerns—detailed search context remains isolated within sub-agents while the lead agent synthesizes results. This pattern showed substantial improvements over single-agent systems on complex research tasks.

### Choosing Approaches

Selection depends on task characteristics:

- **Compaction**: Maintains conversational flow for extensive back-and-forth
- **Note-taking**: Excels for iterative development with clear milestones
- **Multi-agent**: Handles complex research and analysis with parallel exploration benefits

Even as models improve, maintaining coherence across extended interactions remains central to building effective agents.

## Conclusion

Context engineering represents fundamental shift in building with LLMs. As capabilities expand, the challenge transcends crafting perfect prompts—it requires "thoughtfully curating what information enters the model's limited attention budget at each step."

Whether implementing compaction for long-horizon tasks, designing token-efficient tools, or enabling agent environment exploration, one principle endures: identify the smallest high-signal token set maximizing desired outcome likelihood.

These techniques will evolve as models improve. Smarter models require less prescriptive engineering, enabling greater autonomy. However, treating context as precious, finite resources remains central to building reliable, effective agents.

---

**Written by**: Anthropic's Applied AI team: Prithvi Rajasekaran, Ethan Dixon, Carly Ryan, and Jeremy Hadfield, with contributions from Rafi Ayub, Hannah Moran, Cal Rueb, and Connor Jennings.
