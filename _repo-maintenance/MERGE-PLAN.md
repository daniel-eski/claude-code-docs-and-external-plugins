# Comprehensive Merge Plan

**Created**: 2026-01-09
**Status**: Ready for execution
**Prepared by**: Claude Code Agent

---

## Executive Summary

This plan consolidates work from **4 feature branches** containing **~12,400 lines** across **103 files** created by multiple Claude Code agents working in parallel. The branches have a divergent topology requiring careful sequencing.

### Branch Overview

| Branch | Unique Content | Lines | Files | Status |
|--------|---------------|-------|-------|--------|
| `feature/navigation-and-best-practices` | Best practices docs, navigation layer | ~2,600 | 41 | Ready |
| `feature/maintenance-infrastructure` | claude-code-advisor plugin | ~7,300 | 50 | Ready |
| `feature/plugin-ecosystem-documentation` | Plugin ecosystem guides | ~1,600 | 7 | Included in above |
| `feature/context-introspection-plugin` | Context introspection plugin | ~940 | 5 | Needs cleanup |

### Key Challenge

```
                    ┌─► 44175bd (claude-code-advisor) ← NOT IN OTHER BRANCHES
                    │
main ─► ef215d7 ─► 7779e47 ─┤
                    │
                    └─► 59ad36e ─► 23bfd0b ─► 5eee2cb ─► 649e72b
                        (plugin-   (nav &     (context-introspection)
                         ecosystem)  best-
                                    practices)
```

The `feature/maintenance-infrastructure` branch diverged at `7779e47` and has content (`claude-code-advisor`) not present in the other branches.

---

## Pre-Merge Checklist

### 1. Backup Current State
```bash
# Create a safety branch before any merges
git checkout main
git pull origin main
git checkout -b backup/pre-merge-2026-01-09
git push origin backup/pre-merge-2026-01-09
```

### 2. Verify Branch State
```bash
# Ensure all branches are up to date
git fetch --all

# Verify expected commits
git log --oneline origin/feature/navigation-and-best-practices ^origin/main | wc -l
# Expected: 6 commits

git log --oneline origin/feature/maintenance-infrastructure ^origin/main | wc -l
# Expected: 3 commits

git log --oneline origin/feature/context-introspection-plugin ^origin/feature/navigation-and-best-practices | wc -l
# Expected: 2 commits
```

### 3. Review Pending Changes
```bash
# No uncommitted changes should exist
git status
```

---

## Phase 1: Merge Navigation and Best Practices

**Priority**: HIGH
**Risk**: LOW
**Estimated conflicts**: Minor (CLAUDE.md, README.md)

This branch includes `feature/plugin-ecosystem-documentation` as an ancestor, so merging it brings in both.

### What This Adds
- `12-best-practices/` — 9 curated documents from Anthropic sources
- `13-external-resources-guide/` — 7 link collections by topic
- `01-10/README.md` — Per-folder navigation files (11 files)
- `docs-index.md` — Master index of all 49 documentation files
- `11-external-resources/plugins/` — Plugin ecosystem documentation (7 files)
- `.gitignore` — Standard ignores
- Enhanced `CLAUDE.md` with "Finding Documentation by Task" section
- Enhanced `README.md` with new folder descriptions

### Execution Steps

```bash
# Step 1.1: Checkout main and ensure clean
git checkout main
git pull origin main

# Step 1.2: Create merge branch
git checkout -b merge/phase-1-navigation

# Step 1.3: Merge navigation-and-best-practices
git merge origin/feature/navigation-and-best-practices --no-ff -m "Merge feature/navigation-and-best-practices

Adds:
- 12-best-practices/ - 9 curated documents from Anthropic
- 13-external-resources-guide/ - External links by topic
- Per-folder README.md navigation files for 01-10/
- docs-index.md master index
- 11-external-resources/plugins/ ecosystem documentation
- Enhanced CLAUDE.md with task-based navigation
- Enhanced README.md with folder descriptions"
```

### Expected Conflicts

| File | Conflict Type | Resolution Strategy |
|------|--------------|---------------------|
| `CLAUDE.md` | Both modified | Keep navigation branch version (more comprehensive) |
| `README.md` | Both modified | Keep navigation branch version |
| `llms.txt` | Minor additions | Accept incoming changes |

### Conflict Resolution

```bash
# If CLAUDE.md conflicts:
git checkout --theirs CLAUDE.md
git add CLAUDE.md

# If README.md conflicts:
git checkout --theirs README.md
git add README.md

# Complete merge
git commit
```

### Validation

```bash
# Verify new directories exist
ls -la 12-best-practices/
ls -la 13-external-resources-guide/
ls -la 01-getting-started/README.md

# Verify CLAUDE.md has navigation section
grep -q "Finding Documentation by Task" CLAUDE.md && echo "OK" || echo "MISSING"

# Count files
find 12-best-practices -name "*.md" | wc -l  # Expected: 10
find 13-external-resources-guide -name "*.md" | wc -l  # Expected: 7
```

### Push Phase 1

```bash
git checkout main
git merge merge/phase-1-navigation --ff-only
git push origin main
git branch -d merge/phase-1-navigation
```

---

## Phase 2: Merge Maintenance Infrastructure

**Priority**: HIGH
**Risk**: MEDIUM
**Estimated conflicts**: CLAUDE.md (significant but additive)

### What This Adds
- `11-external-resources/plugins/claude-code-advisor/` — Complete plugin (50 files)
  - 10 specialized subagents
  - 22 reference documents
  - 4 slash commands
  - Full documentation suite
- `.repo-metadata.json` — Repository statistics
- `CHANGELOG.md` — Change history
- `CONTRIBUTING.md` — Contribution guidelines
- Enhanced tools with commit SHA tracking
- `.source` files for all skills

### Execution Steps

```bash
# Step 2.1: Ensure main is updated from Phase 1
git checkout main
git pull origin main

# Step 2.2: Create merge branch
git checkout -b merge/phase-2-maintenance

# Step 2.3: Merge maintenance-infrastructure
git merge origin/feature/maintenance-infrastructure --no-ff -m "Merge feature/maintenance-infrastructure

Adds:
- claude-code-advisor plugin (10 agents, 22 references, 4 commands)
- .repo-metadata.json for repository statistics
- CHANGELOG.md and CONTRIBUTING.md
- Enhanced fetch-skill.sh with commit SHA tracking
- freshness-report.sh, regenerate-catalog.sh, generate-stats.sh
- .source files with commit SHAs for all skills"
```

### Expected Conflicts

| File | Conflict Type | Resolution Strategy |
|------|--------------|---------------------|
| `CLAUDE.md` | Both modified | Manual merge - combine both additions |
| `_repo-maintenance/03-recommendations.md` | Minor | Accept incoming |
| `_repo-maintenance/README.md` | Minor | Accept incoming |
| `_repo-maintenance/context-for-llm-agents.md` | Minor | Accept incoming |
| `11-external-resources/_meta/CONTINUATION-GUIDE.md` | Minor | Accept incoming |

### CLAUDE.md Conflict Resolution (Manual)

The maintenance branch adds to CLAUDE.md:
1. **Repository Structure**: New files (CHANGELOG.md, CONTRIBUTING.md, .repo-metadata.json)
2. **Tool Locations**: New tools (freshness-report.sh, regenerate-catalog.sh, etc.)
3. **Source Tracking**: Updated .source file description with commit SHA
4. **Common Tasks**: New tasks (freshness check, regenerate catalog, generate stats)
5. **Revision History**: New entries for 2026-01-07

**Resolution approach**:
```bash
# Open CLAUDE.md in editor
# The Phase 1 version has the navigation additions
# The maintenance branch has the tool/infrastructure additions
# These are COMPLEMENTARY - merge by keeping BOTH sets of changes

# Sections to add from maintenance branch:
# 1. In "Repository Structure" - add CHANGELOG.md, CONTRIBUTING.md, .repo-metadata.json lines
# 2. In "Tool Locations" - add 5 new tools
# 3. In "Source Tracking > External Skills" - update description
# 4. In "Common Tasks" - add 3 new task sections
# 5. In "Revision History" - add 4 new entries (before 2026-01-09 entries)

git add CLAUDE.md
```

### Validation

```bash
# Verify claude-code-advisor exists
ls -la 11-external-resources/plugins/claude-code-advisor/

# Verify new root files
ls -la CHANGELOG.md CONTRIBUTING.md .repo-metadata.json

# Verify new tools
ls -la 11-external-resources/tools/freshness-report.sh
ls -la 11-external-resources/tools/regenerate-catalog.sh

# Count plugin files
find 11-external-resources/plugins/claude-code-advisor -type f | wc -l  # Expected: ~50

# Verify .source files have commit SHAs
head -3 11-external-resources/core-skills/obra-workflow/brainstorming/.source
```

### Push Phase 2

```bash
git checkout main
git merge merge/phase-2-maintenance --ff-only
git push origin main
git branch -d merge/phase-2-maintenance
```

---

## Phase 3: Merge Context Introspection Plugin

**Priority**: MEDIUM
**Risk**: LOW
**Estimated conflicts**: None expected

### Pre-Merge Cleanup Required

The branch has a commit message issue: `5eee2cb test commit`. This should be cleaned up.

**Option A: Squash merge (Recommended)**
```bash
# Squash the 2 commits into 1 clean commit
git checkout -b merge/phase-3-introspection
git merge --squash origin/feature/context-introspection-plugin
git commit -m "Add context-introspection plugin

Adds a plugin that generates comprehensive reports of all context
sources influencing Claude Code sessions:
- Python script enumerating CLAUDE.md, skills, hooks, MCP, agents
- /context-introspection:report slash command
- Plugin manifest and marketplace.json

The report shows memory hierarchy, loaded skills, active hooks,
MCP servers, custom agents, and commands with file links and previews."
```

**Option B: Interactive rebase (If preserving individual commits matters)**
```bash
git checkout feature/context-introspection-plugin
git rebase -i origin/feature/navigation-and-best-practices
# Change "pick" to "reword" for 5eee2cb
# Save with proper commit message
git push --force origin feature/context-introspection-plugin
# Then merge normally
```

### What This Adds
- `11-external-resources/plugins/context-introspection/` — Complete plugin
  - `scripts/introspect.py` — 756-line Python enumeration script
  - `commands/report.md` — Slash command definition
  - `README.md` — Usage documentation
  - `.claude-plugin/plugin.json` — Plugin manifest
- `11-external-resources/plugins/marketplace.json` — Marketplace registry file

### Execution Steps (Using Option A)

```bash
# Step 3.1: Ensure main is updated from Phase 2
git checkout main
git pull origin main

# Step 3.2: Squash merge
git merge --squash origin/feature/context-introspection-plugin

# Step 3.3: Create clean commit
git commit -m "Add context-introspection plugin

Adds a plugin that generates comprehensive reports of all context
sources influencing Claude Code sessions:
- Python script enumerating CLAUDE.md, skills, hooks, MCP, agents
- /context-introspection:report slash command
- Plugin manifest and marketplace.json

The report shows memory hierarchy, loaded skills, active hooks,
MCP servers, custom agents, and commands with file links and previews."

# Step 3.4: Push
git push origin main
```

### Validation

```bash
# Verify plugin exists
ls -la 11-external-resources/plugins/context-introspection/

# Verify Python script
head -20 11-external-resources/plugins/context-introspection/scripts/introspect.py

# Verify marketplace.json
cat 11-external-resources/plugins/marketplace.json

# Test Python syntax
python3 -m py_compile 11-external-resources/plugins/context-introspection/scripts/introspect.py
```

---

## Phase 4: Post-Merge Consolidation

**Priority**: HIGH
**Risk**: LOW

After all merges, perform these consolidation tasks.

### 4.1 Update CATALOG.md

The two new plugins need to be added to the skills catalog.

```bash
# Option A: Use regenerate script
cd 11-external-resources/tools/
./regenerate-catalog.sh --output ../CATALOG.md

# Option B: Manual addition
# Add entries for:
# - plugins/claude-code-advisor
# - plugins/context-introspection
```

**Manual entries to add** (if not using regenerate script):

```markdown
### Plugins

| Plugin | Description | Status |
|--------|-------------|--------|
| [claude-code-advisor](plugins/claude-code-advisor/) | Deep knowledge about Claude Code extensibility features | Production |
| [context-introspection](plugins/context-introspection/) | Generate reports of all context sources | Production |
```

### 4.2 Update Repository Statistics

```bash
cd 11-external-resources/tools/
./generate-stats.sh --update-metadata
```

### 4.3 Verify CLAUDE.md Completeness

Ensure CLAUDE.md has all sections from all branches:

```bash
# Check for key sections
grep -q "Finding Documentation by Task" CLAUDE.md && echo "Navigation: OK" || echo "Navigation: MISSING"
grep -q "freshness-report.sh" CLAUDE.md && echo "Tools: OK" || echo "Tools: MISSING"
grep -q "12-best-practices" CLAUDE.md && echo "Best Practices: OK" || echo "Best Practices: MISSING"
grep -q "context-introspection" CLAUDE.md && echo "Introspection: OK" || echo "Introspection: MISSING"
```

### 4.4 Add CLAUDE.md Plugin References

Add mentions of the new plugins to CLAUDE.md if not present:

```markdown
### "I need a plugin to understand Claude Code features"
→ Use claude-code-advisor: `11-external-resources/plugins/claude-code-advisor/`

### "I need to see what context is loaded"
→ Use context-introspection: `11-external-resources/plugins/context-introspection/`
```

### 4.5 Update Revision History

Add to CLAUDE.md Revision History:

```markdown
| 2026-01-09 | Merged navigation-and-best-practices: 12-best-practices/, 13-external-resources-guide/, per-folder READMEs, docs-index.md |
| 2026-01-09 | Merged maintenance-infrastructure: claude-code-advisor plugin, CHANGELOG.md, CONTRIBUTING.md, enhanced tooling |
| 2026-01-09 | Merged context-introspection-plugin: context enumeration plugin with marketplace.json |
```

### 4.6 Review Plugin Documentation Overlap

Check for redundancy between:
- `07-plugins/` (official docs)
- `11-external-resources/plugins/*.md` (ecosystem guides)

**If significant overlap exists**, add cross-references rather than duplicate content:

```markdown
<!-- In 11-external-resources/plugins/README.md -->
> For official plugin documentation, see [07-plugins/](../../07-plugins/).
> This section covers community ecosystem and discovery.
```

### 4.7 Run Full Validation

```bash
# Verify all expected directories exist
for dir in 12-best-practices 13-external-resources-guide 11-external-resources/plugins/claude-code-advisor 11-external-resources/plugins/context-introspection; do
  [ -d "$dir" ] && echo "$dir: OK" || echo "$dir: MISSING"
done

# Verify key files
for file in CHANGELOG.md CONTRIBUTING.md .repo-metadata.json docs-index.md; do
  [ -f "$file" ] && echo "$file: OK" || echo "$file: MISSING"
done

# Count total files
echo "Total markdown files: $(find . -name '*.md' -not -path './.git/*' | wc -l)"
```

---

## Phase 5: Branch Preservation (REVISED)

> **IMPORTANT**: Original branches are PRESERVED for reference and potential redo.
> Do NOT delete any feature branches after merging.

### 5.1 Keep All Remote Branches

All feature branches remain on origin for historical reference:
- `origin/feature/navigation-and-best-practices`
- `origin/feature/maintenance-infrastructure`
- `origin/feature/plugin-ecosystem-documentation`
- `origin/feature/context-introspection-plugin`

### 5.2 Document Branch Purpose

After merge completion, these branches serve as:
- **Reference points** if merge issues are discovered later
- **Audit trail** of parallel agent work
- **Redo capability** if consolidation needs adjustment

### 5.3 Optional: Archive Tag

If desired, create archive tags for clarity:
```bash
git tag archive/pre-merge/navigation-and-best-practices origin/feature/navigation-and-best-practices
git tag archive/pre-merge/maintenance-infrastructure origin/feature/maintenance-infrastructure
git push origin --tags
```

---

## Rollback Procedures

### If Phase 1 Fails
```bash
git checkout main
git reset --hard origin/main
git branch -D merge/phase-1-navigation
```

### If Phase 2 Fails
```bash
git checkout main
git reset --hard <commit-before-phase-2>
git branch -D merge/phase-2-maintenance
```

### Complete Rollback
```bash
git checkout main
git reset --hard backup/pre-merge-2026-01-09
git push --force origin main  # CAUTION: Only if no one else has pulled
```

---

## Summary Checklist

- [ ] **Pre-merge**: Create backup branch
- [ ] **Pre-merge**: Verify all remote branches are fetched
- [ ] **Phase 1**: Merge navigation-and-best-practices
- [ ] **Phase 1**: Resolve CLAUDE.md conflicts (keep theirs)
- [ ] **Phase 1**: Validate new directories exist
- [ ] **Phase 1**: Push to main
- [ ] **Phase 2**: Merge maintenance-infrastructure
- [ ] **Phase 2**: Manually merge CLAUDE.md (combine additions)
- [ ] **Phase 2**: Validate claude-code-advisor plugin
- [ ] **Phase 2**: Push to main
- [ ] **Phase 3**: Squash merge context-introspection-plugin
- [ ] **Phase 3**: Validate plugin and Python syntax
- [ ] **Phase 3**: Push to main
- [ ] **Phase 4**: Update CATALOG.md with new plugins
- [ ] **Phase 4**: Update repository statistics
- [ ] **Phase 4**: Verify CLAUDE.md completeness
- [ ] **Phase 4**: Add plugin references to CLAUDE.md
- [ ] **Phase 4**: Update revision history
- [ ] **Phase 4**: Check for documentation overlap
- [ ] **Phase 4**: Run full validation
- [ ] **Phase 5**: Delete merged remote branches
- [ ] **Phase 5**: Delete local branches
- [ ] **Phase 5**: Prune remote tracking references

---

## Expected Final State

After all merges, the repository will contain:

```
/
├── 01-getting-started/        # + README.md
├── 02-core-features/          # + README.md
├── 03-ide-integration/        # + README.md
├── 04-cloud-providers/        # + README.md
├── 05-enterprise/             # + README.md
├── 06-cicd-automation/        # + README.md
├── 07-plugins/                # + README.md
├── 08-configuration/          # + README.md
├── 09-reference/              # + README.md
├── 10-other/                  # + README.md
├── 11-external-resources/
│   ├── plugins/
│   │   ├── claude-code-advisor/     # NEW: 50 files
│   │   ├── context-introspection/   # NEW: 4 files
│   │   ├── ECOSYSTEM-GUIDE.md       # NEW
│   │   ├── MARKETPLACES.md          # NEW
│   │   ├── REGISTRY.md              # NEW
│   │   ├── marketplace.json         # NEW
│   │   └── ...
│   └── tools/
│       ├── freshness-report.sh      # NEW
│       ├── regenerate-catalog.sh    # NEW
│       ├── generate-stats.sh        # NEW
│       └── ...
├── 12-best-practices/               # NEW: 10 files
├── 13-external-resources-guide/     # NEW: 7 files
├── _repo-maintenance/
├── .gitignore                       # NEW
├── .repo-metadata.json              # NEW
├── CHANGELOG.md                     # NEW
├── CLAUDE.md                        # Enhanced
├── CONTRIBUTING.md                  # NEW
├── docs-index.md                    # NEW
├── README.md                        # Enhanced
└── ...
```

**Total additions**: ~12,400 lines, 103 files, 2 complete plugins

---

## Notes for Executing Agent

1. **Take your time with CLAUDE.md in Phase 2** — it's the most complex merge
2. **Squash merge is strongly recommended for Phase 3** — cleaner history
3. **Run validation after each phase** — don't proceed if validation fails
4. **The backup branch is your safety net** — don't delete until fully validated
5. **Post-merge consolidation is important** — don't skip Phase 4
