# Changelog

All notable changes to this repository will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]

_No unreleased changes._

## [1.1.1] - 2026-01-07

### Added
- `fix-unknown-shas.sh` - Fix skills with "unknown" commit SHAs (for rate-limit recovery)

### Fixed
- All 32 working skills now have valid commit SHAs (was 19 due to rate limiting)

## [1.1.0] - 2026-01-07

### Added
- CHANGELOG.md for tracking repository changes
- `.repo-metadata.json` for centralized sync state tracking
- Maintenance tooling:
  - `generate-stats.sh` - Auto-generate repository statistics
  - `freshness-report.sh` - Detect stale skills with upstream changes
  - `regenerate-catalog.sh` - Auto-generate CATALOG.md
  - `migrate-source-files.sh` - One-time migration for commit SHA tracking
- Enhanced `fetch-skill.sh` with commit SHA capture for change detection
- CONTRIBUTING.md with contribution guidelines
- Comprehensive maintenance documentation in `_repo-maintenance/`

### Changed
- `.source` files now include `commit_sha` and `branch` fields
- CLAUDE.md updated with new tool references and workflows

## [1.0.0] - 2026-01-06

### Added
- Initial documentation mirror from https://code.claude.com/docs (49 files)
- Directory structure matching official docs organization:
  - `01-getting-started/` - 4 files
  - `02-core-features/` - 7 files
  - `03-ide-integration/` - 4 files
  - `04-cloud-providers/` - 4 files
  - `05-enterprise/` - 6 files
  - `06-cicd-automation/` - 4 files
  - `07-plugins/` - 4 files
  - `08-configuration/` - 6 files
  - `09-reference/` - 6 files
  - `10-other/` - 4 files
- External skills library with 32 production-ready skills:
  - `obra-workflow/` - 6 skills from Jesse Vincent (obra/superpowers)
  - `obra-development/` - 9 skills from obra/superpowers
  - `git-workflow/` - 6 skills (5 from fvadicamo, 1 placeholder)
  - `testing/` - 2 skills (1 working, 1 placeholder)
  - `document-creation/` - 5 skills from Anthropic
  - `skill-authoring/` - 2 skills (templates)
  - `aws-skills/` - 4 skills from zxkane
- Deployment and validation tools:
  - `fetch-skill.sh` - Download skills from GitHub
  - `validate-skill.sh` - Validate SKILL.md format
  - `deploy-skill.sh` - Deploy to Claude Code
  - `deploy-all.sh` - Bulk deployment
  - `update-sources.sh` - Re-fetch all skills
- Reference links to community skills and learning resources
- Skill authoring documentation and templates
- CLAUDE.md for AI agent guidance
- README.md with complete navigation

### Sources
- Documentation: https://code.claude.com/docs (synced 2026-01-06)
- Skills:
  - https://github.com/obra/superpowers
  - https://github.com/obra/superpowers-lab
  - https://github.com/anthropics/skills
  - https://github.com/fvadicamo/dev-agent-skills
  - https://github.com/zxkane/aws-skills

### Known Issues
- 2 placeholder skills (repos not accessible at fetch time):
  - `changelog-generator` (ComposioHQ)
  - `pypict-skill` (omkamal)
