# Skill Sources Registry

**Last Verified**: 2026-01-06
**Purpose**: Authoritative reference for skill repository structures, verified through actual fetching.

---

## Why This Document Exists

Curated lists like [awesome-claude-skills](https://github.com/VoltAgent/awesome-claude-skills) are helpful for discovery but often contain:
- Individual repo URLs that are actually paths within consolidated repos
- Stale links to renamed/deleted/private repositories
- Inconsistent path patterns

This registry documents the **actual, verified** structure of skill sources based on successful fetches.

---

## Verified Consolidated Repositories

These repos contain multiple skills in a single repository.

### obra/superpowers

**URL**: https://github.com/obra/superpowers
**Author**: Jesse Vincent
**Skill path pattern**: `/skills/<skill-name>/SKILL.md`
**Verified**: 2026-01-06

| Skill Name | Path | Lines | Status |
|------------|------|-------|--------|
| brainstorming | /skills/brainstorming/ | 54 | OK |
| writing-plans | /skills/writing-plans/ | 116 | OK |
| executing-plans | /skills/executing-plans/ | 76 | OK |
| dispatching-parallel-agents | /skills/dispatching-parallel-agents/ | 180 | OK |
| using-superpowers | /skills/using-superpowers/ | 87 | OK |
| test-driven-development | /skills/test-driven-development/ | 371 | OK |
| subagent-driven-development | /skills/subagent-driven-development/ | 240 | OK |
| systematic-debugging | /skills/systematic-debugging/ | 296 | OK |
| finishing-a-development-branch | /skills/finishing-a-development-branch/ | 200 | OK |
| requesting-code-review | /skills/requesting-code-review/ | 105 | OK |
| receiving-code-review | /skills/receiving-code-review/ | 213 | OK |
| using-git-worktrees | /skills/using-git-worktrees/ | 217 | OK |
| verification-before-completion | /skills/verification-before-completion/ | 139 | OK |
| writing-skills | /skills/writing-skills/ | 655 | OK (>500 lines) |

**Skills listed elsewhere that DON'T exist here**:
- root-cause-tracing (404)
- testing-skills-with-subagents (404)
- testing-anti-patterns (404)
- condition-based-waiting (404)
- commands (404)
- sharing-skills (404)

---

### obra/superpowers-lab

**URL**: https://github.com/obra/superpowers-lab
**Author**: Jesse Vincent
**Skill path pattern**: `/skills/<skill-name>/SKILL.md`
**Description**: Experimental skills under active development
**Verified**: 2026-01-06

| Skill Name | Path | Lines | Status |
|------------|------|-------|--------|
| using-tmux-for-interactive-commands | /skills/using-tmux-for-interactive-commands/ | 178 | OK |

**Note**: This repo may have more experimental skills added over time.

---

### anthropics/skills

**URL**: https://github.com/anthropics/skills
**Author**: Anthropic (official)
**Stars**: 34.4k+
**Skill path pattern**: `/skills/<skill-name>/SKILL.md`
**Template path**: `/template/SKILL.md` (NOT in /skills/)
**Verified**: 2026-01-06

| Skill Name | Path | Lines | Status |
|------------|------|-------|--------|
| docx | /skills/docx/ | 196 | OK |
| doc-coauthoring | /skills/doc-coauthoring/ | 375 | OK |
| pptx | /skills/pptx/ | 483 | OK |
| xlsx | /skills/xlsx/ | 288 | OK |
| pdf | /skills/pdf/ | 294 | OK |
| webapp-testing | /skills/webapp-testing/ | 95 | OK |
| skill-creator | /skills/skill-creator/ | 356 | OK |
| template | /template/ | 6 | OK |

**Other skills available but not fetched**:
- algorithmic-art
- brand-guidelines
- canvas-design
- frontend-design
- internal-comms
- mcp-builder
- slack-gif-creator
- theme-factory
- web-artifacts-builder

---

### fvadicamo/dev-agent-skills

**URL**: https://github.com/fvadicamo/dev-agent-skills
**Author**: Francesco Vadicamo
**Skill path pattern**: `/skills/<skill-name>/SKILL.md`
**Verified**: 2026-01-06

| Skill Name | Path | Lines | Status |
|------------|------|-------|--------|
| git-commit | /skills/git-commit/ | 235 | OK |
| github-pr-creation | /skills/github-pr-creation/ | 201 | OK |
| github-pr-merge | /skills/github-pr-merge/ | 210 | OK |
| github-pr-review | /skills/github-pr-review/ | 235 | OK |
| creating-skills | /skills/creating-skills/ | 261 | OK |

---

### zxkane/aws-skills

**URL**: https://github.com/zxkane/aws-skills
**Author**: zxkane
**Skill path pattern**: `/skills/<skill-name>/SKILL.md`
**Verified**: 2026-01-06

| Skill Name | Path | Lines | Status |
|------------|------|-------|--------|
| aws-agentic-ai | /skills/aws-agentic-ai/ | 117 | OK |
| aws-cdk-development | /skills/aws-cdk-development/ | 278 | OK |
| aws-cost-operations | /skills/aws-cost-operations/ | 317 | OK |
| aws-serverless-eda | /skills/aws-serverless-eda/ | 757 | OK (>500 lines) |

---

### muratcankoylan/Agent-Skills-for-Context-Engineering

**URL**: https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering
**Author**: Muratcan Koylan (@99Ravens)
**Skill path pattern**: `/skills/<skill-name>/SKILL.md`
**Type**: Educational content (not fetched locally, links only)
**Verified**: 2026-01-06

| Skill Name | Path | Description |
|------------|------|-------------|
| context-fundamentals | /skills/context-fundamentals/ | What context is, why it matters |
| context-degradation | /skills/context-degradation/ | Context failure patterns |
| context-compression | /skills/context-compression/ | Compression strategies |
| context-optimization | /skills/context-optimization/ | Compaction, masking, caching |
| multi-agent-patterns | /skills/multi-agent-patterns/ | Agent architectures |
| memory-systems | /skills/memory-systems/ | Memory architecture design |
| tool-design | /skills/tool-design/ | Building effective tools |
| evaluation | /skills/evaluation/ | Evaluation frameworks |
| advanced-evaluation | /skills/advanced-evaluation/ | LLM-as-Judge methods |
| bdi-mental-states | /skills/bdi-mental-states/ | BDI frameworks |
| project-development | /skills/project-development/ | LLM app development |

---

## Known Non-Existent Repositories

These were listed in awesome-claude-skills but returned 404 when attempting to fetch.

| Listed URL | Status | Date Checked | Notes |
|------------|--------|--------------|-------|
| ComposioHQ/changelog-generator | 404 | 2026-01-06 | May be private or renamed |
| omkamal/pypict-skill | 404 | 2026-01-06 | May be private or renamed |

---

## URL Construction Patterns

### Raw Content URL (for fetching)
```
https://raw.githubusercontent.com/{owner}/{repo}/main/{path}/SKILL.md
```

### GitHub API URL (for directory listing)
```
https://api.github.com/repos/{owner}/{repo}/contents/{path}
```

### Web URL (for human viewing)
```
https://github.com/{owner}/{repo}/tree/main/{path}
```

---

## Verification Commands

To verify a skill exists before fetching:

```bash
# Check if SKILL.md exists (returns HTTP code)
curl -sL -w "%{http_code}" -o /dev/null \
  "https://raw.githubusercontent.com/{owner}/{repo}/main/skills/{skill-name}/SKILL.md"

# List all skills in a repo
curl -s "https://api.github.com/repos/{owner}/{repo}/contents/skills" | \
  jq -r '.[].name'
```

---

## Maintenance Notes

1. **Re-verify periodically** - Repos evolve, skills get added/removed
2. **Check awesome-claude-skills updates** - New skills may be added
3. **API rate limits** - GitHub API has rate limits; use authentication for bulk operations
4. **Private repos** - Some listed repos may become private; document when this happens

---

## How to Use This Registry

### For Future Agents

When asked to fetch skills:
1. Check this registry first for verified paths
2. If skill not in registry, verify URL exists before fetching
3. Update registry after successful/failed fetches
4. Add new discoveries to appropriate sections

### For Humans

Use this as a reference when:
- Manually adding skills
- Debugging fetch failures
- Understanding where skills come from
- Contributing back to awesome-claude-skills with corrections
