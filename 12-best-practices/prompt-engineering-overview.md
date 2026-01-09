# Prompt Engineering Overview

> Source: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview
> Retrieved: January 2026

---

## Before Prompt Engineering

This guide assumes that you have:
1. A clear definition of the success criteria for your use case
2. Some ways to empirically test against those criteria
3. A first draft prompt you want to improve

If not, spend time establishing that first. Check out "Define your success criteria" and "Create strong empirical evaluations" for tips and guidance.

## When to Prompt Engineer

This guide focuses on success criteria that are controllable through prompt engineering. Not every success criteria or failing eval is best solved by prompt engineering. For example, latency and cost can be sometimes more easily improved by selecting a different model.

### Prompting vs. Finetuning

Prompt engineering is far faster than other methods of model behavior control, such as finetuning, and can often yield leaps in performance in far less time. Here are some reasons to consider prompt engineering over finetuning:

- **Resource efficiency**: Fine-tuning requires high-end GPUs and large memory, while prompt engineering only needs text input, making it much more resource-friendly.

- **Cost-effectiveness**: For cloud-based AI services, fine-tuning incurs significant costs. Prompt engineering uses the base model, which is typically cheaper.

- **Maintaining model updates**: When providers update models, fine-tuned versions might need retraining. Prompts usually work across versions without changes.

- **Time-saving**: Fine-tuning can take hours or even days. In contrast, prompt engineering provides nearly instantaneous results, allowing for quick problem-solving.

- **Minimal data needs**: Fine-tuning needs substantial task-specific, labeled data, which can be scarce or expensive. Prompt engineering works with few-shot or even zero-shot learning.

- **Flexibility & rapid iteration**: Quickly try various approaches, tweak prompts, and see immediate results. This rapid experimentation is difficult with fine-tuning.

- **Domain adaptation**: Easily adapt models to new domains by providing domain-specific context in prompts, without retraining.

- **Comprehension improvements**: Prompt engineering is far more effective than finetuning at helping models better understand and utilize external content such as retrieved documents.

- **Preserves general knowledge**: Fine-tuning risks catastrophic forgetting, where the model loses general knowledge. Prompt engineering maintains the model's broad capabilities.

- **Transparency**: Prompts are human-readable, showing exactly what information the model receives. This transparency aids in understanding and debugging.

## How to Prompt Engineer

The prompt engineering pages in this section have been organized from most broadly effective techniques to more specialized techniques. When troubleshooting performance, try these techniques in order, although the actual impact of each technique will depend on your use case.

1. **Prompt generator** - Use the Console's prompt generator tool
2. **Be clear and direct** - State exactly what you want
3. **Use examples (multishot)** - Show don't tell
4. **Let Claude think (chain of thought)** - Allow reasoning steps
5. **Use XML tags** - Structure your prompts
6. **Give Claude a role (system prompts)** - Set context and persona
7. **Prefill Claude's response** - Guide the output format
8. **Chain complex prompts** - Break down multi-step tasks
9. **Long context tips** - Handle large documents effectively

## Prompt Engineering Tutorial

If you're an interactive learner, you can dive into interactive tutorials:

- **GitHub prompting tutorial**: An example-filled tutorial that covers the prompt engineering concepts found in our docs.
  - https://github.com/anthropics/prompt-eng-interactive-tutorial

- **Google Sheets prompting tutorial**: A lighter weight version of our prompt engineering tutorial via an interactive spreadsheet.
  - https://docs.google.com/spreadsheets/d/19jzLgRruG9kjUQNKtCg1ZjdD6l6weA6qRXG5zLIAhC8
