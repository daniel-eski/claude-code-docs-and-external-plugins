# External Skills Integration: Implementation Plan

> ⚠️ **HISTORICAL DOCUMENT** — This plan was created in January 2026 and has been **completed**. It is preserved for reference to understand the design decisions behind the `11-external-resources/` structure.
>
> **Status**: ✅ Implemented  
> **Completed**: January 6-7, 2026  
> **See also**: [`_repo-maintenance/`](_repo-maintenance/) for current maintenance analysis and future improvements

---

## Implementation Summary

### What Was Planned vs. Implemented

| Planned | Actual Implementation | Notes |
|---------|----------------------|-------|
| `high-priority/` folder | `core-skills/` folder | Clearer naming |
| `medium-priority/` folder | `extended-skills/` folder | Clearer naming |
| `scripts/` folder | `tools/` folder | Matches conventions |
| `SKILLS_CATALOG.md` | `CATALOG.md` | Shorter name |
| `DEPLOYMENT_GUIDE.md` | `QUICKSTART.md` | More action-oriented |
| 31 high-priority skills | 28 working + 2 placeholders | 2 repos inaccessible |
| 9 medium-priority skills | 4 AWS skills + links | Context engineering as links only |
| Commit SHA tracking | Fetch timestamp only | **Gap identified** — see `_repo-maintenance/03-recommendations.md` |

### Skills Successfully Fetched

| Category | Planned | Actual | Status |
|----------|---------|--------|--------|
| obra-workflow | 7 | 6 | ✅ Complete |
| obra-development | 14 | 9 | ✅ Complete (some consolidated) |
| git-workflow | 2 | 6 | ✅ Exceeded (fvadicamo has 5) |
| testing | 2 | 1 working, 1 placeholder | ⚠️ pypict-skill inaccessible |
| document-creation | 5 | 5 | ✅ Complete |
| skill-authoring | (not planned) | 2 | ✅ Added (templates) |
| aws-skills | 4 | 4 | ✅ Complete |

### Infrastructure Created

- ✅ `fetch-skill.sh` — Downloads skills from GitHub
- ✅ `validate-skill.sh` — Validates SKILL.md format
- ✅ `deploy-skill.sh` — Deploys single skills
- ✅ `deploy-all.sh` — Bulk deployment
- ✅ `update-sources.sh` — Re-fetch all skills
- ✅ `.source` files — Track origin and fetch timestamp

---

## Original Plan (Preserved for Reference)

<details>
<summary>Click to expand original planning document</summary>

## Objective

Enhance this documentation repository by integrating external Claude Skills from the community, creating a deployable resource for developing and testing skills.

---

## Phase 1: Foundation (Directory Structure & Catalog)

### 1.1 Directory Structure

```
11-external-resources/
├── README.md                    # Overview, usage guide, contribution guidelines
├── SKILLS_CATALOG.md            # Complete indexed catalog with all skills
├── DEPLOYMENT_GUIDE.md          # How to deploy skills to ~/.claude/skills/
│
├── high-priority/
│   ├── README.md                # Index of high-priority skills
│   │
│   ├── obra-workflow/           # Jesse Vincent's workflow skills
│   │   ├── README.md            # Overview of obra's skill philosophy
│   │   ├── superpowers-lab/
│   │   ├── brainstorming/
│   │   ├── writing-plans/
│   │   ├── executing-plans/
│   │   ├── dispatching-parallel-agents/
│   │   ├── sharing-skills/
│   │   └── using-superpowers/
│   │
│   ├── obra-development/        # obra's development-focused skills
│   │   ├── test-driven-development/
│   │   ├── subagent-driven-development/
│   │   ├── systematic-debugging/
│   │   ├── root-cause-tracing/
│   │   ├── testing-skills-with-subagents/
│   │   ├── testing-anti-patterns/
│   │   ├── finishing-a-development-branch/
│   │   ├── requesting-code-review/
│   │   ├── receiving-code-review/
│   │   ├── using-git-worktrees/
│   │   ├── verification-before-completion/
│   │   ├── condition-based-waiting/
│   │   ├── commands/
│   │   └── writing-skills/
│   │
│   ├── git-workflow/            # Git/GitHub workflow skills
│   │   ├── fvadicamo-dev-agent-skills/
│   │   └── composio-changelog-generator/
│   │
│   ├── testing/                 # Testing-specific skills
│   │   ├── anthropics-webapp-testing/
│   │   └── omkamal-pypict-skill/
│   │
│   └── document-creation/       # Anthropic's document skills
│       ├── docx/
│       ├── doc-coauthoring/
│       ├── pptx/
│       ├── xlsx/
│       └── pdf/
│
├── medium-priority/
│   ├── README.md
│   │
│   ├── aws-skills/              # zxkane's AWS development skills
│   │
│   └── context-engineering/     # muratcankoylan's context series
│       ├── README.md            # Overview linking to all context skills
│       ├── context-fundamentals/
│       ├── context-degradation/
│       ├── context-compression/
│       ├── context-optimization/
│       ├── multi-agent-patterns/
│       ├── memory-systems/
│       ├── tool-design/
│       └── evaluation/
│
├── reference/
│   ├── README.md
│   ├── official-guides.md       # Links to Anthropic's official skill guides
│   └── lower-priority-catalog.md # Indexed links to lower priority skills
│
└── scripts/
    ├── fetch-skill.sh           # Download a skill from GitHub
    ├── deploy-skill.sh          # Deploy skill to ~/.claude/skills/
    ├── validate-skill.sh        # Validate SKILL.md format
    └── update-catalog.sh        # Regenerate catalog from local skills
```

### 1.2 Catalog Format

Each skill entry in `SKILLS_CATALOG.md` will include:

```markdown
### skill-name
- **Source**: GitHub URL
- **Author**: author-name
- **Priority**: high/medium/lower
- **Category**: workflow/development/testing/etc.
- **Local Path**: `high-priority/category/skill-name/` or "Link only"
- **Status**: Downloaded | Link Only | Verified
- **Description**: Brief description of what the skill does
- **Dependencies**: Any required packages or tools
```

---

## Phase 2: Content Acquisition

### 2.1 Fetching Strategy

For each high-priority skill:

1. **Identify the SKILL.md file** (or equivalent) in the GitHub repo
2. **Fetch raw content** using GitHub raw URLs
3. **Preserve directory structure** (supporting files, scripts if present)
4. **Document the source** with commit SHA for reproducibility

### 2.2 High-Priority Skills to Fetch (31 skills)

| Category | Skills | Source |
|----------|--------|--------|
| obra-workflow | 7 skills | github.com/obra/* |
| obra-development | 14 skills | github.com/obra/* |
| git-workflow | 2 skills | fvadicamo, ComposioHQ |
| testing | 2 skills | anthropics, omkamal |
| document-creation | 5 skills | anthropics/* |

### 2.3 Medium-Priority Skills (9 skills)

- Will fetch: AWS skills (zxkane)
- Will catalog with links: Context engineering series (muratcankoylan)

### 2.4 Lower/Lowest Priority

- Catalog with links only
- Include in `reference/lower-priority-catalog.md`

---

## Phase 3: Documentation

### 3.1 Usage Documentation (`DEPLOYMENT_GUIDE.md`)

Contents:
1. **Quick Start**: Deploy a single skill in 30 seconds
2. **Bulk Deployment**: Script to deploy all high-priority skills
3. **Testing Skills**: How to verify a skill works
4. **Customization**: How to modify skills for your workflow
5. **Troubleshooting**: Common issues and solutions

### 3.2 Per-Category READMEs

Each category folder gets a README with:
- Overview of the category's purpose
- List of included skills with brief descriptions
- Recommended deployment order
- Inter-skill dependencies (if any)

---

## Phase 4: Verification & Testing

### 4.1 Skill Validation Checks

For each downloaded skill, verify:

1. **Structure**: SKILL.md exists with valid YAML frontmatter
2. **Metadata**: `name` and `description` fields present
3. **Syntax**: YAML parses correctly
4. **References**: Any linked files exist locally

### 4.2 Validation Script (`scripts/validate-skill.sh`)

```bash
#!/bin/bash
# Validates a skill directory
# Usage: ./validate-skill.sh path/to/skill/

SKILL_PATH="$1"
SKILL_MD="$SKILL_PATH/SKILL.md"

# Check SKILL.md exists
if [ ! -f "$SKILL_MD" ]; then
    echo "FAIL: SKILL.md not found"
    exit 1
fi

# Check frontmatter exists
if ! head -1 "$SKILL_MD" | grep -q "^---"; then
    echo "FAIL: Missing YAML frontmatter"
    exit 1
fi

# Check required fields
if ! grep -q "^name:" "$SKILL_MD"; then
    echo "FAIL: Missing 'name' field"
    exit 1
fi

if ! grep -q "^description:" "$SKILL_MD"; then
    echo "FAIL: Missing 'description' field"
    exit 1
fi

echo "PASS: Skill is valid"
```

### 4.3 Functional Test Protocol

After deploying skills to `~/.claude/skills/`:

1. Start fresh Claude Code session
2. Ask "What Skills are available?" - verify skill appears
3. Trigger the skill with a matching request
4. Confirm skill activates and functions correctly

---

## Phase 5: Maintenance

### 5.1 Update Strategy

- **scripts/update-catalog.sh**: Regenerates SKILLS_CATALOG.md from local skills
- Document source commit SHAs for tracking upstream changes
- Periodic review of awesome-claude-skills for new additions

### 5.2 CLAUDE.md Update

Add section documenting:
- New `11-external-resources/` directory
- How to deploy skills
- Skill categories and priorities

---

## Execution Order

| Step | Task | Verification |
|------|------|--------------|
| 1 | Create directory structure | `ls -la 11-external-resources/` |
| 2 | Create SKILLS_CATALOG.md skeleton | File exists with headers |
| 3 | Create DEPLOYMENT_GUIDE.md | File exists with sections |
| 4 | Fetch obra-workflow skills (7) | SKILL.md files present |
| 5 | Fetch obra-development skills (14) | SKILL.md files present |
| 6 | Fetch git-workflow skills (2) | SKILL.md files present |
| 7 | Fetch testing skills (2) | SKILL.md files present |
| 8 | Fetch document-creation skills (5) | SKILL.md files present |
| 9 | Fetch medium-priority skills | Files/links present |
| 10 | Create reference/lower-priority-catalog.md | Links documented |
| 11 | Create validation scripts | Scripts executable |
| 12 | Run validation on all downloaded skills | All pass |
| 13 | Create category READMEs | Each folder has README |
| 14 | Update root CLAUDE.md | New section present |
| 15 | Update root README.md | New section present |

---

## Success Criteria

1. **Completeness**: All 31 high-priority skills downloaded and cataloged
2. **Validity**: All skills pass validation checks
3. **Usability**: Any skill can be deployed with documented steps
4. **Documentation**: Clear guides for deployment and testing
5. **Maintainability**: Scripts exist for updates and validation

---

## Risk Mitigation

| Risk | Mitigation |
|------|------------|
| GitHub repo not found | Log error, continue with others |
| SKILL.md format varies | Adapt fetching logic, document variations |
| Skills have dependencies | Document in catalog, note in READMEs |
| Rate limiting on fetches | Add delays between requests |
| Large supporting files | Only fetch SKILL.md + essential refs |

---

## Questions Before Proceeding

1. **Deployment preference**: Should the guide default to `~/.claude/skills/` (personal) or `.claude/skills/` (project)?
2. **Anthropic skills**: The anthropics/* repos may require special access. Should I attempt to fetch them, or just document links?
3. **Supporting files**: Some skills have scripts/helpers. Fetch all, or SKILL.md only initially?

</details>

---

## Lessons Learned

1. **Repository accessibility varies**: Some GitHub repos are private or renamed. Creating placeholder skills with `.source` files preserves the intent for future retry.

2. **Naming matters**: "core-skills" is more intuitive than "high-priority" — describes content, not importance.

3. **Commit SHA tracking should have been included from the start**: The `.source` files track fetch timestamp but not the upstream commit, making change detection impossible. This is documented as a gap in `_repo-maintenance/03-recommendations.md`.

4. **meta/ folder is valuable**: Adding skill authoring documentation (`skill-anatomy.md`, `patterns-and-antipatterns.md`) significantly improves the repository's usefulness.

---

## Current State

For the current repository state and future improvement plans, see:
- [`_repo-maintenance/01-current-state.md`](_repo-maintenance/01-current-state.md) — Full inventory
- [`_repo-maintenance/03-recommendations.md`](_repo-maintenance/03-recommendations.md) — Improvement roadmap
- [`11-external-resources/CATALOG.md`](11-external-resources/CATALOG.md) — Live skills catalog
