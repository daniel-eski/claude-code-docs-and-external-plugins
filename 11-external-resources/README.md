# External Skills & Resources

A curated collection of community skills, plugins, and learning resources for Claude Code skill development.

## Quick Navigation

| Section | Description | Use When... |
|---------|-------------|-------------|
| [core-skills/](core-skills/) | Production-ready skills | You need working skills now |
| [extended-skills/](extended-skills/) | Specialized skills | You have specific domain needs |
| [reference/](reference/) | Links to external resources | You want to explore more |
| [tools/](tools/) | Deployment scripts | You're setting up skills |
| [meta/](meta/) | Skill authoring guides | You're creating skills |

## Getting Started

### 1. Browse Available Skills

See [CATALOG.md](CATALOG.md) for the complete indexed catalog of all skills.

### 2. Quick Deploy a Single Skill

```bash
cd tools/
./deploy-skill.sh ../core-skills/obra-workflow/brainstorming
```

### 3. Deploy All Core Skills

```bash
cd tools/
./deploy-all.sh
```

### 4. Verify Installation

Skills are deployed to `~/.claude/skills/` by default. Restart Claude Code to activate.

See [QUICKSTART.md](QUICKSTART.md) for detailed deployment instructions.

## Directory Structure

```
11-external-resources/
├── core-skills/           # Downloaded skills ready for deployment
│   ├── obra-workflow/     # Jesse Vincent's workflow methodology (7 skills)
│   ├── obra-development/  # Development-focused skills (14 skills)
│   ├── git-workflow/      # Git/GitHub automation
│   ├── testing/           # Testing skills (webapp-testing, pypict)
│   ├── document-creation/ # Document generation (docx, pdf, xlsx, pptx)
│   └── skill-authoring/   # Meta skills for creating skills
│
├── extended-skills/       # Specialized domain skills
│   ├── aws-skills/        # AWS infrastructure automation
│   └── context-engineering/ # Educational content on context engineering
│
├── reference/             # Curated external links
├── tools/                 # Deployment & validation scripts
└── meta/                  # Skill authoring documentation
```

## For AI Agents

This directory is optimized for AI agent consumption:

- All `SKILL.md` files follow the standard Claude Code skill format
- Each skill includes machine-readable YAML frontmatter with `name` and `description`
- The [CATALOG.md](CATALOG.md) provides structured navigation with source URLs
- Skills are organized by functional category for easy discovery

## Skill Sources

| Source | Description |
|--------|-------------|
| [obra/*](https://github.com/obra) | Jesse Vincent's workflow and development skills |
| [anthropics/skills](https://github.com/anthropics/skills) | Official Anthropic skills |
| [fvadicamo/dev-agent-skills](https://github.com/fvadicamo/dev-agent-skills) | Git/GitHub workflow bundle |
| [muratcankoylan/Agent-Skills-for-Context-Engineering](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering) | Context engineering education |

## Contributing

To add a new skill:

1. Fetch it: `./tools/fetch-skill.sh <github-url> <local-path>`
2. Validate it: `./tools/validate-skill.sh <local-path>`
3. Update [CATALOG.md](CATALOG.md) with the new entry
4. Update the appropriate category README
