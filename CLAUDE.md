# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a local copy of the official Claude Code documentation from https://code.claude.com/docs, organized for offline reference and LLM consumption. It contains no executable code.

## Documentation Structure

The repository uses numbered directories for topic organization:

- `01-getting-started/` - Installation, setup, interactive mode basics
- `02-core-features/` - Memory, skills, subagents, hooks, MCP integration
- `03-ide-integration/` - VS Code, JetBrains, desktop app, Chrome extension
- `04-cloud-providers/` - Amazon Bedrock, Google Vertex AI, Microsoft Foundry, LLM gateways
- `05-enterprise/` - IAM, network config, security, sandboxing, monitoring
- `06-cicd-automation/` - GitHub Actions, GitLab CI/CD, headless/programmatic usage
- `07-plugins/` - Plugin creation, reference, discovery, marketplaces
- `08-configuration/` - Settings, model config, terminal, statusline, output styles
- `09-reference/` - CLI reference, troubleshooting, costs, data usage, analytics
- `10-other/` - Slack integration, changelog, legal, common workflows

## Key Files

- `llms.txt` - Official documentation index with URLs to online docs
- `README.md` - Complete file listing with descriptions for each doc

## External Resources (11-external-resources/)

Curated external skills and resources for "meta" skill development:

- `core-skills/` - 28 production-ready skills from obra, fvadicamo, and anthropics
- `extended-skills/` - 4 AWS skills + context engineering reference links
- `reference/` - Curated links to community skills and learning resources
- `tools/` - Deployment and validation scripts
- `meta/` - Skill authoring documentation

**Quick deploy all skills:**
```bash
cd 11-external-resources/tools/
./deploy-all.sh
```

**Browse available skills:** See `11-external-resources/CATALOG.md`

## Working with This Repository

When answering questions about Claude Code features, consult the relevant markdown files directly. The documentation is authoritative and up-to-date as of the "Last Updated" date in README.md.

For skill development, reference:
- `11-external-resources/core-skills/skill-authoring/` - Official templates
- `11-external-resources/meta/` - Skill authoring guides
