# Context for LLM Agents

> **Critical document** for AI assistants (Claude Code, etc.) continuing work on this repository.

Read this document first when picking up work on repository maintenance.

---

## Quick Orientation

**What is this repository?**
- Offline documentation mirror of Claude Code (https://code.claude.com/docs)
- Plus a curated library of external skills from the community
- With deployment tools and meta-documentation

**What problem are we solving?**
- Making the repository maintainable as upstream content changes
- Detecting when skills or docs become outdated
- Reducing manual maintenance burden

**Where is the analysis?**
- `01-current-state.md` - Complete repository analysis
- `02-challenges.md` - Identified problems
- `03-recommendations.md` - Prioritized solutions
- `04-implementation-specs.md` - Technical specifications
- `05-future-considerations.md` - Long-term ideas

---

## Current Status (as of 2026-01-07)

| Phase | Status |
|-------|--------|
| Analysis | ✅ Complete |
| Recommendations | ✅ Documented |
| Implementation Specs | ✅ Documented |
| Implementation | ⏳ Not started |
| Validation | ⏳ Pending |

**Last action taken**: Created this maintenance documentation folder.

**Next recommended action**: Get human review of recommendations, then implement P1 items.

---

## Key Files to Read

Before making changes, read these files:

1. **`/CLAUDE.md`** - Comprehensive AI agent guidance (enhanced Jan 7, 2026)
2. **`/README.md`** - Repository overview with full navigation
3. **`/11-external-resources/README.md`** - Skills section overview
4. **`/11-external-resources/tools/README.md`** - Existing tooling
5. **`/11-external-resources/meta/`** - Skill authoring documentation
6. **This folder's documents** - Analysis and recommendations

**Historical reference:**
- **`/IMPLEMENTATION_PLAN.md`** - Original plan for building `11-external-resources/`. Preserved with annotations showing what was planned vs. implemented.

**Skills-specific continuity (if working with skills):**
- **`/11-external-resources/_meta/CONTINUATION-GUIDE.md`** - Detailed guidance for adding/updating skills
- **`/11-external-resources/_meta/SKILL-SOURCES-REGISTRY.md`** - Verified source URLs and repo structures

---

## Understanding the Repository Structure

```
/
├── 01-10 folders         # Core documentation (from code.claude.com)
├── 11-external-resources # Skills library
│   ├── core-skills/      # 28 working skills
│   ├── extended-skills/  # 4 AWS skills
│   ├── reference/        # External links
│   ├── tools/            # Deployment scripts
│   └── meta/             # Skill authoring guides
├── _repo-maintenance/    # THIS FOLDER (meta-analysis)
├── CLAUDE.md             # AI guidance
├── README.md             # Repository overview
└── llms.txt              # Official docs index
```

---

## Key Technical Details

### Source Tracking for Skills

Each skill has a `.source` file:
```yaml
source: https://github.com/obra/superpowers/tree/main/skills/brainstorming
fetched: 2026-01-07T00:17:35Z
```

**Important gap**: No commit SHA tracking (cannot detect upstream changes).

### Existing Tools

| Tool | Location | Purpose |
|------|----------|---------|
| fetch-skill.sh | `11-external-resources/tools/` | Download skills from GitHub |
| validate-skill.sh | `11-external-resources/tools/` | Validate SKILL.md format |
| deploy-skill.sh | `11-external-resources/tools/` | Deploy to Claude Code |
| deploy-all.sh | `11-external-resources/tools/` | Bulk deployment |
| update-sources.sh | `11-external-resources/tools/` | Re-fetch all skills |

### Placeholder Skills

Two skills are placeholders (repos weren't accessible):
- `core-skills/git-workflow/changelog-generator/`
- `core-skills/testing/pypict-skill/`

---

## Implementation Priorities

If implementing recommendations, follow this order:

### Phase 1 (P1 - Do First)
1. **R4**: Create `CHANGELOG.md` (15 min)
2. **R2**: Create `.repo-metadata.json` (15 min)
3. **R1**: Enhance `.source` files with commit SHA (30 min)
4. **R3**: Create `generate-stats.sh` (45 min)

### Phase 2 (P2 - Do Soon)
5. **R8**: Create `CONTRIBUTING.md` (30 min)
6. **R5**: Create `freshness-report.sh` (2 hours)
7. **R6**: Create `regenerate-catalog.sh` (2 hours)

### Phase 3 (Later)
8. **R7**: Create `check-links.sh`
9. **R9**: Create GitHub Actions workflow

See `04-implementation-specs.md` for detailed specifications.

---

## How to Continue This Work

### If Implementing Recommendations

1. Confirm with human which recommendations to implement
2. Read the specification in `04-implementation-specs.md`
3. Implement one recommendation at a time
4. Test the implementation
5. Update `03-recommendations.md` to mark as complete
6. Update `CHANGELOG.md` with changes

### If Updating the Analysis

1. Re-read key repository files to check for changes
2. Update `01-current-state.md` with new findings
3. Update `02-challenges.md` if new issues found
4. Update recommendations if priorities changed

### If Adding New Skills

1. Use existing `fetch-skill.sh` tool
2. Follow process in `CONTRIBUTING.md` (once created)
3. Update `CATALOG.md`
4. Add entry to `CHANGELOG.md`

### If Syncing Documentation

1. Compare local `llms.txt` with https://code.claude.com/docs/llms.txt
2. Identify new/changed/removed pages
3. Download updated content
4. Update `.repo-metadata.json` sync date
5. Update `CHANGELOG.md`

---

## Validation Checklist

Before considering work complete, verify:

- [ ] All modified skills pass `validate-skill.sh`
- [ ] `CATALOG.md` reflects actual skill inventory
- [ ] Stats in `README.md` or `.repo-metadata.json` are accurate
- [ ] `CHANGELOG.md` documents what changed
- [ ] No broken internal links
- [ ] `CLAUDE.md` updated if new tools/processes added

---

## Common Pitfalls

### 1. Hardcoded Stats
Stats in `README.md` and `CATALOG.md` are manually maintained. If you add/remove skills, update these files or create the auto-generation scripts.

### 2. GitHub Rate Limits
The fetch and freshness scripts use GitHub API. Without authentication, rate limits apply (~60 requests/hour). Add delays between requests or use a GitHub token.

### 3. Relative Paths
Scripts use relative paths extensively. Always `cd` to the correct directory before running.

### 4. Placeholder Detection
Placeholder skills contain the text "This skill could not be fetched". Use this pattern for detection.

---

## Questions to Ask the Human

If context is unclear, ask:

1. "Should I implement the recommendations now, or just review/update the analysis?"
2. "Which priority level (P1/P2/P3) should I focus on?"
3. "Are there any new upstream changes I should be aware of?"
4. "Has the repository structure changed since the last analysis?"

---

## Repository Conventions

### File Naming
- Lowercase with hyphens: `my-skill-name/`
- SKILL.md is always uppercase
- .source is hidden file

### Markdown Style
- ATX headers (`#`, `##`, `###`)
- Fenced code blocks with language tags
- Tables for structured data
- YAML frontmatter for skill metadata

### Script Style
- Bash scripts with `#!/bin/bash`
- Use `set -e` for error handling
- Echo progress messages
- Support both interactive and non-interactive usage

---

## Revision History

| Date | Agent | Action |
|------|-------|--------|
| 2026-01-07 | Claude Code | Initial analysis and documentation |
| 2026-01-07 | Claude Code | Enhanced root files (CLAUDE.md, README.md, IMPLEMENTATION_PLAN.md) |

---

## Final Notes

This documentation was created to ensure continuity across AI sessions. The analysis represents a point-in-time understanding. Upstream repositories (Anthropic docs, skill sources) may have changed since this analysis.

**Always verify current state before making significant changes.**

When in doubt, re-read the source files listed in "Key Files to Read" above.

