# How We Built Our Multi-Agent Research System

> Source: https://www.anthropic.com/engineering/multi-agent-research-system
> Published: 2025

---

## Overview

Anthropic's Research feature employs a multi-agent architecture where "multiple agents (LLMs autonomously using tools in a loop) work together" to explore complex topics effectively. The system evolved from prototype to production, revealing critical lessons about system design and prompt engineering.

## Benefits of Multi-Agent Systems

### Why Multi-Agents Excel at Research

Research involves open-ended problems where predetermined paths fail. Traditional single-agent systems cannot handle the dynamic, path-dependent nature of investigation. Multi-agent systems provide the flexibility needed to "pivot or explore tangential connections as the investigation unfolds."

The architecture leverages parallelization for information compression. Subagents operate simultaneously with separate context windows, exploring different question aspects before condensing findings for the lead researcher. This "separation of concerns—distinct tools, prompts, and exploration trajectories—reduces path dependency."

### Performance Gains

Internal evaluations demonstrated significant improvements. A multi-agent system with Claude Opus 4 as lead agent and Claude Sonnet 4 subagents "outperformed single-agent Claude Opus 4 by 90.2% on our internal research eval." When tasked with identifying all S&P 500 Information Technology company board members, the multi-agent system succeeded while single-agent approaches failed through sequential searching.

### Token Economics

Analysis revealed "three factors explained 95% of the performance variance" in the BrowseComp evaluation: token usage (80% of variance), tool call count, and model choice. However, multi-agent systems consume significant resources—approximately "15× more tokens than chats," requiring tasks where value justifies increased costs.

## Architecture Overview

### Orchestrator-Worker Pattern

The system uses a lead agent coordinating with specialized subagents operating in parallel. When users submit queries, "the lead agent analyzes it, develops a strategy, and spawns subagents to explore different aspects simultaneously."

### Dynamic vs. Static Retrieval

Unlike traditional RAG that "fetches some set of chunks most similar to an input query," this architecture employs "multi-step search that dynamically finds relevant information, adapts to new findings, and analyzes results."

### Workflow Process

The LeadResearcher enters an iterative cycle: thinking through approaches, saving plans to external memory (since context windows exceed 200,000 tokens), and creating specialized subagents. Each subagent performs independent web searches using "interleaved thinking" to evaluate results. The system continues researching until sufficient information accumulates, then passes findings to a CitationAgent for proper attribution before returning results.

## Prompt Engineering Principles

### Key Strategies

**1. Mental Modeling**: Developers must "understand their effects" on agent behavior. Anthropic used Console simulations to observe agents step-by-step, revealing failures like "agents continuing when they already had sufficient results" or using "overly verbose search queries."

**2. Delegation Specificity**: Lead agents require "an objective, an output format, guidance on the tools and sources to use, and clear task boundaries." Vague instructions like "research the semiconductor shortage" caused subagent duplication and misalignment.

**3. Effort Scaling**: Explicit guidelines prevent overinvestment. Simple fact-finding uses one agent with 3-10 tool calls; complex research might employ over 10 subagents with divided responsibilities.

**4. Tool Design**: "Agent-tool interfaces are as critical as human-computer interfaces." Clear descriptions and matched tool selection prevent agents from searching web-only data in Slack or attempting inappropriate tool combinations.

**5. Self-Improvement**: Claude 4 models can diagnose failure modes and suggest improvements. A dedicated tool-testing agent "rewrites the tool description to avoid failures" after dozens of attempts, yielding "40% decrease in task completion time."

**6. Search Strategy**: Agents should "start with short, broad queries, evaluate what's available, then progressively narrow focus," mirroring expert human research approaches.

**7. Extended Thinking**: Using extended and interleaved thinking modes provides "controllable scratchpad" functionality for planning, tool assessment, and quality evaluation. Testing showed it "improved instruction-following, reasoning, and efficiency."

**8. Parallelization**: Spinning up 3-5 subagents simultaneously rather than serially, combined with 3+ parallel tool calls per subagent, "cut research time by up to 90% for complex queries."

## Evaluation Approaches

### Small-Scale Testing

Starting evaluations immediately with approximately 20 representative queries proved effective. Early development shows "dramatic impacts" where "a prompt tweak might boost success rates from 30% to 80%," making small samples sufficient for detecting meaningful changes.

### LLM-as-Judge Methodology

Research outputs resist programmatic evaluation due to free-form text and multiple valid answers. An LLM judge evaluated outputs against rubric criteria: factual accuracy, citation accuracy, completeness, source quality, and tool efficiency. A "single LLM call with a single prompt outputting scores from 0.0-1.0" proved "most consistent and aligned with human judgements."

### Human Validation

Manual testing discovers edge cases automated evaluation misses. Early agents "consistently chose SEO-optimized content farms over authoritative but less highly-ranked sources like academic PDFs." Human testers identified this bias, prompting source quality heuristic additions to prompts.

## Production Reliability Challenges

### Stateful Error Compounding

Agents maintain state across many tool calls. "Without effective mitigations, minor system failures can be catastrophic for agents." Rather than restarting from beginning, the system resumes from error points and "lets the agent know when a tool is failing and letting it adapt works surprisingly well."

### Non-Deterministic Debugging

Agent non-determinism between runs complicates troubleshooting. "Full production tracing let us diagnose why agents failed and fix issues systematically." Monitoring "agent decision patterns and interaction structures" without accessing conversation contents maintains privacy while enabling root cause analysis.

### Deployment Coordination

Continuous agent operation requires careful updates. The system uses "rainbow deployments" to "gradually shift traffic from old to new versions while keeping both running simultaneously," preventing disruption to in-flight agents.

### Synchronous Execution Bottlenecks

Currently, lead agents execute subagents synchronously, waiting for completion before proceeding. This simplifies coordination but creates information flow bottlenecks. Asynchronous execution would enable additional parallelism but introduces complexity in "result coordination, state consistency, and error propagation."

## Practical Tips

### End-State Evaluation

For agents modifying persistent state across multi-turn conversations, "evaluate whether it achieved the correct final state" rather than validating every intermediate step, acknowledging "agents may find alternative paths to the same goal."

### Context Management at Scale

Production agents spanning hundreds of turns require intelligent memory strategies. Agents "summarize completed work phases and store essential information in external memory before proceeding to new tasks," preventing context overflow while preserving coherence.

### Filesystem Output Optimization

Subagent outputs bypassing the lead agent improve fidelity and performance. Rather than routing everything through coordinators, implement "artifact systems where specialized agents can create outputs that persist independently," reducing "token overhead from copying large outputs through conversation history."

## Real-World Impact

User feedback indicates transformative value: discovering overlooked business opportunities, navigating complex healthcare decisions, resolving technical challenges, and "save up to days of work by uncovering research connections they wouldn't have found alone."

The most common use cases include developing software systems across specialized domains (10%), developing professional content (8%), business strategy development (8%), academic research support (7%), and information verification (5%).
