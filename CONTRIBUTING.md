# Contributing to Claude Code Documentation

Thank you for your interest in contributing! This guide explains how to add skills, update documentation, and report issues.

## Quick Links

- [Adding New Skills](#adding-new-skills)
- [Updating Existing Skills](#updating-existing-skills)
- [Syncing Core Documentation](#syncing-core-documentation)
- [Reporting Issues](#reporting-issues)
- [Pull Request Guidelines](#pull-request-guidelines)

---

## Adding New Skills

### From an Existing GitHub Repository

1. **Find the skill** on GitHub or in the [awesome-claude-skills](https://github.com/VoltAgent/awesome-claude-skills) list

2. **Verify the URL exists** before fetching:
   ```bash
   curl -sL -o /dev/null -w "%{http_code}" \
     "https://raw.githubusercontent.com/<owner>/<repo>/main/skills/<skill-name>/SKILL.md"
   ```

3. **Fetch the skill**:
   ```bash
   cd 11-external-resources/tools
   ./fetch-skill.sh https://github.com/<owner>/<repo>/tree/main/skills/<skill-name> \
     ../core-skills/<category>/<skill-name>
   ```

4. **Validate the skill**:
   ```bash
   ./validate-skill.sh ../core-skills/<category>/<skill-name>
   ```

5. **Test deployment**:
   ```bash
   ./deploy-skill.sh ../core-skills/<category>/<skill-name> /tmp/test-skills
   ```

6. **Regenerate the catalog**:
   ```bash
   ./regenerate-catalog.sh --output ../CATALOG.md
   ```

7. **Update CHANGELOG.md** with your addition

### Creating a New Skill

See the skill authoring documentation:
- `11-external-resources/meta/skill-authoring-guide.md` - Step-by-step creation guide
- `11-external-resources/meta/skill-anatomy.md` - Deep dive into SKILL.md structure
- `11-external-resources/core-skills/skill-authoring/` - Official templates

---

## Updating Existing Skills

### Check for Upstream Changes

Run the freshness report to see which skills have been updated upstream:

```bash
cd 11-external-resources/tools
./freshness-report.sh
```

This compares the commit SHA stored in each `.source` file with the current upstream commit.

### Re-fetch Updated Skills

Option A - Re-fetch all skills:
```bash
./update-sources.sh --fetch
```

Option B - Re-fetch a single skill:
```bash
./fetch-skill.sh <github-url> <local-path>
```

### After Updating

1. **Regenerate the catalog**:
   ```bash
   ./regenerate-catalog.sh --output ../CATALOG.md
   ```

2. **Update stats**:
   ```bash
   ./generate-stats.sh --update-metadata
   ```

3. **Update CHANGELOG.md** with what changed

---

## Syncing Core Documentation

Core documentation (folders `01-10`) mirrors https://code.claude.com/docs.

### Manual Sync Process

1. **Compare indexes**:
   - Local: `llms.txt`
   - Online: https://code.claude.com/docs/llms.txt

2. **Identify changes**:
   - New pages
   - Removed pages
   - Updated pages (check content differences)

3. **Download updates** manually from the source

4. **Update metadata**:
   - Update `.repo-metadata.json` with new `last_synced` date
   - Update CHANGELOG.md with what changed

### Notes

- Anthropic may not provide raw access to docs
- Some pages use custom components (`<Tabs>`, `<Card>`, etc.)
- Preserve the directory structure matching official docs

---

## Reporting Issues

### Stale Content

If you notice outdated information:

1. Run `./freshness-report.sh` to check upstream status
2. Open an issue describing what's outdated
3. Include links to current upstream content

### Broken Links

1. Run `./check-links.sh` (if available) or check manually
2. Report dead links as issues
3. Or submit a PR fixing them

### Missing Skills

1. Check [awesome-claude-skills](https://github.com/VoltAgent/awesome-claude-skills) for the skill
2. Open an issue requesting the skill
3. Or submit a PR adding it using the process above

### Placeholder Skills

Two skills are currently placeholders (source not accessible):
- `changelog-generator` (ComposioHQ)
- `pypict-skill` (omkamal)

If you can access these, please contribute them!

---

## Pull Request Guidelines

### General

- One skill or logical change per PR
- Run validation before submitting
- Test that deployment works

### Required Updates

For skill additions:
- [ ] Skill passes `./validate-skill.sh`
- [ ] CATALOG.md updated (run `./regenerate-catalog.sh --output CATALOG.md`)
- [ ] CHANGELOG.md updated with addition

For documentation changes:
- [ ] No broken internal links
- [ ] Consistent markdown formatting
- [ ] CHANGELOG.md updated if significant

### Commit Messages

Use clear, descriptive commit messages:
- `Add: new-skill-name from author/repo`
- `Update: skill-name to latest upstream`
- `Fix: broken link in community-catalog.md`
- `Docs: update quickstart with new workflow`

---

## Questions?

Open an issue with the "question" label.

---

## Tool Reference

| Tool | Purpose |
|------|---------|
| `fetch-skill.sh` | Download skill from GitHub |
| `validate-skill.sh` | Validate SKILL.md format |
| `deploy-skill.sh` | Deploy to Claude Code |
| `deploy-all.sh` | Bulk deployment |
| `update-sources.sh` | Re-fetch all skills |
| `freshness-report.sh` | Check for upstream updates |
| `regenerate-catalog.sh` | Auto-generate CATALOG.md |
| `generate-stats.sh` | Generate repository statistics |
| `migrate-source-files.sh` | Add commit SHA to .source files |
