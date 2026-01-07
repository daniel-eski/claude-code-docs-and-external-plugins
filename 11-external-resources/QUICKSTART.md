# Quick Start Guide

Get skills deployed and working in under 5 minutes.

## Prerequisites

- Claude Code installed and configured
- Bash shell (macOS, Linux, or WSL)

## Deploy Your First Skill (30 seconds)

```bash
# Navigate to tools directory
cd 11-external-resources/tools/

# Deploy a single skill
./deploy-skill.sh ../core-skills/obra-workflow/brainstorming

# Restart Claude Code to activate
```

## Deploy All Core Skills (2 minutes)

```bash
cd 11-external-resources/tools/

# Deploy all skills to ~/.claude/skills/
./deploy-all.sh

# Or deploy to a project-specific location
./deploy-all.sh .claude/skills
```

## Verify Installation

### Check deployed skills

```bash
ls ~/.claude/skills/
```

### Validate a skill

```bash
./validate-skill.sh ~/.claude/skills/brainstorming
```

### Test in Claude Code

1. Start a new Claude Code session
2. Ask: "What skills are available?"
3. Verify your deployed skills appear in the list

## Deployment Targets

| Target | Path | Scope |
|--------|------|-------|
| Personal (default) | `~/.claude/skills/` | All projects |
| Project-specific | `.claude/skills/` | Current project only |

## Common Tasks

### Deploy a specific category

```bash
# Deploy only document creation skills
for skill in ../core-skills/document-creation/*/; do
    ./deploy-skill.sh "$skill"
done
```

### Update all skills from sources

```bash
./update-sources.sh --fetch
./deploy-all.sh
```

### Remove a deployed skill

```bash
rm -rf ~/.claude/skills/<skill-name>
```

## Troubleshooting

### Skill not appearing in Claude Code

1. Verify SKILL.md exists: `ls ~/.claude/skills/<skill-name>/SKILL.md`
2. Validate the skill: `./validate-skill.sh ~/.claude/skills/<skill-name>`
3. Restart Claude Code completely

### Deployment fails

1. Check the skill was fetched: `ls ../core-skills/<category>/<skill-name>/SKILL.md`
2. If missing or placeholder, re-fetch: `./fetch-skill.sh <github-url> <local-path>`

### Permission errors

```bash
chmod +x tools/*.sh
```

## Next Steps

- Browse all available skills: [CATALOG.md](CATALOG.md)
- Learn skill structure: [meta/skill-anatomy.md](meta/skill-anatomy.md)
- Create your own skills: [meta/skill-authoring-guide.md](meta/skill-authoring-guide.md)
