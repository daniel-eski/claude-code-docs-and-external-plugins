# Tools

Utility scripts for managing Claude Code skills.

## Scripts

| Script | Purpose |
|--------|---------|
| `fetch-skill.sh` | Download a skill from a GitHub repository |
| `validate-skill.sh` | Validate SKILL.md format for Claude Code compatibility |
| `deploy-skill.sh` | Deploy a single skill to Claude Code |
| `deploy-all.sh` | Deploy all core skills at once |
| `update-sources.sh` | Check for updates or re-fetch all skills |

## Usage Examples

### Fetch a skill from GitHub

```bash
# From a standalone repo
./fetch-skill.sh https://github.com/obra/brainstorming ../core-skills/obra-workflow/brainstorming

# From a subdirectory (e.g., anthropics/skills)
./fetch-skill.sh https://github.com/anthropics/skills/tree/main/skills/xlsx ../core-skills/document-creation/xlsx
```

### Validate a skill

```bash
./validate-skill.sh ../core-skills/obra-workflow/brainstorming
```

### Deploy skills

```bash
# Deploy a single skill to ~/.claude/skills/
./deploy-skill.sh ../core-skills/obra-workflow/brainstorming

# Deploy to a custom location
./deploy-skill.sh ../core-skills/obra-workflow/brainstorming /path/to/target

# Deploy all core skills
./deploy-all.sh

# Deploy all to project-specific location
./deploy-all.sh .claude/skills
```

### Check for updates

```bash
# List all skills with their source URLs and fetch dates
./update-sources.sh

# Re-fetch all skills from their sources
./update-sources.sh --fetch
```

## Source Tracking

Each fetched skill includes a `.source` file with:
- `source`: Original GitHub URL
- `fetched`: Timestamp of when the skill was downloaded
- `raw_base`: Raw content URL used for fetching

This enables tracking where skills came from and when they were last updated.
