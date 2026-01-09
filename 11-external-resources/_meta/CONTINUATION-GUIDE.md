# Continuation Guide for Future Agents

**Purpose**: Help future LLM agents (or humans) quickly understand this project and continue the work effectively.

---

## Quick Context

You're looking at an external skills directory for a Claude Code documentation repository. The main repo contains official Claude Code docs (directories 01-10). This directory (11-external-resources) contains **community skills** fetched from GitHub repositories.

**Current state**: 32 skills downloaded, validated, and documented. The infrastructure is working.

---

## Before You Start

### Read These First (in order)

1. **This file** - You're here
2. `SESSION-RETROSPECTIVE.md` - What was done and why
3. `SKILL-SOURCES-REGISTRY.md` - Verified source locations
4. `FUTURE-ENHANCEMENTS.md` - Planned improvements with implementations

### Key Files to Know

| File | Purpose |
|------|---------|
| `CATALOG.md` | Complete skill listing with status |
| `tools/fetch-skill.sh` | How skills are downloaded |
| `tools/validate-skill.sh` | How skills are validated |
| `tools/deploy-all.sh` | How skills are deployed |

---

## Critical Knowledge

### Skill Repo Structures Are NOT What They Seem

The [awesome-claude-skills](https://github.com/VoltAgent/awesome-claude-skills) list shows URLs like:
- `obra/brainstorming`
- `obra/writing-plans`
- `obra/test-driven-development`

**These are NOT individual repos.** They're paths within consolidated repos:
- All obra skills → `obra/superpowers/skills/<name>/`
- Experimental obra → `obra/superpowers-lab/skills/<name>/`

Always check `SKILL-SOURCES-REGISTRY.md` for the actual structure.

### Some Listed Skills Don't Exist

These were in awesome-claude-skills but returned 404:
- ComposioHQ/changelog-generator
- omkamal/pypict-skill
- obra/root-cause-tracing
- obra/condition-based-waiting
- obra/commands
- (and others - see registry)

If you encounter a 404, it's likely not your fault. Create a placeholder and document.

### Skill File Format

Every skill has:
```
skill-name/
├── SKILL.md      # Required: YAML frontmatter + markdown
└── .source       # Tracking: where it came from, when fetched
```

SKILL.md format:
```yaml
---
name: skill-name
description: What it does and when to use it
---

# Quick Start
...

# Core Workflow
...

# Important Rules
...
```

---

## Common Tasks

### Adding a New Skill

```bash
cd 11-external-resources/tools/

# 1. Verify URL exists first
curl -sL -o /dev/null -w "%{http_code}" \
  "https://raw.githubusercontent.com/owner/repo/main/skills/skill-name/SKILL.md"

# 2. Fetch if 200
./fetch-skill.sh "https://github.com/owner/repo/tree/main/skills/skill-name" \
  "../core-skills/category/skill-name"

# 3. Validate
./validate-skill.sh "../core-skills/category/skill-name"

# 4. Update CATALOG.md
# Add entry to appropriate section

# 5. Update category README
# Add to skills table
```

### Adding a New Skill Source (Repo)

1. Explore the repo structure first:
```bash
curl -s "https://api.github.com/repos/owner/repo/contents/skills" | jq -r '.[].name'
```

2. Add to `SKILL-SOURCES-REGISTRY.md`

3. Fetch all skills from the repo

4. Create category directory if needed

5. Update CATALOG.md

### Re-fetching All Skills (Refresh)

```bash
cd 11-external-resources/tools/
./update-sources.sh --fetch
```

### Deploying Skills to Claude Code

```bash
# All skills
./deploy-all.sh

# Single skill
./deploy-skill.sh ../core-skills/obra-workflow/brainstorming

# To project-specific location
./deploy-all.sh .claude/skills
```

---

## What's NOT Done Yet

### High Value / Should Do

1. **Context engineering skills** - Only linked, not downloaded
   - Location: `extended-skills/context-engineering/README.md` (links only)
   - Source: muratcankoylan/Agent-Skills-for-Context-Engineering
   - Consider downloading all 11 skills locally

2. **Supporting files** - Only SKILL.md fetched
   - Many skills have scripts/, references/, assets/
   - Could enhance fetch-skill.sh to grab these too

### Medium Value / Nice to Have

3. **Batch fetch command** - See FUTURE-ENHANCEMENTS.md
4. **README templates** - Reduce manual work

### Now Implemented ✅

- ~~Smart update checker~~ → `freshness-report.sh` compares local vs remote commit SHAs
- ~~Auto-regenerate catalog~~ → `regenerate-catalog.sh` generates CATALOG.md
- ~~URL verification~~ → fetch-skill.sh handles 404s gracefully

### Lower Value / Someday

5. **Parallel fetching**
6. **Invocation testing**

---

## Decision Points You May Face

### "Should I download more skills from awesome-claude-skills?"

**Consider**:
- Verify URLs exist first (many are stale)
- Check if the skill is useful for the user's goals
- Group by category for organization

### "Should I download the full skill directory, not just SKILL.md?"

**Trade-offs**:
- Pro: Complete skill with all resources
- Con: More complexity, some scripts may have dependencies
- Recommendation: Start with SKILL.md, add scripts if they're actually needed

### "A skill validation failed. What do I do?"

**Options**:
1. Check if it's a frontmatter issue (fixable)
2. Check if SKILL.md is actually a different format
3. Create a placeholder if it's a source issue
4. Document the issue in .source file

### "Should I modify existing skills?"

**Generally no**. These are third-party skills. If improvements are needed:
1. Document the issue
2. Consider contributing upstream
3. Or create a wrapper/enhancement skill locally

---

## Communication with User

### What to Ask

- "The awesome-claude-skills list shows X skills from [repo]. Do you want all of them or a subset?"
- "This skill requires [dependency]. Should I include setup instructions?"
- "Some skills couldn't be fetched (404). Should I create placeholders or investigate?"

### What to Report

- How many skills were fetched successfully
- Any 404s or errors encountered
- Validation results (pass/warn/fail)
- Any unexpected repo structures discovered

---

## Gotchas and Warnings

1. **GitHub rate limiting** - If doing bulk operations, add delays or use authentication

2. **Raw URL construction** - The pattern is:
   ```
   https://raw.githubusercontent.com/{owner}/{repo}/main/{path}/SKILL.md
   ```
   NOT:
   ```
   https://raw.githubusercontent.com/{owner}/{repo}/tree/main/{path}/SKILL.md
   ```

3. **API URL is different** - For listing directories:
   ```
   https://api.github.com/repos/{owner}/{repo}/contents/{path}
   ```

4. **Template location** - In anthropics/skills, the template is at `/template/` not `/skills/template/`

5. **Line count warnings** - Skills over 500 lines generate warnings but still work

---

## Quick Reference

### Skill Counts (as of 2026-01-06)

| Category | Count | Status |
|----------|-------|--------|
| obra-workflow | 6 | All OK |
| obra-development | 9 | All OK |
| git-workflow | 6 | 5 OK, 1 placeholder |
| testing | 2 | 1 OK, 1 placeholder |
| document-creation | 5 | All OK |
| skill-authoring | 2 | All OK |
| aws-skills | 4 | All OK |
| **Total** | **34** | **32 OK, 2 placeholders** |

### Tool Scripts

| Script | Usage |
|--------|-------|
| fetch-skill.sh | `./fetch-skill.sh <github-url> <local-path>` |
| validate-skill.sh | `./validate-skill.sh <skill-dir>` |
| deploy-skill.sh | `./deploy-skill.sh <skill-dir> [target]` |
| deploy-all.sh | `./deploy-all.sh [target]` |
| update-sources.sh | `./update-sources.sh [--fetch]` |
| freshness-report.sh | `./freshness-report.sh [--verbose]` |
| regenerate-catalog.sh | `./regenerate-catalog.sh --output ../CATALOG.md` |
| generate-stats.sh | `./generate-stats.sh [--json\|--markdown]` |

---

## If You Need Help

1. Read the SESSION-RETROSPECTIVE.md for full context
2. Check SKILL-SOURCES-REGISTRY.md for verified URLs
3. Look at existing skills as examples
4. Ask the user for clarification on priorities

---

## Final Notes

This project was built iteratively with user feedback. The infrastructure works. The main opportunities are:
1. Adding more skills (carefully, with verification)
2. Implementing the enhancements in FUTURE-ENHANCEMENTS.md
3. Keeping the registry and documentation up to date

Good luck, future agent!
