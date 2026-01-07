# Skill Anatomy

Deep dive into the structure and components of a Claude Code skill.

## Directory Structure

```
skill-name/
├── SKILL.md              # Required: Main skill definition
├── scripts/              # Optional: Executable code
│   ├── init.py
│   ├── process.sh
│   └── helpers.js
├── references/           # Optional: Extended documentation
│   ├── REFERENCE.md
│   ├── API_GUIDE.md
│   └── TEMPLATES.md
└── assets/              # Optional: Static resources
    ├── template.docx
    ├── logo.png
    └── sample-data.csv
```

## SKILL.md Structure

### 1. YAML Frontmatter (Required)

```yaml
---
name: my-skill-name       # 1-64 chars, lowercase, hyphens only
description: >-           # 1-1024 chars
  Clear description of what this skill does and when to use it.
  Include keywords that help Claude identify relevant tasks.
license: MIT              # Optional
compatibility: python>=3.8 # Optional
metadata:                 # Optional: custom key-value pairs
  category: "development"
  version: "1.0"
---
```

#### Name Rules
- 1-64 characters
- Lowercase letters, numbers, hyphens only
- Must be unique within deployment scope
- Should be descriptive: `git-commit` not `gc`

#### Description Rules
- 1-1024 characters
- **This is the trigger mechanism** - Claude uses this to decide when to invoke
- Include: what it does, when to use it, keywords
- Example: "Creates Git commits following Conventional Commits. Use when committing code changes with structured messages."

### 2. Markdown Body

#### Quick Start Section
First section users/agents see. Keep under 30 lines.

```markdown
# Quick Start

1. Ensure Git is configured
2. Stage your changes: `git add .`
3. Run the skill with your message
```

#### Core Workflow Section
Main steps with code examples. Numbered steps, 3-7 items.

```markdown
# Core Workflow

1. **Analyze changes**
   ```bash
   git diff --staged
   ```

2. **Choose commit type**
   - `feat`: New feature
   - `fix`: Bug fix
   ...
```

#### Important Rules Section
Critical constraints using **ALWAYS** and **NEVER**.

```markdown
# Important Rules

- **ALWAYS** check for staged changes before committing
- **NEVER** commit secrets or credentials
- **ALWAYS** run tests before pushing
```

#### Optional Sections

- Overview - Why this skill exists
- When to Use - Bulleted use cases
- Common Patterns - Code examples
- Common Mistakes - Problem/fix pairs
- Advanced Topics - Extended features

### 3. References Directory

For detailed documentation that would exceed the 500-line limit.

```markdown
# In SKILL.md
For API details, see [references/API.md](references/API.md)
```

Reference files are loaded **on-demand**, keeping initial context lean.

### 4. Scripts Directory

Executable code that the skill uses.

```python
# scripts/validate.py
#!/usr/bin/env python3
import sys
# Validation logic
```

Scripts should be:
- Self-contained
- Well-documented
- Error-handling included
- Tested before inclusion

### 5. Assets Directory

Static resources: templates, images, sample data.

## Progressive Disclosure

Skills use a three-level context loading system:

| Level | Content | Tokens | When Loaded |
|-------|---------|--------|-------------|
| 1: Metadata | name + description | ~100 | Always (startup) |
| 2: Instructions | SKILL.md body | ~500-5000 | When triggered |
| 3: Resources | scripts, references, assets | Unbounded | On-demand |

This keeps Claude's context efficient while enabling complex skills.

## File Size Guidelines

| Component | Guideline |
|-----------|-----------|
| SKILL.md | < 500 lines |
| description | 200-500 characters recommended |
| Quick Start | < 30 lines |
| Individual scripts | < 200 lines each |
| references/*.md | As needed |

## Example: Minimal Skill

```yaml
---
name: hello-world
description: A minimal example skill that greets users.
---

# Quick Start

Say "hello" to trigger this skill.

# Core Workflow

1. Receive greeting request
2. Respond with "Hello, World!"

# Important Rules

- **ALWAYS** be friendly
```

## Example: Complex Skill

See `core-skills/obra-development/test-driven-development/SKILL.md` for a comprehensive example (371 lines).
