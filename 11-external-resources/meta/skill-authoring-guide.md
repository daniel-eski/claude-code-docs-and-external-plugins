# Skill Authoring Guide

Step-by-step guide to creating effective Claude Code skills.

## Before You Start

### 1. Define the Problem

Ask yourself:
- What task am I automating?
- When should Claude use this skill?
- What does success look like?

### 2. Check Existing Skills

Before creating a new skill:
- Search this catalog for similar skills
- Check [awesome-claude-skills](https://github.com/VoltAgent/awesome-claude-skills)
- Consider extending an existing skill

### 3. Gather Examples

Collect 3-5 real examples of the task:
- What inputs are provided?
- What outputs are expected?
- What edge cases exist?

## Creating the Skill

### Step 1: Initialize from Template

```bash
cd 11-external-resources/core-skills/skill-authoring/
cp -r template/ ../../<category>/my-new-skill/
```

### Step 2: Write the Description

The description is **the most important part**. It's the only thing Claude sees when deciding whether to use your skill.

**Bad description:**
```yaml
description: Handles commits
```

**Good description:**
```yaml
description: >-
  Creates Git commits following Conventional Commits specification.
  Use when committing code changes. Supports feat, fix, docs, style,
  refactor, perf, test, and chore commit types.
```

Tips:
- Start with what it does
- Include when to use it
- Add keywords Claude might match on
- 200-500 characters is ideal

### Step 3: Write the Quick Start

Get users productive in under 30 seconds:

```markdown
# Quick Start

1. Stage your changes: `git add .`
2. Describe what you changed
3. The skill will format and create the commit
```

### Step 4: Write the Core Workflow

Document the main process in 3-7 numbered steps:

```markdown
# Core Workflow

1. **Analyze staged changes**
   Review what's being committed.

2. **Categorize the change**
   Determine the commit type (feat, fix, etc.).

3. **Write the message**
   Format: `<type>(<scope>): <description>`

4. **Create the commit**
   ```bash
   git commit -m "<formatted message>"
   ```
```

### Step 5: Define Important Rules

Use **ALWAYS** and **NEVER** for critical constraints:

```markdown
# Important Rules

- **ALWAYS** verify staged changes exist before committing
- **NEVER** include secrets, API keys, or credentials
- **ALWAYS** use present tense in commit messages
- **NEVER** exceed 72 characters in the subject line
```

### Step 6: Add Supporting Content

If needed, add:
- Common patterns with examples
- Troubleshooting section
- References for complex topics

### Step 7: Validate

```bash
cd ../../tools/
./validate-skill.sh ../core-skills/<category>/my-new-skill/
```

Fix any errors before deploying.

### Step 8: Test

Deploy to a test location:

```bash
./deploy-skill.sh ../core-skills/<category>/my-new-skill/ /tmp/test-skills/
```

Test with Claude Code:
1. Describe a task that should trigger the skill
2. Verify the skill activates
3. Check the output is correct

### Step 9: Deploy

```bash
./deploy-skill.sh ../core-skills/<category>/my-new-skill/
```

## Writing Tips

### Be Specific
- "Creates commits" → "Creates Git commits using Conventional Commits"
- "Handles files" → "Converts Markdown files to PDF format"

### Include Examples
Show don't tell:
```markdown
**Good commit:**
feat(auth): add OAuth2 login support

**Bad commit:**
fixed stuff
```

### Use Consistent Formatting
- Headings: `# H1`, `## H2`, `### H3`
- Code: Triple backticks with language
- Lists: Hyphens for unordered, numbers for ordered
- Emphasis: `**bold**` for important, `*italic*` for terms

### Think About Context
Skills are loaded into Claude's context. Ask:
- Is this information essential?
- Could this go in references/ instead?
- Am I being concise?

## Iteration

Skills improve over time:

1. **Deploy and use** - Get real experience
2. **Notice friction** - What's confusing or slow?
3. **Update the skill** - Improve descriptions, add examples
4. **Re-deploy** - Test improvements

## Getting Help

- Read existing skills in this repository
- Check `core-skills/skill-authoring/skill-creator/SKILL.md`
- See [reference/learning-resources.md](../reference/learning-resources.md)
