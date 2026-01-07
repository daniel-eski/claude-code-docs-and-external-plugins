# Identified Challenges

> Problems and gaps that affect repository scalability and maintainability.

---

## Challenge Categories

1. [Staleness Detection](#1-staleness-detection)
2. [Manual Maintenance Burden](#2-manual-maintenance-burden)
3. [Incomplete Tracking](#3-incomplete-tracking)
4. [Missing Automation](#4-missing-automation)
5. [Content Gaps](#5-content-gaps)

---

## 1. Staleness Detection

### 1.1 Core Documentation Has No Update Detection

**Problem**: The 49 documentation files in folders `01-10` are copied from `code.claude.com/docs`, but there's no way to know when upstream content changes.

**Current State**:
- Single "Last Updated" date in README.md (manually maintained)
- No per-file timestamps or hashes
- No comparison mechanism with upstream

**Impact**:
- Documentation will silently become outdated
- Users may rely on stale information
- No way to generate a "what changed" report

**Example Scenario**: Anthropic updates the `skills.md` documentation with new features. This repo's copy becomes outdated, but there's no alert or detection.

### 1.2 External Skills Have Limited Change Detection

**Problem**: While `.source` files track *when* a skill was fetched, they don't track *what version* was fetched.

**Current State** (from `.source` files):
```
source: https://github.com/obra/superpowers/tree/main/skills/brainstorming
fetched: 2026-01-07T00:17:35Z
```

**Missing**:
- No commit SHA (can't compare with current upstream)
- No file hash (can't detect if content changed)
- `update-sources.sh` re-fetches blindly without knowing if updates exist

**Impact**:
- Running `update-sources.sh --fetch` downloads everything regardless of changes
- No way to answer "which skills have upstream updates?"
- Wasted bandwidth and time on unnecessary fetches

---

## 2. Manual Maintenance Burden

### 2.1 README.md Statistics

**Problem**: Stats in README.md are hardcoded:

```markdown
- **Total Documentation Files:** 49
- **Categories:** 11 (including external resources)
- **External Skills:** 32 (28 core + 4 extended)
```

**Impact**: Adding/removing files requires manual stat updates, which will be forgotten.

### 2.2 CATALOG.md Line Counts

**Problem**: The skill catalog includes line counts that are manually maintained:

```markdown
| brainstorming | 54 | Generate and explore ideas | Available |
| writing-plans | 116 | Create strategic documentation | Available |
```

**Impact**: If skills are updated, line counts become inaccurate.

### 2.3 Last Updated Dates

**Problem**: Multiple "Last Updated" dates across files:
- README.md: "Last Updated: January 6, 2026"
- CATALOG.md: "Last updated: 2026-01-06"

**Impact**: Easy to update one but forget the other.

---

## 3. Incomplete Tracking

### 3.1 No Metadata for Core Docs

**Problem**: Core documentation files have no source metadata.

**Comparison**:

| Feature | External Skills | Core Docs |
|---------|-----------------|-----------|
| Source URL tracked | ✅ `.source` files | ❌ None |
| Fetch timestamp | ✅ `fetched:` field | ❌ None |
| Update script | ✅ `update-sources.sh` | ❌ None |

### 3.2 Placeholder Skills Lack Resolution Tracking

**Problem**: Two skills are placeholders because repos weren't accessible:
- `changelog-generator` (ComposioHQ)
- `pypict-skill` (omkamal)

**Missing**:
- No record of *why* they failed (404? private? rate limited?)
- No retry mechanism or freshness check
- No alerting if repos become available

### 3.3 Community Catalog Links Not Validated

**Problem**: `reference/community-catalog.md` contains ~30+ links to external skills.

**Missing**:
- No link validation
- Links may point to renamed, moved, or deleted repos
- No indication of which links are stale

---

## 4. Missing Automation

### 4.1 No CI/CD Pipeline

**Problem**: No automated validation or checks.

**Missing Automation**:
- Skill validation on PR
- Link checking
- Freshness reports
- Catalog regeneration

### 4.2 No Scheduled Maintenance

**Problem**: No mechanism for periodic health checks.

**Desirable**:
- Weekly/monthly freshness check against upstream
- Automated issue creation for stale content
- Dependency update checks

### 4.3 No Bulk Catalog Update

**Problem**: After fetching updated skills, catalog must be manually updated.

**Current Workflow** (inefficient):
1. Run `update-sources.sh --fetch`
2. Manually count lines for each skill
3. Manually update CATALOG.md
4. Manually update README.md stats

**Desired Workflow**:
1. Run `update-sources.sh --fetch`
2. Run `regenerate-catalog.sh` (doesn't exist)
3. Commit changes

---

## 5. Content Gaps

### 5.1 Missing Changelog

**Problem**: No record of what changed between updates.

**Impact**:
- Can't communicate "what's new" to users
- Can't track which skills were updated
- Can't identify regressions

### 5.2 Incomplete Skill Coverage

**Problem**: Some planned skills weren't fetched:
- Context engineering skills (only links, no local copies)
- Various skills from community catalog

**Reason**: Educational decision (links sufficient for some content) vs. access issues (repos not accessible).

### 5.3 No Contribution Guidelines

**Problem**: No CONTRIBUTING.md explaining how to:
- Add new skills
- Report stale content
- Suggest documentation updates

---

## Challenge Priority Matrix

| Challenge | Severity | Effort to Fix | Frequency of Impact |
|-----------|----------|---------------|---------------------|
| No core doc change detection | High | Medium | Continuous |
| No commit SHA tracking | High | Low | Every update check |
| Manual catalog updates | Medium | Low | Every skill update |
| No CI/CD | Medium | Medium | Every PR |
| Placeholder skills stuck | Low | Low | Rare |
| Community links not validated | Low | Medium | Rare |
| No changelog | Medium | Low | Every update |
| No CONTRIBUTING.md | Low | Low | New contributor |

---

## Dependency Graph

Some challenges depend on solving others first:

```
[Commit SHA tracking] ──────┐
                            ├──► [Freshness detection] ──► [Automated alerts]
[Core doc metadata] ────────┘
                                          │
[Catalog auto-generation] ◄───────────────┘
                            │
[CI/CD pipeline] ◄──────────┴──► [Link validation]
```

---

## Summary

**Critical Gaps**:
1. Cannot detect when upstream content changes (both core docs and skills)
2. Too much manual maintenance for catalog and stats
3. No automation for validation or health checks

**Root Cause**: The repository was designed for initial bulk import, not ongoing maintenance.

**Solution Direction**: See [03-recommendations.md](03-recommendations.md) for prioritized solutions.

