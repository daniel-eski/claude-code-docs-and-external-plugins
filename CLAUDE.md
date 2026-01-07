# CLAUDE.md

> Guidance for Claude Code and other LLM agents working with this repository.

---

## Quick Orientation

**What is this repository?**
A comprehensive offline resource combining:
1. **Official Claude Code documentation** (folders `01-10`) — mirrored from https://code.claude.com/docs
2. **Curated external skills library** (`11-external-resources/`) — 32 production-ready skills with deployment tools
3. **Repository maintenance documentation** (`_repo-maintenance/`) — analysis and improvement plans

**Repository created**: January 2026  
**Documentation source date**: January 6, 2026

---

## For AI Agents: Where to Start

### If You're Answering Questions About Claude Code

Consult the relevant documentation in folders `01-10`:

| Topic | Folder |
|-------|--------|
| Installation, setup | `01-getting-started/` |
| Skills, memory, hooks, MCP | `02-core-features/` |
| VS Code, JetBrains, Chrome | `03-ide-integration/` |
| AWS, GCP, Azure providers | `04-cloud-providers/` |
| Security, IAM, enterprise | `05-enterprise/` |
| GitHub Actions, CI/CD | `06-cicd-automation/` |
| Plugin development | `07-plugins/` |
| Settings, configuration | `08-configuration/` |
| CLI reference, troubleshooting | `09-reference/` |
| Slack, changelog, legal | `10-other/` |

The documentation is authoritative and matches the official docs as of the sync date.

### If You're Working with Skills

The skills library is in `11-external-resources/`:

```
11-external-resources/
├── CATALOG.md              # Complete skill index (start here)
├── QUICKSTART.md           # 5-minute deployment guide
├── core-skills/            # 28 production-ready skills
│   ├── obra-workflow/      # 6 workflow methodology skills
│   ├── obra-development/   # 9 development practice skills
│   ├── git-workflow/       # 6 Git/GitHub automation skills
│   ├── testing/            # 2 testing skills
│   ├── document-creation/  # 5 document generation skills
│   └── skill-authoring/    # 2 skill creation templates
├── extended-skills/        # 4 AWS skills + context engineering links
├── reference/              # External community links
├── tools/                  # Deployment scripts
└── meta/                   # Skill authoring documentation
```

**Quick deploy all skills:**
```bash
cd 11-external-resources/tools/
./deploy-all.sh
```

### If You're Modifying This Repository

Two documentation sets exist for AI agent continuity:

#### Repository-Wide Maintenance (`_repo-maintenance/`)

**Read first**: `_repo-maintenance/context-for-llm-agents.md`

For structural changes, maintainability improvements, and overall repository health:

| Document | Purpose |
|----------|---------|
| `context-for-llm-agents.md` | **Start here** — continuation context for AI agents |
| `01-current-state.md` | Complete repository inventory and analysis |
| `02-challenges.md` | Identified maintainability issues |
| `03-recommendations.md` | Prioritized improvement plan |
| `04-implementation-specs.md` | Technical specifications for improvements |
| `05-future-considerations.md` | Long-term architectural ideas |

#### Skills-Specific Documentation (`11-external-resources/_meta/`)

**Read first**: `11-external-resources/_meta/CONTINUATION-GUIDE.md`

For adding, updating, or fetching skills:

| Document | Purpose |
|----------|---------|
| `CONTINUATION-GUIDE.md` | How to continue skills work effectively |
| `SKILL-SOURCES-REGISTRY.md` | Verified source URLs and repo structures |
| `SESSION-RETROSPECTIVE.md` | What was done and why |
| `FUTURE-ENHANCEMENTS.md` | Planned skill infrastructure improvements |

Both documentation sets ensure continuity across AI sessions.

---

## Repository Structure

```
/
├── 01-getting-started/      # 4 docs: overview, quickstart, setup, interactive-mode
├── 02-core-features/        # 7 docs: memory, skills, subagents, hooks, mcp
├── 03-ide-integration/      # 4 docs: vs-code, jetbrains, desktop, chrome
├── 04-cloud-providers/      # 4 docs: bedrock, vertex-ai, foundry, llm-gateway
├── 05-enterprise/           # 6 docs: iam, security, sandboxing, monitoring
├── 06-cicd-automation/      # 4 docs: github-actions, gitlab, headless
├── 07-plugins/              # 4 docs: plugins, reference, discovery, marketplaces
├── 08-configuration/        # 6 docs: settings, model-config, terminal, statusline
├── 09-reference/            # 6 docs: cli-reference, troubleshooting, costs
├── 10-other/                # 4 docs: slack, changelog, legal, workflows
├── 11-external-resources/   # Skills library with tools and guides
├── _repo-maintenance/       # Maintenance analysis and recommendations
├── CLAUDE.md                # This file
├── README.md                # Human-readable overview
├── IMPLEMENTATION_PLAN.md   # [HISTORICAL] Original implementation plan
└── llms.txt                 # Official docs index with URLs
```

---

## Key Files Reference

| File | Purpose | When to Use |
|------|---------|-------------|
| `llms.txt` | Official docs index with URLs | Comparing with upstream, finding doc URLs |
| `README.md` | Complete navigation with descriptions | Human users exploring the repo |
| `11-external-resources/CATALOG.md` | Skills inventory | Finding and deploying skills |
| `_repo-maintenance/README.md` | Maintenance doc index | Before making structural changes |

---

## Tool Locations

Scripts for working with skills:

| Tool | Location | Purpose |
|------|----------|---------|
| `fetch-skill.sh` | `11-external-resources/tools/` | Download skill from GitHub |
| `validate-skill.sh` | `11-external-resources/tools/` | Validate SKILL.md format |
| `deploy-skill.sh` | `11-external-resources/tools/` | Deploy single skill |
| `deploy-all.sh` | `11-external-resources/tools/` | Deploy all core skills |
| `update-sources.sh` | `11-external-resources/tools/` | Re-fetch all skills from sources |

---

## Skill Authoring Resources

For creating or understanding skills:

| Resource | Location | Purpose |
|----------|----------|---------|
| Official templates | `11-external-resources/core-skills/skill-authoring/` | SKILL.md templates |
| Skill anatomy | `11-external-resources/meta/skill-anatomy.md` | Deep dive into SKILL.md structure |
| Authoring guide | `11-external-resources/meta/skill-authoring-guide.md` | Step-by-step creation guide |
| Patterns | `11-external-resources/meta/patterns-and-antipatterns.md` | Best practices and pitfalls |
| Official docs | `02-core-features/skills.md` | Claude Code's skill documentation |

---

## Source Tracking

### Core Documentation (folders 01-10)
- Source: https://code.claude.com/docs
- Sync method: Manual
- Last sync: January 6, 2026
- Index file: `llms.txt`

### External Skills
- Each skill has a `.source` file with origin URL and fetch timestamp
- Run `11-external-resources/tools/update-sources.sh` to see all sources
- Run `11-external-resources/tools/update-sources.sh --fetch` to refresh

---

## Common Tasks

### Find documentation about a feature
1. Check the folder names (01-10) for the relevant category
2. Open the specific markdown file
3. Or search with: `grep -r "feature name" 0*-*/`

### Deploy a skill
```bash
cd 11-external-resources/tools/
./deploy-skill.sh ../core-skills/obra-workflow/brainstorming
```

### Validate a skill before deployment
```bash
cd 11-external-resources/tools/
./validate-skill.sh ../core-skills/obra-workflow/brainstorming
```

### Add a new skill from GitHub
```bash
cd 11-external-resources/tools/
./fetch-skill.sh https://github.com/author/repo/tree/main/skills/skill-name ../core-skills/category/skill-name
./validate-skill.sh ../core-skills/category/skill-name
```

### Update all skills from upstream
```bash
cd 11-external-resources/tools/
./update-sources.sh --fetch
```

---

## Important Notes

1. **Documentation currency**: The official docs (folders 01-10) are a point-in-time snapshot. Check https://code.claude.com for the latest.

2. **Placeholder skills**: Two skills are placeholders (repos weren't accessible at fetch time):
   - `core-skills/git-workflow/changelog-generator/`
   - `core-skills/testing/pypict-skill/`

3. **Maintenance documentation**: The `_repo-maintenance/` folder contains analysis for improving this repository. Read it before making structural changes.

4. **No executable code in main docs**: Folders 01-10 are pure documentation. Only `11-external-resources/tools/` contains scripts.

---

## Revision History

| Date | Change |
|------|--------|
| 2026-01-07 | Added `_repo-maintenance/` folder with maintainability analysis |
| 2026-01-07 | Enhanced CLAUDE.md with comprehensive agent guidance |
| 2026-01-06 | Initial repository creation with 49 docs + 32 skills |
