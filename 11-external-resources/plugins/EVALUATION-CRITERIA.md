# Plugin Evaluation Criteria

A framework for assessing Claude Code plugins before adoption. Use this when evaluating new plugins for the registry or personal use.

**Last Updated**: January 9, 2026

---

## Quick Assessment Checklist

For rapid evaluation, check these in order:

- [ ] **Recency**: Updated in last 30 days?
- [ ] **Activity**: Multiple contributors? Open issues being addressed?
- [ ] **Stars**: Community validation (>100 is good signal, >1000 is strong)
- [ ] **Code Review**: Source code readable and not obfuscated?
- [ ] **Permissions**: What does it access? (filesystem, network, credentials)
- [ ] **Dependencies**: External requirements reasonable?

---

## Detailed Evaluation Framework

### 1. Recency & Maintenance

The Claude Code ecosystem moves fast. A plugin from 2 months ago may be outdated.

| Signal | Good | Concerning |
|--------|------|------------|
| Last commit | < 30 days | > 90 days |
| Release frequency | Regular releases | No releases in 60+ days |
| Issue response | Maintainer responds | Issues ignored |
| PR activity | PRs merged | PRs stale |
| Claude Code version | Targets v2.1.0+ | Targets < v2.0 |

**Questions to ask**:
- When was the last meaningful update?
- Does it target recent Claude Code features (LSP, hot reload, etc.)?
- Are there unaddressed breaking changes from recent Claude Code updates?

---

### 2. Community Adoption

Stars and forks indicate community validation, but context matters.

| Metric | Interpretation |
|--------|----------------|
| **Stars** | General interest; >1000 = strong signal |
| **Forks** | Active development/customization |
| **Used by** | Real-world adoption |
| **Issues/PRs** | Engagement level |

**Caveats**:
- New plugins won't have high stars yet
- Stars can be inflated by social media buzz
- Some excellent niche plugins have low stars

**Questions to ask**:
- Is there evidence of real-world usage?
- Are issues constructive (feature requests, bug reports) or complaints?
- Who are the contributors? (Recognized community members?)

---

### 3. Code Quality

Review the source before installing anything that runs on your machine.

| Check | What to Look For |
|-------|------------------|
| **Readability** | Clear, well-structured code |
| **Obfuscation** | Minified or obfuscated code is a red flag |
| **Dependencies** | Minimal, well-known packages |
| **Error handling** | Graceful failures, not silent crashes |
| **Tests** | Test coverage indicates maturity |
| **Documentation** | README explains what it does and how |

**Red Flags**:
- Minified JavaScript in source (not just dist/)
- Excessive dependencies for simple functionality
- No README or documentation
- Disabled linting/type checking
- `eval()`, `exec()`, or dynamic code execution without clear purpose

---

### 4. Security Considerations

Plugins can access your filesystem, network, and potentially credentials.

#### Access Assessment

| Access Type | Risk Level | Questions |
|-------------|------------|-----------|
| **Filesystem read** | Low-Medium | What files does it read? Why? |
| **Filesystem write** | Medium | Where does it write? Is it scoped? |
| **Network calls** | Medium-High | What endpoints? Is data sent externally? |
| **Credential access** | High | Does it read `~/.claude/.credentials.json`? Why? |
| **Shell execution** | High | What commands does it run? |

#### Common Legitimate Access Patterns

| Access | Legitimate Use |
|--------|---------------|
| Read project files | Code analysis, search |
| Write to `~/.claude/plugins/` | Plugin cache/config |
| Network to api.anthropic.com | Usage data, official APIs |
| Git commands | Workflow automation |

#### Concerning Patterns

| Pattern | Concern |
|---------|---------|
| Writes outside `~/.claude/` or project | Unexpected side effects |
| Network to unknown endpoints | Data exfiltration risk |
| Reads credentials without clear purpose | Credential theft risk |
| Runs arbitrary user input as shell commands | Command injection |

---

### 5. Functionality Assessment

Does the plugin actually do what it claims?

| Question | How to Verify |
|----------|---------------|
| Does it solve a real problem? | Is the use case clear and valuable? |
| Does it duplicate built-in features? | Check Claude Code docs first |
| What's the overhead? | Latency, memory, API calls? |
| Is it configurable? | Can you adjust behavior? |
| What happens on failure? | Graceful degradation? |

---

### 6. Compatibility Check

| Factor | What to Check |
|--------|---------------|
| **Claude Code version** | Does it require minimum version? |
| **OS compatibility** | macOS, Linux, Windows support? |
| **Runtime requirements** | Node.js version? Other dependencies? |
| **Conflicts** | Does it conflict with other plugins? |

---

## Risk Levels by Plugin Type

| Plugin Type | Risk Level | Reason |
|-------------|------------|--------|
| **Statusline/HUD** | Low | Read-only, display only |
| **Skills (knowledge)** | Low | Just prompt content |
| **Commands (prompts)** | Low | Just structured prompts |
| **LSP configurations** | Low-Medium | Configures external binaries |
| **Agents** | Medium | May invoke tools autonomously |
| **Hooks** | Medium-High | Run shell commands on events |
| **MCP servers** | Medium-High | External process with capabilities |
| **Background workers** | High | Persistent processes |

---

## Evaluation Template

Use this template when documenting plugin evaluation:

```markdown
## Plugin: [name]

**Evaluated**: [date]
**Version**: [version]
**GitHub**: [url]

### Summary
[1-2 sentence description]

### Scores

| Criteria | Score (1-5) | Notes |
|----------|-------------|-------|
| Recency | | |
| Community | | |
| Code Quality | | |
| Security | | |
| Functionality | | |
| Compatibility | | |

### Access Requirements
- Filesystem: [what it reads/writes]
- Network: [endpoints]
- Credentials: [yes/no, why]
- Shell: [commands it runs]

### Pros
- [Pro 1]
- [Pro 2]

### Cons
- [Con 1]
- [Con 2]

### Recommendation
[Suitable for: specific use cases]
[Not suitable for: scenarios to avoid]
```

---

## Decision Framework

After evaluation, categorize the plugin:

| Category | Criteria | Action |
|----------|----------|--------|
| **Safe to use** | High marks across all criteria, clear purpose, minimal access | Document in REGISTRY.md |
| **Use with caution** | Some concerns but manageable, clear value | Document with caveats |
| **Needs more research** | Insufficient information to decide | Add to CONSIDERATIONS.md |
| **Avoid** | Security concerns, abandoned, or poor quality | Document why (helps others) |

---

## Common Pitfalls

### Don't assume stars = quality
A viral tweet can inflate stars. Look at the actual code.

### Don't ignore recency
A 3-month-old plugin may not work with current Claude Code.

### Don't skip the code review
Even popular plugins can have security issues.

### Don't install everything
Each plugin adds complexity. Be selective.

### Don't trust the README alone
Verify claims by checking the code.

---

## Revision History

| Date | Change |
|------|--------|
| 2026-01-09 | Initial criteria framework |
