# Patterns and Anti-Patterns

Common patterns to follow and anti-patterns to avoid when creating Claude Code skills.

## Good Patterns

### 1. Clear Trigger Descriptions

**Pattern:** Write descriptions that clearly indicate when the skill should activate.

```yaml
# Good
description: >-
  Creates pull requests on GitHub with proper formatting.
  Use when opening PRs for code review.

# Better
description: >-
  Creates GitHub pull requests following team conventions.
  Use when ready to merge a feature branch. Includes title formatting,
  description template, reviewer assignment, and label suggestions.
```

### 2. Structured Workflows

**Pattern:** Break complex tasks into numbered steps.

```markdown
# Core Workflow

1. **Validate prerequisites**
   Check that all required tools are available.

2. **Gather input**
   Collect necessary information from the user.

3. **Process**
   Execute the main task logic.

4. **Verify output**
   Confirm the result is correct.

5. **Report**
   Summarize what was done.
```

### 3. Progressive Disclosure

**Pattern:** Keep SKILL.md concise, put details in references/.

```markdown
# In SKILL.md (brief)
For advanced configuration, see [references/advanced.md](references/advanced.md).

# In references/advanced.md (detailed)
## Advanced Configuration
...detailed documentation...
```

### 4. Explicit Constraints

**Pattern:** Use **ALWAYS**/**NEVER** for critical rules.

```markdown
# Important Rules

- **ALWAYS** validate user input before processing
- **NEVER** store credentials in plain text
- **ALWAYS** provide rollback instructions for destructive operations
```

### 5. Concrete Examples

**Pattern:** Show real examples, not abstract descriptions.

```markdown
## Example

**Input:**
```
Create a commit for the login bug fix
```

**Output:**
```
fix(auth): resolve null pointer in login handler

- Add null check for user object
- Update error message for clarity
```
```

### 6. Error Handling Guidance

**Pattern:** Document what to do when things go wrong.

```markdown
## Troubleshooting

### Error: "No staged changes"
**Cause:** Nothing is staged for commit.
**Fix:** Run `git add <files>` before committing.

### Error: "Branch protection"
**Cause:** Direct commits to main are blocked.
**Fix:** Create a feature branch first.
```

---

## Anti-Patterns

### 1. Vague Descriptions

**Anti-pattern:** Generic descriptions that don't help Claude decide when to use the skill.

```yaml
# Bad
description: Handles code stuff

# Bad
description: A useful development tool

# Bad
description: Does things with Git
```

**Fix:** Be specific about what, when, and why.

### 2. Wall of Text

**Anti-pattern:** Long, unstructured paragraphs.

```markdown
# Bad
This skill helps you with commits. First you need to stage your changes
using git add and then you can run the skill which will analyze your
changes and determine what type of commit it should be based on the
conventional commits specification which includes types like feat for
features and fix for bug fixes and docs for documentation changes...
```

**Fix:** Use headings, lists, and code blocks.

### 3. Missing Context

**Anti-pattern:** Assuming knowledge without stating prerequisites.

```markdown
# Bad - assumes user knows the setup
Run `./deploy.sh` to deploy.

# Good - states prerequisites
## Prerequisites
- Docker installed and running
- AWS CLI configured with valid credentials

## Deploy
Run `./deploy.sh` to deploy.
```

### 4. Over-Engineering

**Anti-pattern:** Adding features that aren't needed.

```markdown
# Bad - too many options upfront
This skill supports:
- 47 different commit formats
- 12 branch naming conventions
- Integration with 8 CI systems
- Custom hooks for...
```

**Fix:** Start simple, add complexity only when needed.

### 5. Duplicate Information

**Anti-pattern:** Repeating the same information in multiple places.

```markdown
# Bad
## Overview
Creates Git commits with proper formatting.

## Description
This skill creates Git commits with proper formatting.

## What It Does
It creates Git commits with proper formatting.
```

**Fix:** Say it once, in the right place.

### 6. Hardcoded Values

**Anti-pattern:** Embedding environment-specific values.

```markdown
# Bad
The API endpoint is https://api.mycompany.com/v1

# Good
Configure the API endpoint via `API_ENDPOINT` environment variable.
```

### 7. Missing Validation

**Anti-pattern:** Not checking inputs before processing.

```markdown
# Bad - assumes input is valid
1. Take the file path
2. Process the file

# Good - validates first
1. Verify the file exists and is readable
2. Check the file is the expected format
3. Process the file
```

### 8. No Success Criteria

**Anti-pattern:** Not defining what success looks like.

```markdown
# Bad
Run the process and check if it worked.

# Good
## Verification
After completion:
- [ ] Output file exists at `./output/result.json`
- [ ] File contains valid JSON
- [ ] All required fields are present
```

---

## Checklist

Before deploying a skill, verify:

- [ ] Description clearly states what and when
- [ ] Quick Start gets users productive in < 30 seconds
- [ ] Core Workflow has 3-7 numbered steps
- [ ] Important Rules use ALWAYS/NEVER
- [ ] Examples show real inputs and outputs
- [ ] Total SKILL.md is under 500 lines
- [ ] validate-skill.sh passes
