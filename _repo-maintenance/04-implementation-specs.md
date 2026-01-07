# Implementation Specifications

> Detailed technical specifications for implementing each recommendation.

---

## Table of Contents

- [R1: Enhanced .source Files](#r1-enhanced-source-files)
- [R2: Repository Metadata File](#r2-repository-metadata-file)
- [R3: Auto-Generated Stats Script](#r3-auto-generated-stats-script)
- [R4: CHANGELOG.md](#r4-changelogmd)
- [R5: Freshness Report Tool](#r5-freshness-report-tool)
- [R6: Catalog Auto-Generation](#r6-catalog-auto-generation)
- [R7: Link Health Checker](#r7-link-health-checker)
- [R8: CONTRIBUTING.md](#r8-contributingmd)
- [R9: GitHub Actions Workflow](#r9-github-actions-workflow)

---

## R1: Enhanced .source Files

### Goal
Track commit SHA in `.source` files to enable change detection.

### Changes to `fetch-skill.sh`

**Location**: `11-external-resources/tools/fetch-skill.sh`

**Current behavior** (lines 79-84):
```bash
# Record source for tracking
cat > "$LOCAL_PATH/.source" << EOF
source: $GITHUB_URL
fetched: $(date -u +%Y-%m-%dT%H:%M:%SZ)
raw_base: $RAW_BASE
EOF
```

**New behavior**:
```bash
# Get commit SHA if possible
COMMIT_SHA=""
if [[ "$REPO_PART" != "" ]]; then
    # Try to get latest commit for this path
    COMMIT_API="https://api.github.com/repos/$REPO_PART/commits?path=$PATH_PART&per_page=1"
    COMMIT_SHA=$(curl -sL "$COMMIT_API" | grep '"sha"' | head -1 | sed 's/.*"sha": "\([^"]*\)".*/\1/')
fi

# Record source for tracking
cat > "$LOCAL_PATH/.source" << EOF
source: $GITHUB_URL
fetched: $(date -u +%Y-%m-%dT%H:%M:%SZ)
commit_sha: ${COMMIT_SHA:-unknown}
branch: main
raw_base: $RAW_BASE
EOF
```

### Migration for Existing Skills

Create `tools/migrate-source-files.sh`:

```bash
#!/bin/bash
# Add commit_sha field to existing .source files
# Run once to migrate existing skills

for source_file in $(find ../core-skills ../extended-skills -name ".source"); do
    if ! grep -q "^commit_sha:" "$source_file"; then
        # Extract repo info and query for SHA
        SOURCE_URL=$(grep "^source:" "$source_file" | cut -d' ' -f2-)
        # ... (similar logic to fetch-skill.sh)
        echo "commit_sha: unknown" >> "$source_file"
        echo "branch: main" >> "$source_file"
    fi
done
```

### Validation

After implementation, `.source` files should look like:

```yaml
source: https://github.com/obra/superpowers/tree/main/skills/brainstorming
fetched: 2026-01-07T00:17:35Z
commit_sha: a1b2c3d4e5f67890abcdef1234567890abcdef12
branch: main
raw_base: https://raw.githubusercontent.com/obra/superpowers/main/skills/brainstorming
```

---

## R2: Repository Metadata File

### Goal
Create a single source of truth for repository sync state.

### File Location
`/.repo-metadata.json` (repository root)

### Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "version": {
      "type": "string",
      "description": "Schema version for this file"
    },
    "documentation": {
      "type": "object",
      "properties": {
        "source": {"type": "string"},
        "llms_txt_url": {"type": "string"},
        "last_synced": {"type": "string", "format": "date-time"},
        "sync_method": {"type": "string", "enum": ["manual", "automated"]},
        "file_count": {"type": "integer"}
      }
    },
    "skills": {
      "type": "object",
      "properties": {
        "last_bulk_update": {"type": "string", "format": "date-time"},
        "total_core": {"type": "integer"},
        "total_extended": {"type": "integer"},
        "placeholders": {"type": "integer"}
      }
    },
    "generated": {
      "type": "object",
      "properties": {
        "timestamp": {"type": "string", "format": "date-time"},
        "by": {"type": "string"}
      }
    }
  }
}
```

### Initial Content

```json
{
  "version": "1.0",
  "documentation": {
    "source": "https://code.claude.com/docs",
    "llms_txt_url": "https://code.claude.com/docs/llms.txt",
    "last_synced": "2026-01-06T00:00:00Z",
    "sync_method": "manual",
    "file_count": 49
  },
  "skills": {
    "last_bulk_update": "2026-01-07T00:17:35Z",
    "total_core": 28,
    "total_extended": 4,
    "placeholders": 2
  },
  "generated": {
    "timestamp": "2026-01-07T00:00:00Z",
    "by": "manual"
  }
}
```

### Integration with README.md

Update README.md to note that stats are tracked in `.repo-metadata.json` rather than hardcoding values. Or create a script that updates README.md from the JSON.

---

## R3: Auto-Generated Stats Script

### Goal
Generate accurate statistics without manual counting.

### File Location
`11-external-resources/tools/generate-stats.sh`

### Script

```bash
#!/bin/bash
# Generate statistics for the repository
# Usage: ./generate-stats.sh [--json|--markdown]

set -e

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
RESOURCES_DIR="$(dirname "$SCRIPT_DIR")"
REPO_ROOT="$(dirname "$RESOURCES_DIR")"

OUTPUT_FORMAT="${1:---json}"

# Count documentation files (folders 01-10)
DOC_COUNT=$(find "$REPO_ROOT"/0*-* -name "*.md" 2>/dev/null | wc -l | tr -d ' ')

# Count skills
SKILL_COUNT=$(find "$RESOURCES_DIR" -name "SKILL.md" 2>/dev/null | wc -l | tr -d ' ')

# Count working vs placeholder skills
PLACEHOLDER_COUNT=$(grep -l "This skill could not be fetched" "$RESOURCES_DIR"/*/skills/*/SKILL.md "$RESOURCES_DIR"/*/*/SKILL.md 2>/dev/null | wc -l | tr -d ' ')
WORKING_COUNT=$((SKILL_COUNT - PLACEHOLDER_COUNT))

# Count core vs extended
CORE_COUNT=$(find "$RESOURCES_DIR/core-skills" -name "SKILL.md" 2>/dev/null | wc -l | tr -d ' ')
EXTENDED_COUNT=$(find "$RESOURCES_DIR/extended-skills" -name "SKILL.md" 2>/dev/null | wc -l | tr -d ' ')

# Generate skill details
generate_skill_details() {
    for skill_md in $(find "$RESOURCES_DIR" -name "SKILL.md" | sort); do
        skill_dir=$(dirname "$skill_md")
        skill_name=$(basename "$skill_dir")
        lines=$(wc -l < "$skill_md" | tr -d ' ')
        rel_path="${skill_dir#$RESOURCES_DIR/}"
        
        # Extract description from YAML
        desc=$(grep "^description:" "$skill_md" | head -1 | sed 's/description:[[:space:]]*//' | cut -c1-60)
        
        if [ "$OUTPUT_FORMAT" = "--json" ]; then
            echo "    {\"name\": \"$skill_name\", \"lines\": $lines, \"path\": \"$rel_path\", \"description\": \"$desc\"},"
        else
            echo "| $skill_name | $lines | $desc |"
        fi
    done
}

if [ "$OUTPUT_FORMAT" = "--json" ]; then
    cat << EOF
{
  "generated": "$(date -u +%Y-%m-%dT%H:%M:%SZ)",
  "documentation": {
    "file_count": $DOC_COUNT
  },
  "skills": {
    "total": $SKILL_COUNT,
    "working": $WORKING_COUNT,
    "placeholder": $PLACEHOLDER_COUNT,
    "core": $CORE_COUNT,
    "extended": $EXTENDED_COUNT
  },
  "skill_details": [
$(generate_skill_details | sed '$ s/,$//')
  ]
}
EOF
else
    echo "# Repository Statistics"
    echo ""
    echo "Generated: $(date -u +%Y-%m-%d)"
    echo ""
    echo "## Counts"
    echo ""
    echo "- Documentation files: $DOC_COUNT"
    echo "- Total skills: $SKILL_COUNT"
    echo "- Working skills: $WORKING_COUNT"
    echo "- Placeholder skills: $PLACEHOLDER_COUNT"
    echo ""
    echo "## Skills"
    echo ""
    echo "| Name | Lines | Description |"
    echo "|------|-------|-------------|"
    generate_skill_details
fi
```

### Usage

```bash
# JSON output (for automation)
./generate-stats.sh --json > ../../.repo-metadata.json

# Markdown output (for documentation)
./generate-stats.sh --markdown
```

---

## R4: CHANGELOG.md

### Goal
Track what changed between updates.

### File Location
`/CHANGELOG.md` (repository root)

### Initial Content

```markdown
# Changelog

All notable changes to this repository will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]

### Added
### Changed
### Deprecated
### Removed
### Fixed

## [1.0.0] - 2026-01-07

### Added
- Initial documentation mirror from code.claude.com (49 files)
- Core skills: 28 production-ready skills
  - obra-workflow (6 skills)
  - obra-development (9 skills)
  - git-workflow (6 skills, 1 placeholder)
  - testing (2 skills, 1 placeholder)
  - document-creation (5 skills)
  - skill-authoring (2 skills)
- Extended skills: 4 AWS skills from zxkane
- Deployment and validation tools
- Reference links to community skills and learning resources

### Sources
- Documentation: https://code.claude.com/docs (as of 2026-01-06)
- obra skills: https://github.com/obra/superpowers
- anthropics skills: https://github.com/anthropics/skills
- fvadicamo skills: https://github.com/fvadicamo/dev-agent-skills
- zxkane skills: https://github.com/zxkane/aws-skills
```

### Update Process

When making changes:
1. Add entry under `[Unreleased]`
2. When releasing, move unreleased items to new version section
3. Date the version

---

## R5: Freshness Report Tool

### Goal
Detect which skills have upstream changes.

### File Location
`11-external-resources/tools/freshness-report.sh`

### Prerequisites
- R1 must be implemented (commit SHA in .source files)
- GitHub API access (rate limited without auth)

### Script

```bash
#!/bin/bash
# Check for upstream updates to fetched skills
# Usage: ./freshness-report.sh [--verbose]

set -e

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
RESOURCES_DIR="$(dirname "$SCRIPT_DIR")"
VERBOSE="${1:---quiet}"

echo "Freshness Report - $(date -u +%Y-%m-%d)"
echo "======================================"
echo ""

UP_TO_DATE=0
STALE=0
UNKNOWN=0

for source_file in $(find "$RESOURCES_DIR/core-skills" "$RESOURCES_DIR/extended-skills" -name ".source" 2>/dev/null | sort); do
    skill_dir=$(dirname "$source_file")
    skill_name=$(basename "$skill_dir")
    
    SOURCE_URL=$(grep "^source:" "$source_file" | cut -d' ' -f2-)
    LOCAL_SHA=$(grep "^commit_sha:" "$source_file" | cut -d' ' -f2-)
    FETCHED=$(grep "^fetched:" "$source_file" | cut -d' ' -f2-)
    
    if [ -z "$SOURCE_URL" ] || [ "$LOCAL_SHA" = "unknown" ] || [ -z "$LOCAL_SHA" ]; then
        echo "❓ $skill_name - cannot check (no commit SHA)"
        UNKNOWN=$((UNKNOWN + 1))
        continue
    fi
    
    # Extract repo and path from URL
    if [[ "$SOURCE_URL" == *"/tree/main/"* ]]; then
        REPO_PART=$(echo "$SOURCE_URL" | sed -E 's|https://github.com/([^/]+/[^/]+)/tree/main/(.*)|\1|')
        PATH_PART=$(echo "$SOURCE_URL" | sed -E 's|https://github.com/([^/]+/[^/]+)/tree/main/(.*)|\2|')
    else
        REPO_PART=$(echo "$SOURCE_URL" | sed 's|https://github.com/||')
        PATH_PART=""
    fi
    
    # Query GitHub for latest commit
    if [ -n "$PATH_PART" ]; then
        API_URL="https://api.github.com/repos/$REPO_PART/commits?path=$PATH_PART&per_page=1"
    else
        API_URL="https://api.github.com/repos/$REPO_PART/commits?per_page=1"
    fi
    
    REMOTE_SHA=$(curl -sL "$API_URL" | grep '"sha"' | head -1 | sed 's/.*"sha": "\([^"]*\)".*/\1/')
    
    if [ -z "$REMOTE_SHA" ]; then
        echo "❓ $skill_name - could not fetch remote SHA"
        UNKNOWN=$((UNKNOWN + 1))
        continue
    fi
    
    # Compare
    if [ "${LOCAL_SHA:0:40}" = "${REMOTE_SHA:0:40}" ]; then
        if [ "$VERBOSE" = "--verbose" ]; then
            echo "✅ $skill_name - up to date (${LOCAL_SHA:0:7})"
        fi
        UP_TO_DATE=$((UP_TO_DATE + 1))
    else
        echo "⚠️  $skill_name - STALE"
        echo "    Local:  ${LOCAL_SHA:0:7} (fetched $FETCHED)"
        echo "    Remote: ${REMOTE_SHA:0:7}"
        echo "    URL: $SOURCE_URL"
        STALE=$((STALE + 1))
    fi
    
    # Rate limit protection
    sleep 0.5
done

echo ""
echo "======================================"
echo "Summary: $UP_TO_DATE up-to-date, $STALE stale, $UNKNOWN unknown"

if [ $STALE -gt 0 ]; then
    echo ""
    echo "To update stale skills, run:"
    echo "  ./update-sources.sh --fetch"
fi
```

### Output Example

```
Freshness Report - 2026-01-07
======================================

⚠️  writing-plans - STALE
    Local:  a1b2c3d (fetched 2026-01-01T00:00:00Z)
    Remote: f5e6d7c
    URL: https://github.com/obra/superpowers/tree/main/skills/writing-plans
❓ changelog-generator - cannot check (no commit SHA)

======================================
Summary: 30 up-to-date, 1 stale, 2 unknown

To update stale skills, run:
  ./update-sources.sh --fetch
```

---

## R6: Catalog Auto-Generation

### Goal
Generate CATALOG.md from actual skill files.

### File Location
`11-external-resources/tools/regenerate-catalog.sh`

### Script

```bash
#!/bin/bash
# Regenerate CATALOG.md from actual skills
# Usage: ./regenerate-catalog.sh > ../CATALOG.md

set -e

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
RESOURCES_DIR="$(dirname "$SCRIPT_DIR")"

cat << 'HEADER'
# Skills Catalog

Complete indexed catalog of all available skills.

**Auto-generated** - Do not edit manually. Run `tools/regenerate-catalog.sh` to update.

HEADER

echo "**Last generated**: $(date -u +%Y-%m-%dT%H:%M:%SZ)"
echo ""

# Count skills
TOTAL=$(find "$RESOURCES_DIR/core-skills" "$RESOURCES_DIR/extended-skills" -name "SKILL.md" | wc -l | tr -d ' ')
echo "**Total skills**: $TOTAL"
echo ""
echo "---"
echo ""

# Generate sections for each category
generate_section() {
    local category_path="$1"
    local category_name="$2"
    local source_url="$3"
    
    if [ ! -d "$category_path" ]; then
        return
    fi
    
    echo "## $category_name"
    echo ""
    if [ -n "$source_url" ]; then
        echo "Source: [$source_url]($source_url)"
        echo ""
    fi
    echo "| Skill | Lines | Description | Status |"
    echo "|-------|-------|-------------|--------|"
    
    for skill_md in $(find "$category_path" -maxdepth 2 -name "SKILL.md" | sort); do
        skill_dir=$(dirname "$skill_md")
        skill_name=$(basename "$skill_dir")
        lines=$(wc -l < "$skill_md" | tr -d ' ')
        
        # Check if placeholder
        if grep -q "This skill could not be fetched" "$skill_md" 2>/dev/null; then
            status="Placeholder"
        else
            status="Available"
        fi
        
        # Extract description
        desc=$(grep "^description:" "$skill_md" | head -1 | sed 's/description:[[:space:]]*//' | sed "s/'//g" | cut -c1-50)
        
        echo "| $skill_name | $lines | $desc | $status |"
    done
    
    echo ""
}

# Core skills sections
generate_section "$RESOURCES_DIR/core-skills/obra-workflow" "Workflow Skills (obra-workflow/)" "https://github.com/obra/superpowers"
generate_section "$RESOURCES_DIR/core-skills/obra-development" "Development Skills (obra-development/)" "https://github.com/obra/superpowers"
generate_section "$RESOURCES_DIR/core-skills/git-workflow" "Git Workflow Skills (git-workflow/)" ""
generate_section "$RESOURCES_DIR/core-skills/testing" "Testing Skills (testing/)" ""
generate_section "$RESOURCES_DIR/core-skills/document-creation" "Document Creation Skills (document-creation/)" "https://github.com/anthropics/skills"
generate_section "$RESOURCES_DIR/core-skills/skill-authoring" "Skill Authoring (skill-authoring/)" "https://github.com/anthropics/skills"

echo "---"
echo ""
echo "## Extended Skills"
echo ""

generate_section "$RESOURCES_DIR/extended-skills/aws-skills" "AWS Skills (aws-skills/)" "https://github.com/zxkane/aws-skills"

echo "---"
echo ""
echo "## Quick Deploy Commands"
echo ""
cat << 'FOOTER'
```bash
# Deploy a single skill
./tools/deploy-skill.sh core-skills/obra-workflow/brainstorming

# Deploy all core skills
./tools/deploy-all.sh

# Validate before deploying
./tools/validate-skill.sh core-skills/obra-workflow/brainstorming
```
FOOTER
```

### Usage

```bash
cd 11-external-resources/tools
./regenerate-catalog.sh > ../CATALOG.md
```

---

## R7: Link Health Checker

### Goal
Detect dead links in markdown files.

### File Location
`11-external-resources/tools/check-links.sh`

### Script

```bash
#!/bin/bash
# Check for dead links in markdown files
# Usage: ./check-links.sh [path]

set -e

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
TARGET="${1:-$(dirname "$SCRIPT_DIR")}"

echo "Link Health Check - $(date -u +%Y-%m-%d)"
echo "========================================="
echo "Scanning: $TARGET"
echo ""

ALIVE=0
REDIRECT=0
DEAD=0
SKIPPED=0

# Extract URLs from markdown
extract_urls() {
    grep -ohE 'https?://[^)">[:space:]]+' "$1" 2>/dev/null | sort -u
}

# Check a single URL
check_url() {
    local url="$1"
    local status=$(curl -sL -o /dev/null -w "%{http_code}" --max-time 10 "$url" 2>/dev/null || echo "000")
    echo "$status"
}

for md_file in $(find "$TARGET" -name "*.md" -type f | sort); do
    rel_path="${md_file#$TARGET/}"
    file_has_issues=false
    
    for url in $(extract_urls "$md_file"); do
        # Skip certain URLs
        if [[ "$url" == *"localhost"* ]] || [[ "$url" == *"127.0.0.1"* ]]; then
            SKIPPED=$((SKIPPED + 1))
            continue
        fi
        
        status=$(check_url "$url")
        
        case "$status" in
            200)
                ALIVE=$((ALIVE + 1))
                ;;
            301|302|307|308)
                if [ "$file_has_issues" = false ]; then
                    echo "📄 $rel_path"
                    file_has_issues=true
                fi
                echo "  ↪️  REDIRECT ($status): $url"
                REDIRECT=$((REDIRECT + 1))
                ;;
            000|404|410|500|502|503)
                if [ "$file_has_issues" = false ]; then
                    echo "📄 $rel_path"
                    file_has_issues=true
                fi
                echo "  ❌ DEAD ($status): $url"
                DEAD=$((DEAD + 1))
                ;;
            *)
                ALIVE=$((ALIVE + 1))  # Assume alive for unusual codes
                ;;
        esac
        
        sleep 0.2  # Rate limiting
    done
done

echo ""
echo "========================================="
echo "Summary: $ALIVE alive, $REDIRECT redirects, $DEAD dead, $SKIPPED skipped"

if [ $DEAD -gt 0 ]; then
    exit 1
fi
```

---

## R8: CONTRIBUTING.md

### Goal
Provide clear contribution guidelines.

### File Location
`/CONTRIBUTING.md` (repository root)

### Content

```markdown
# Contributing to Claude Code Documentation

Thank you for your interest in contributing! This guide explains how to add skills, update documentation, and report issues.

## Adding New Skills

### From an Existing GitHub Repository

1. Find the skill on GitHub or [awesome-claude-skills](https://github.com/VoltAgent/awesome-claude-skills)

2. Fetch the skill:
   ```bash
   cd 11-external-resources/tools
   ./fetch-skill.sh https://github.com/<author>/<repo>/tree/main/skills/<skill-name> ../core-skills/<category>/<skill-name>
   ```

3. Validate the skill:
   ```bash
   ./validate-skill.sh ../core-skills/<category>/<skill-name>
   ```

4. Update the catalog:
   ```bash
   ./regenerate-catalog.sh > ../CATALOG.md
   ```

5. Test deployment:
   ```bash
   ./deploy-skill.sh ../core-skills/<category>/<skill-name> /tmp/test-skills
   ```

### Creating a New Skill

See `11-external-resources/meta/skill-authoring-guide.md` for complete guidance.

## Updating Existing Skills

1. Run the freshness report:
   ```bash
   cd 11-external-resources/tools
   ./freshness-report.sh
   ```

2. If skills are stale, re-fetch:
   ```bash
   ./update-sources.sh --fetch
   ```

3. Regenerate the catalog:
   ```bash
   ./regenerate-catalog.sh > ../CATALOG.md
   ```

4. Update CHANGELOG.md with what changed.

## Updating Core Documentation

Core documentation (folders `01-10`) mirrors https://code.claude.com/docs.

To update:
1. Compare `llms.txt` with the online version
2. Download changed pages manually
3. Update `.repo-metadata.json` with new sync date
4. Update CHANGELOG.md

## Reporting Issues

### Stale Content
If you notice outdated information:
1. Check if upstream has been updated
2. Open an issue describing what's outdated
3. Include links to current upstream content

### Broken Links
1. Run `./tools/check-links.sh`
2. Report dead links as issues
3. Or submit a PR fixing them

### Missing Skills
1. Check [awesome-claude-skills](https://github.com/VoltAgent/awesome-claude-skills) for the skill
2. Open an issue requesting the skill
3. Or submit a PR adding it

## Pull Request Guidelines

- One skill or logical change per PR
- Run validation before submitting
- Update CATALOG.md if adding skills
- Update CHANGELOG.md with your changes
- Describe what you changed and why

## Questions?

Open an issue with the "question" label.
```

---

## R9: GitHub Actions Workflow

### Goal
Automate validation and freshness checks.

### File Location
`/.github/workflows/maintenance.yml`

### Workflow

```yaml
name: Repository Maintenance

on:
  push:
    branches: [main]
    paths:
      - '11-external-resources/**'
  pull_request:
    branches: [main]
  schedule:
    # Run weekly on Sundays at midnight UTC
    - cron: '0 0 * * 0'
  workflow_dispatch:
    inputs:
      check_freshness:
        description: 'Run freshness check'
        required: false
        default: 'false'

jobs:
  validate-skills:
    name: Validate Skills
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Make scripts executable
        run: chmod +x 11-external-resources/tools/*.sh
      
      - name: Validate all skills
        run: |
          cd 11-external-resources/tools
          FAILED=0
          for skill in $(find ../core-skills ../extended-skills -name "SKILL.md" -exec dirname {} \;); do
            if ! ./validate-skill.sh "$skill"; then
              FAILED=$((FAILED + 1))
            fi
          done
          if [ $FAILED -gt 0 ]; then
            echo "❌ $FAILED skills failed validation"
            exit 1
          fi
          echo "✅ All skills passed validation"

  check-links:
    name: Check Links
    runs-on: ubuntu-latest
    if: github.event_name == 'pull_request' || github.event_name == 'workflow_dispatch'
    steps:
      - uses: actions/checkout@v4
      
      - name: Make scripts executable
        run: chmod +x 11-external-resources/tools/*.sh
      
      - name: Check external links
        run: |
          cd 11-external-resources/tools
          ./check-links.sh ../reference || true  # Don't fail on dead links, just report
        continue-on-error: true

  freshness-report:
    name: Freshness Report
    runs-on: ubuntu-latest
    if: github.event_name == 'schedule' || (github.event_name == 'workflow_dispatch' && github.event.inputs.check_freshness == 'true')
    steps:
      - uses: actions/checkout@v4
      
      - name: Make scripts executable
        run: chmod +x 11-external-resources/tools/*.sh
      
      - name: Generate freshness report
        run: |
          cd 11-external-resources/tools
          ./freshness-report.sh --verbose
      
      - name: Create issue if stale
        uses: actions/github-script@v7
        with:
          script: |
            // Parse output and create issue if stale skills found
            // This is a simplified example
            console.log('Freshness report complete');
```

---

## Implementation Checklist

Use this checklist when implementing:

- [ ] R1: Modify `fetch-skill.sh` to capture commit SHA
- [ ] R1: Run migration script on existing `.source` files
- [ ] R2: Create `.repo-metadata.json`
- [ ] R3: Create `generate-stats.sh`
- [ ] R4: Create `CHANGELOG.md`
- [ ] R5: Create `freshness-report.sh`
- [ ] R6: Create `regenerate-catalog.sh`
- [ ] R7: Create `check-links.sh`
- [ ] R8: Create `CONTRIBUTING.md`
- [ ] R9: Create `.github/workflows/maintenance.yml`
- [ ] Update CLAUDE.md with new tools
- [ ] Update README.md to reference new files

