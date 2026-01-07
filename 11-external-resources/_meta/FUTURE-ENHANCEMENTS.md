# Future Enhancements

**Created**: 2026-01-06
**Purpose**: Documented optimization opportunities with implementation details for future work.

---

## Overview

These enhancements were identified during the initial implementation session. They're ordered by impact/effort ratio, with full implementation details provided for each.

---

## Priority 1: URL Verification Before Bulk Operations

### Problem
The initial implementation trusted the awesome-claude-skills list, leading to multiple failed fetch attempts for non-existent repos. A 2-minute verification step would have saved 10+ minutes.

### Solution
Create a command that verifies GitHub URLs before bulk operations.

### Implementation

**File**: `~/.claude/commands/verify-github-urls.md`

```markdown
---
description: Verify a list of GitHub URLs exist before bulk operations
---
I have a list of GitHub URLs to verify. For each URL, I need to check if the repository and specific path exists.

URLs to verify:
$ARGUMENTS

For each URL:
1. Parse the URL to extract owner, repo, and path
2. Construct the raw.githubusercontent.com URL
3. Make a HEAD request (or curl with -I flag)
4. Record the HTTP status code

Output format:
| URL | Status | Notes |
|-----|--------|-------|
| github.com/owner/repo/path | 200 OK | Exists |
| github.com/owner/repo2/path | 404 | Not found |

Summary:
- Total checked: X
- Existing: Y
- Missing: Z

If any URLs return 404, ask: "Some URLs don't exist. Should I proceed with only the valid ones, or would you like to investigate the missing ones first?"
```

### Usage
```
/verify-github-urls https://github.com/obra/brainstorming https://github.com/obra/superpowers/tree/main/skills/brainstorming
```

---

## Priority 2: Batch Fetch with Source List

### Problem
Skills were fetched one at a time with repetitive commands. A batch approach would be more efficient.

### Solution
Create a command that takes a structured source list and fetches all skills.

### Implementation

**File**: `~/.claude/commands/batch-fetch-skills.md`

```markdown
---
description: Fetch multiple skills from a structured source specification
---
Fetch skills from the following source list. Each entry should specify:
- Source repo URL
- Skill name (path within repo)
- Local destination path

Source specification:
$ARGUMENTS

Expected format (one per line):
```
<repo-url> <skill-path> <local-dest>
```

Example:
```
https://github.com/obra/superpowers skills/brainstorming core-skills/obra-workflow/brainstorming
https://github.com/anthropics/skills skills/docx core-skills/document-creation/docx
```

For each entry:
1. Verify the source exists (warn if 404, continue with others)
2. Create the local destination directory
3. Fetch SKILL.md to the destination
4. Create .source tracking file with:
   - source: original URL
   - fetched: ISO timestamp
5. Report success/failure

After all fetches, provide summary:
| Skill | Status | Lines |
|-------|--------|-------|
| brainstorming | OK | 54 |
| missing-skill | SKIP (404) | - |

Total: X fetched, Y skipped
```

---

## Priority 3: Smart Update Checker

### Problem
The current `update-sources.sh` is basic - it only lists sources. It doesn't actually check if upstream has changed.

### Solution
Enhance to compare local vs. remote content.

### Implementation

**File**: `11-external-resources/tools/update-sources.sh` (enhanced version)

```bash
#!/bin/bash
# Enhanced update checker with content comparison
# Usage: ./update-sources.sh [--check | --fetch | --report]

set -e

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
RESOURCES_DIR="$(dirname "$SCRIPT_DIR")"
MODE="${1:---check}"

check_skill_update() {
    local skill_dir="$1"
    local skill_name=$(basename "$skill_dir")
    local source_file="$skill_dir/.source"

    if [ ! -f "$source_file" ]; then
        echo "UNKNOWN: $skill_name (no .source file)"
        return
    fi

    local source_url=$(grep "^source:" "$source_file" | sed 's/source:[[:space:]]*//')
    local fetched_date=$(grep "^fetched:" "$source_file" | sed 's/fetched:[[:space:]]*//')

    # Convert GitHub URL to raw URL
    local raw_url=$(echo "$source_url" | sed 's|github.com|raw.githubusercontent.com|' | sed 's|/tree/main/|/main/|')
    raw_url="$raw_url/SKILL.md"

    # Get remote content hash
    local remote_hash=$(curl -sL "$raw_url" 2>/dev/null | md5sum | cut -d' ' -f1)
    local local_hash=$(md5sum "$skill_dir/SKILL.md" 2>/dev/null | cut -d' ' -f1)

    if [ "$remote_hash" = "$local_hash" ]; then
        echo "CURRENT: $skill_name (fetched: $fetched_date)"
    elif [ -z "$remote_hash" ] || [ "$remote_hash" = "d41d8cd98f00b204e9800998ecf8427e" ]; then
        echo "ERROR: $skill_name (could not fetch remote)"
    else
        echo "OUTDATED: $skill_name (local differs from remote)"
    fi
}

case "$MODE" in
    --check)
        echo "Checking for updates..."
        for source_file in $(find "$RESOURCES_DIR/core-skills" "$RESOURCES_DIR/extended-skills" -name ".source" 2>/dev/null); do
            check_skill_update "$(dirname "$source_file")"
        done
        ;;
    --fetch)
        echo "Re-fetching all skills..."
        # Implementation: iterate and re-fetch outdated skills
        ;;
    --report)
        echo "Generating update report..."
        # Implementation: output structured report
        ;;
    *)
        echo "Usage: $0 [--check | --fetch | --report]"
        ;;
esac
```

---

## Priority 4: README Template System

### Problem
Created 10+ similar README files manually. Repetitive and error-prone.

### Solution
Create a template system for generating category READMEs.

### Implementation

**File**: `11-external-resources/tools/templates/category-readme.md`

```markdown
# {{CATEGORY_NAME}}

{{DESCRIPTION}}

## Source

{{SOURCE_URLS}}

## Skills

| Skill | Description |
|-------|-------------|
{{SKILLS_TABLE}}

{{OPTIONAL_SECTIONS}}

## Related Skills

{{RELATED_LINKS}}
```

**File**: `11-external-resources/tools/generate-readme.sh`

```bash
#!/bin/bash
# Generate README from template
# Usage: ./generate-readme.sh <category-dir> [--force]

CATEGORY_DIR="$1"
FORCE="${2:---no-force}"
TEMPLATE="$(dirname "$0")/templates/category-readme.md"

if [ ! -d "$CATEGORY_DIR" ]; then
    echo "Error: Directory not found: $CATEGORY_DIR"
    exit 1
fi

if [ -f "$CATEGORY_DIR/README.md" ] && [ "$FORCE" != "--force" ]; then
    echo "README.md exists. Use --force to overwrite."
    exit 1
fi

CATEGORY_NAME=$(basename "$CATEGORY_DIR" | tr '-' ' ' | sed 's/\b\(.\)/\u\1/g')

# Build skills table
SKILLS_TABLE=""
for skill_dir in "$CATEGORY_DIR"/*/; do
    if [ -f "$skill_dir/SKILL.md" ]; then
        skill_name=$(basename "$skill_dir")
        # Extract description from frontmatter
        desc=$(grep "^description:" "$skill_dir/SKILL.md" | head -1 | sed 's/description:[[:space:]]*//' | cut -c1-60)
        SKILLS_TABLE="$SKILLS_TABLE| $skill_name | $desc |\n"
    fi
done

# Generate README
sed -e "s/{{CATEGORY_NAME}}/$CATEGORY_NAME/" \
    -e "s/{{SKILLS_TABLE}}/$SKILLS_TABLE/" \
    "$TEMPLATE" > "$CATEGORY_DIR/README.md"

echo "Generated: $CATEGORY_DIR/README.md"
```

---

## Priority 5: Parallel Fetch Support

### Problem
Skills fetched sequentially with `sleep 0.5` delays. Slow for large batches.

### Solution
Add parallel fetching with controlled concurrency.

### Implementation

**File**: `11-external-resources/tools/parallel-fetch.sh`

```bash
#!/bin/bash
# Parallel skill fetcher with rate limiting
# Usage: ./parallel-fetch.sh <source-list-file> [concurrency]

SOURCE_LIST="$1"
CONCURRENCY="${2:-3}"

if [ ! -f "$SOURCE_LIST" ]; then
    echo "Usage: $0 <source-list-file> [concurrency]"
    echo "Source list format: <github-url> <local-path>"
    exit 1
fi

# Use GNU parallel if available, otherwise fall back to xargs
if command -v parallel &> /dev/null; then
    cat "$SOURCE_LIST" | parallel -j "$CONCURRENCY" --colsep ' ' \
        "./fetch-skill.sh {1} {2}"
else
    # Fallback: simple backgrounding with wait
    count=0
    while read -r url path; do
        ./fetch-skill.sh "$url" "$path" &
        ((count++))
        if [ $((count % CONCURRENCY)) -eq 0 ]; then
            wait
            sleep 1  # Rate limit between batches
        fi
    done < "$SOURCE_LIST"
    wait
fi

echo "Parallel fetch complete"
```

---

## Priority 6: Skill Invocation Testing

### Problem
Current validation only checks SKILL.md format, not whether the skill actually works when invoked.

### Solution
Create a testing framework that invokes skills with sample inputs.

### Implementation

This is more complex and would require:
1. A test case format for each skill
2. A test runner that invokes Claude Code with the skill
3. Output validation

**Recommended approach**: Start with a simple smoke test that just verifies skills are discoverable:

**File**: `11-external-resources/tools/smoke-test.sh`

```bash
#!/bin/bash
# Basic smoke test: verify skills can be listed
# Usage: ./smoke-test.sh

echo "Smoke test: Checking skill discovery..."

# Deploy to temp location
TEMP_SKILLS="/tmp/claude-skills-smoke-$$"
./deploy-all.sh "$TEMP_SKILLS"

# Count deployed skills
DEPLOYED=$(find "$TEMP_SKILLS" -name "SKILL.md" | wc -l)

echo "Deployed $DEPLOYED skills to $TEMP_SKILLS"

# Check for basic validity
VALID=0
INVALID=0
for skill_dir in "$TEMP_SKILLS"/*/; do
    if ./validate-skill.sh "$skill_dir" > /dev/null 2>&1; then
        ((VALID++))
    else
        ((INVALID++))
        echo "INVALID: $(basename "$skill_dir")"
    fi
done

echo "Valid: $VALID, Invalid: $INVALID"

# Cleanup
rm -rf "$TEMP_SKILLS"

if [ $INVALID -eq 0 ]; then
    echo "SMOKE TEST PASSED"
    exit 0
else
    echo "SMOKE TEST FAILED"
    exit 1
fi
```

---

## Priority 7: Automatic Registry Updates

### Problem
The SKILL-SOURCES-REGISTRY.md needs manual updates when repos change.

### Solution
Create a script that regenerates the registry from actual fetches.

### Implementation

**File**: `11-external-resources/tools/regenerate-registry.sh`

```bash
#!/bin/bash
# Regenerate SKILL-SOURCES-REGISTRY.md from .source files
# Usage: ./regenerate-registry.sh

SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
RESOURCES_DIR="$(dirname "$SCRIPT_DIR")"
REGISTRY="$RESOURCES_DIR/_meta/SKILL-SOURCES-REGISTRY.md"

echo "# Skill Sources Registry" > "$REGISTRY"
echo "" >> "$REGISTRY"
echo "**Auto-generated**: $(date -u +%Y-%m-%dT%H:%M:%SZ)" >> "$REGISTRY"
echo "" >> "$REGISTRY"

# Group by source repo
declare -A repos
for source_file in $(find "$RESOURCES_DIR" -name ".source" 2>/dev/null); do
    skill_dir=$(dirname "$source_file")
    skill_name=$(basename "$skill_dir")
    source_url=$(grep "^source:" "$source_file" | sed 's/source:[[:space:]]*//')

    # Extract repo from URL
    repo=$(echo "$source_url" | sed -E 's|https://github.com/([^/]+/[^/]+).*|\1|')

    repos["$repo"]+="$skill_name|$source_url\n"
done

# Output each repo section
for repo in "${!repos[@]}"; do
    echo "## $repo" >> "$REGISTRY"
    echo "" >> "$REGISTRY"
    echo "| Skill | Path |" >> "$REGISTRY"
    echo "|-------|------|" >> "$REGISTRY"
    echo -e "${repos[$repo]}" | while IFS='|' read -r name url; do
        [ -n "$name" ] && echo "| $name | $url |" >> "$REGISTRY"
    done
    echo "" >> "$REGISTRY"
done

echo "Registry regenerated: $REGISTRY"
```

---

## Implementation Roadmap

### Immediate (Next Session)
1. [ ] Create `/verify-github-urls` command
2. [ ] Update SKILL-SOURCES-REGISTRY.md with any new discoveries

### Short-term (Within Week)
3. [ ] Create `/batch-fetch-skills` command
4. [ ] Enhance `update-sources.sh` with content comparison
5. [ ] Create README template system

### Medium-term (When Needed)
6. [ ] Add parallel fetch support
7. [ ] Implement smoke test framework
8. [ ] Create registry auto-regeneration

---

## Notes for Implementers

1. **Test locally first** - Before deploying commands/skills, test in isolation
2. **Preserve existing work** - Don't overwrite working skills
3. **Update documentation** - After implementing, update CATALOG.md and READMEs
4. **Check rate limits** - GitHub API has limits; batch operations need delays
5. **Graceful degradation** - Handle 404s and errors without stopping entirely
