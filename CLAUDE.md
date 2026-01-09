# CLAUDE.md

> Guidance for Claude Code and other LLM agents working with this repository.

---

## Quick Orientation

**What is this repository?**
A comprehensive offline resource combining:
1. **Official Claude Code documentation** (folders `01-10`) — mirrored from https://code.claude.com/docs
2. **Curated external skills library** (`11-external-resources/`) — 32 production-ready skills with deployment tools
3. **Plugins** (`11-external-resources/plugins/`) — 2 production plugins (claude-code-advisor, context-introspection)
4. **Best practices documents** (`12-best-practices/`) — 9 key docs from Anthropic engineering blog and platform
5. **External resources guide** (`13-external-resources-guide/`) — curated links organized by use case
6. **Repository maintenance documentation** (`_repo-maintenance/`) — analysis and improvement plans

**Repository created**: January 2026
**Documentation source date**: January 6, 2026

---

## Finding Documentation by Task

Use this decision tree to quickly find the right resources. Each folder has a README.md with file descriptions.

### "I need to set up or configure Claude Code"
→ First time: `01-getting-started/quickstart.md` (5-minute guide)
→ Full setup: `01-getting-started/setup.md` (installation, auth)
→ Keyboard shortcuts: `01-getting-started/interactive-mode.md`
→ Settings: `08-configuration/settings.md` (all config options)
→ Browse all: `docs-index.md` (complete 49-file index)

### "I need prompt engineering guidance"
→ Start with `12-best-practices/prompt-engineering-overview.md` (local)
→ For Claude 4.x specific: `12-best-practices/claude-4-prompting.md` (local)
→ For more: `13-external-resources-guide/prompt-engineering.md` (links)

### "I need to build agents or subagents"
→ Start with `12-best-practices/building-effective-agents.md` (local)
→ Subagents: `02-core-features/subagents.md` (local)
→ Advanced: `12-best-practices/multi-agent-research-system.md` (local)
→ For more: `13-external-resources-guide/agent-development.md` (links)

### "I need agentic coding best practices"
→ Read `12-best-practices/claude-code-best-practices.md` (local)
→ Context management: `12-best-practices/context-engineering.md` (local)

### "I need to use tools or MCP"
→ MCP setup: `02-core-features/mcp.md` (local)
→ Tool use overview: `12-best-practices/tool-use-overview.md` (local)
→ Tool design: `12-best-practices/writing-tools-for-agents.md` (local)
→ For more: `13-external-resources-guide/mcp-resources.md` (links)

### "I need to work with skills"
→ Skills guide: `02-core-features/skills.md` (local)
→ Skill library: `11-external-resources/CATALOG.md`

### "I need to work with hooks"
→ Hooks guide: `02-core-features/hooks-guide.md` (practical examples)
→ Hooks reference: `02-core-features/hooks.md` (schema, events)

### "I need CI/CD or automation"
→ GitHub Actions: `06-cicd-automation/github-actions.md`
→ GitLab CI/CD: `06-cicd-automation/gitlab-ci-cd.md`
→ Headless/programmatic: `06-cicd-automation/headless.md`

### "I need cloud provider setup"
→ AWS Bedrock: `04-cloud-providers/amazon-bedrock.md`
→ Google Vertex AI: `04-cloud-providers/google-vertex-ai.md`
→ Microsoft Foundry: `04-cloud-providers/microsoft-foundry.md`
→ Generic gateway: `04-cloud-providers/llm-gateway.md`

### "I need enterprise/security guidance"
→ Security: `05-enterprise/security.md`
→ IAM: `05-enterprise/iam.md`
→ Sandboxing: `05-enterprise/sandboxing.md`

### "I need API documentation (not CLI)"
→ See `13-external-resources-guide/api-reference.md` (links to platform.claude.com)

### "I want to learn through tutorials"
→ See `13-external-resources-guide/tutorials-courses.md` (links)

### "I need to find GitHub repositories"
→ See `13-external-resources-guide/github-repositories.md` (links)

### "I need to understand Claude Code features deeply"
→ Use the `claude-code-advisor` plugin: `11-external-resources/plugins/claude-code-advisor/`
→ Commands: `/cc-advisor`, `/cc-analyze`, `/cc-verify`, `/cc-design`

### "I need to see what context is loaded in my session"
→ Use the `context-introspection` plugin: `11-external-resources/plugins/context-introspection/`
→ Command: `/context-introspection:report`

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
├── 01-getting-started/        # 4 docs + README.md: overview, quickstart, setup, interactive-mode
├── 02-core-features/          # 7 docs + README.md: memory, skills, subagents, hooks, mcp
├── 03-ide-integration/        # 4 docs + README.md: vs-code, jetbrains, desktop, chrome
├── 04-cloud-providers/        # 4 docs + README.md: bedrock, vertex-ai, foundry, llm-gateway
├── 05-enterprise/             # 6 docs + README.md: iam, security, sandboxing, monitoring
├── 06-cicd-automation/        # 4 docs + README.md: github-actions, gitlab, headless
├── 07-plugins/                # 4 docs + README.md: plugins, reference, discovery, marketplaces
├── 08-configuration/          # 6 docs + README.md: settings, model-config, terminal, statusline
├── 09-reference/              # 6 docs + README.md: cli-reference, troubleshooting, costs
├── 10-other/                  # 4 docs + README.md: slack, changelog, legal, workflows
├── 11-external-resources/     # Skills library with tools and guides
├── 12-best-practices/         # 9 key docs: agent patterns, prompting, tools
├── 13-external-resources-guide/  # Curated links by use case
├── _repo-maintenance/         # Maintenance analysis and recommendations
├── .repo-metadata.json        # Centralized sync state and statistics
├── CHANGELOG.md               # Repository change history
├── CLAUDE.md                  # This file
├── CONTRIBUTING.md            # Contribution guidelines
├── docs-index.md              # Master index of all 49 docs with descriptions
├── README.md                  # Human-readable overview
├── IMPLEMENTATION_PLAN.md     # [HISTORICAL] Original implementation plan
└── llms.txt                   # Official docs index with URLs
```

**Note**: Each folder 01-10 has a README.md with one-line descriptions of all files in that folder.

---

## Key Files Reference

| File | Purpose | When to Use |
|------|---------|-------------|
| `docs-index.md` | Master index of all 49 docs with descriptions | Quick task-based navigation, scanning file contents |
| `llms.txt` | Official docs index with URLs | Comparing with upstream, finding doc URLs |
| `README.md` | Complete navigation with descriptions | Human users exploring the repo |
| `01-10/README.md` | Per-folder file indexes | Understanding files within a specific folder |
| `11-external-resources/CATALOG.md` | Skills inventory | Finding and deploying skills |
| `12-best-practices/README.md` | Best practices index | Finding locally-saved key documents |
| `13-external-resources-guide/README.md` | External links by use case | Finding documentation beyond local files |
| `_repo-maintenance/README.md` | Maintenance doc index | Before making structural changes |

---

## Tool Locations

Scripts for working with skills:

| Tool | Location | Purpose |
|------|----------|---------|
| `fetch-skill.sh` | `11-external-resources/tools/` | Download skill from GitHub (with commit SHA tracking) |
| `validate-skill.sh` | `11-external-resources/tools/` | Validate SKILL.md format |
| `deploy-skill.sh` | `11-external-resources/tools/` | Deploy single skill |
| `deploy-all.sh` | `11-external-resources/tools/` | Deploy all core skills |
| `update-sources.sh` | `11-external-resources/tools/` | Re-fetch all skills from sources |
| `freshness-report.sh` | `11-external-resources/tools/` | Check for upstream skill updates |
| `regenerate-catalog.sh` | `11-external-resources/tools/` | Auto-generate CATALOG.md |
| `generate-stats.sh` | `11-external-resources/tools/` | Generate repository statistics |
| `migrate-source-files.sh` | `11-external-resources/tools/` | One-time migration for commit SHA tracking |
| `fix-unknown-shas.sh` | `11-external-resources/tools/` | Fix skills with "unknown" commit SHAs |

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
- Each skill has a `.source` file with origin URL, fetch timestamp, and commit SHA
- `.source` files enable change detection via `freshness-report.sh`
- Run `11-external-resources/tools/freshness-report.sh` to check for upstream updates
- Run `11-external-resources/tools/update-sources.sh --fetch` to refresh all skills

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

### Check which skills have upstream changes
```bash
cd 11-external-resources/tools/
./freshness-report.sh
```

### Regenerate the catalog after changes
```bash
cd 11-external-resources/tools/
./regenerate-catalog.sh --output ../CATALOG.md
```

### Update repository statistics
```bash
cd 11-external-resources/tools/
./generate-stats.sh --update-metadata
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
| 2026-01-09 | Merged all feature branches: consolidated navigation, best practices, plugins, and tooling |
| 2026-01-09 | Added `claude-code-advisor` plugin (10 agents, 22 references, 4 commands) |
| 2026-01-09 | Added `context-introspection` plugin for session context reporting |
| 2026-01-09 | Added plugin ecosystem documentation in `11-external-resources/plugins/` |
| 2026-01-09 | Added per-folder README.md navigation files to all 01-10 documentation folders |
| 2026-01-09 | Added `docs-index.md` master index with all 49 files and task-based navigation |
| 2026-01-09 | Enhanced "Finding Documentation by Task" with file-level paths |
| 2026-01-09 | Added `12-best-practices/` with 9 key documents from Anthropic engineering blog and platform docs |
| 2026-01-09 | Added `13-external-resources-guide/` with curated links organized by use case |
| 2026-01-07 | Added fix-unknown-shas.sh for fixing rate-limited commit SHA entries |
| 2026-01-07 | Added maintenance infrastructure: freshness-report.sh, regenerate-catalog.sh, generate-stats.sh |
| 2026-01-07 | Enhanced fetch-skill.sh with commit SHA tracking for change detection |
| 2026-01-07 | Added CHANGELOG.md, CONTRIBUTING.md, .repo-metadata.json |
| 2026-01-07 | Added `_repo-maintenance/` folder with maintainability analysis |
| 2026-01-07 | Enhanced CLAUDE.md with comprehensive agent guidance |
| 2026-01-06 | Initial repository creation with 49 docs + 32 skills |
