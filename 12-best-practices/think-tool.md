# The "Think" Tool: Enabling Claude to Stop and Think in Complex Tool Use Situations

> Source: https://www.anthropic.com/engineering/claude-think-tool
> Published: 2025

---

## Overview

Anthropic has introduced a "think" tool that enables Claude to pause and reflect during complex problem-solving tasks. This technique creates dedicated reasoning space within tool use workflows, delivering substantial improvements in policy adherence and multi-step decision-making.

## What Is the "Think" Tool?

The "think" tool differs from extended thinking capabilities. While extended thinking operates before Claude generates responses, the "think" tool functions as an intermediate step during response generation. As described in the documentation, it allows Claude to "add a step to stop and think about whether it has all the information it needs to move forward."

This approach proves particularly valuable when Claude must process external information from tool results rather than relying solely on initial user queries.

## Performance Results

### τ-Bench Evaluation

Testing on τ-bench revealed dramatic improvements across customer service domains:

- **Airline domain**: The optimized "think" tool achieved 0.570 on pass¹ metrics versus 0.370 baseline—representing a 54% relative improvement
- **Retail domain**: The "think" tool alone achieved 0.812 compared to 0.783 baseline performance

The combination of the "think" tool with domain-specific prompting delivered the strongest results, particularly for complex policy scenarios.

### SWE-Bench Results

When integrated into code evaluation tasks, the "think" tool improved performance by 1.6% on average, demonstrating generalizability beyond customer service applications.

## When to Use the "Think" Tool

The tool proves most effective for:

1. **Tool output analysis** - Processing previous results before advancing
2. **Policy-heavy environments** - Following detailed compliance guidelines
3. **Sequential decision-making** - Where each step builds on prior actions

## When Not to Use

Avoid implementing the "think" tool for:

- Non-sequential tool calls or single operations
- Simple instruction-following without complex constraints

## Implementation Best Practices

**Strategic prompting with examples**: Provide domain-specific guidance showing expected reasoning patterns, including how to verify policy compliance and check information completeness.

**System prompt placement**: For complex instructions, include "think" tool guidance in system prompts rather than tool descriptions alone.

## Key Insights

"Simply making the tool available might improve performance somewhat, but pairing it with optimized prompting yielded dramatically better results for difficult domains."

The research demonstrates minimal implementation overhead combined with substantial cognitive benefits for agentic systems handling intricate workflows.
