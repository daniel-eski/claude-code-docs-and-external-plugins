# Skill Authoring Documentation

Guides for creating and maintaining Claude Code skills.

## Documents

| File | Description |
|------|-------------|
| [skill-anatomy.md](skill-anatomy.md) | Deep dive into SKILL.md structure |
| [skill-authoring-guide.md](skill-authoring-guide.md) | How to write effective skills |
| [patterns-and-antipatterns.md](patterns-and-antipatterns.md) | Common patterns to follow/avoid |

## Quick Reference

### Skill File Structure

```
my-skill/
├── SKILL.md           # Required: frontmatter + instructions
├── scripts/           # Optional: executable code
├── references/        # Optional: additional documentation
└── assets/           # Optional: templates, images, data
```

### SKILL.md Frontmatter

```yaml
---
name: skill-name          # Required: 1-64 chars, lowercase, hyphens
description: >-           # Required: 1-1024 chars - THE TRIGGER
  What this skill does and when to use it.
license: MIT              # Optional
compatibility: python>=3.8 # Optional
---
```

### Key Principles

1. **Description is the trigger** - Claude uses only the description to decide when to invoke
2. **Keep it concise** - Under 500 lines in SKILL.md
3. **Progressive disclosure** - Details go in `references/`
4. **Self-contained** - Include all necessary resources
