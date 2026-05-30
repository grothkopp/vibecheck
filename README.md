# VibeCheck Skills

A set of slash commands / agent skills for auditing, understanding, and fixing vibe-coded (AI-assisted) codebases. Designed for non-technical or semi-technical project owners who want to understand what's going on under the hood.

## What's included

| File | Command | Purpose |
|------|---------|---------|
| `vibecheck_audit.md` | `/vibecheck-audit` | Full codebase audit — discovers and documents all issues |
| `vibecheck_fix.md` | `/vibecheck-fix` | Fixes one or more issues from the audit |
| `vibecheck_status.md` | `/vibecheck-status` | Dashboard of open/resolved issues |
| `vibecheck_explain.md` | `/vibecheck-explain` | Explains issues in plain language with options |

## Recommended workflow

```
1. /vibecheck-audit        → Discover and document all issues
2. /vibecheck-status       → See the overview and priorities
3. /vibecheck-explain      → Understand specific issues before deciding
4. /vibecheck-fix          → Fix issues (agent handles code, tells you about manual steps)
5. /vibecheck-status       → Confirm progress
```

Repeat steps 3-5 as needed until the codebase is in good shape.

## How it works

All findings are stored in a `vibecheck/` folder inside the audited project:

```
your-project/
├── vibecheck/
│   ├── README.md                  ← Executive summary
│   ├── Plan.md                    ← Prioritized fix list (the main tracker)
│   ├── overview.md                ← Codebase structure
│   ├── architecture.md            ← Architecture findings
│   ├── data-model.md              ← Data model findings
│   ├── security.md                ← Security findings
│   ├── scaling-multi-tenancy.md   ← Scaling findings
│   ├── tests.md                   ← Test coverage findings
│   ├── logging-observability.md   ← Logging/monitoring findings
│   └── deployment-backups.md      ← Deployment/backup findings
├── src/
├── ...
```

The `/vibecheck-fix` command updates these files as issues are resolved, so the `vibecheck/` folder always reflects current status.

## Installation

### Claude Code (`.claude/commands/`)

Copy the skill files into your project's `.claude/commands/` directory:

```bash
mkdir -p .claude/commands
cp vibecheck_audit.md .claude/commands/vibecheck-audit.md
cp vibecheck_fix.md .claude/commands/vibecheck-fix.md
cp vibecheck_status.md .claude/commands/vibecheck-status.md
cp vibecheck_explain.md .claude/commands/vibecheck-explain.md
```

Then use them in Claude Code:
```
/vibecheck-audit
/vibecheck-fix SEC-01
/vibecheck-status
/vibecheck-explain SEC-01
```

### Global Claude Code commands (`~/.claude/commands/`)

To make these available across all projects:

```bash
mkdir -p ~/.claude/commands
cp vibecheck_audit.md ~/.claude/commands/vibecheck-audit.md
cp vibecheck_fix.md ~/.claude/commands/vibecheck-fix.md
cp vibecheck_status.md ~/.claude/commands/vibecheck-status.md
cp vibecheck_explain.md ~/.claude/commands/vibecheck-explain.md
```

### Cursor (`.cursor/rules/` or as custom commands)

Copy into `.cursor/rules/` or configure as custom slash commands in Cursor settings:

```bash
mkdir -p .cursor/rules
cp vibecheck_audit.md .cursor/rules/vibecheck-audit.md
cp vibecheck_fix.md .cursor/rules/vibecheck-fix.md
cp vibecheck_status.md .cursor/rules/vibecheck-status.md
cp vibecheck_explain.md .cursor/rules/vibecheck-explain.md
```

### Windsurf / Codeium (`.windsurfrules` or cascade commands)

Place in your project root or configure as cascade rules:

```bash
mkdir -p .windsurf/commands
cp vibecheck_audit.md .windsurf/commands/vibecheck-audit.md
cp vibecheck_fix.md .windsurf/commands/vibecheck-fix.md
cp vibecheck_status.md .windsurf/commands/vibecheck-status.md
cp vibecheck_explain.md .windsurf/commands/vibecheck-explain.md
```

### Generic agent setup (`.agent/skills/` or similar)

Most agent frameworks support custom skill/prompt directories:

```bash
mkdir -p .agent/skills
cp vibecheck_audit.md .agent/skills/vibecheck-audit.md
cp vibecheck_fix.md .agent/skills/vibecheck-fix.md
cp vibecheck_status.md .agent/skills/vibecheck-status.md
cp vibecheck_explain.md .agent/skills/vibecheck-explain.md
```

### Aider (conventions or prompt files)

Add as convention files or reference in `.aider.conf.yml`:

```bash
mkdir -p .aider/prompts
cp vibecheck_audit.md .aider/prompts/vibecheck-audit.md
cp vibecheck_fix.md .aider/prompts/vibecheck-fix.md
cp vibecheck_status.md .aider/prompts/vibecheck-status.md
cp vibecheck_explain.md .aider/prompts/vibecheck-explain.md
```

## Usage examples

### Run a full audit
```
/vibecheck-audit
```
The agent will inspect your codebase, ask you questions about hosting/backups/monitoring, and produce a complete audit in `vibecheck/`.

### Check current status
```
/vibecheck-status
```
Shows how many issues are open, resolved, or waiting for manual action.

### Understand an issue
```
/vibecheck-explain SEC-01
/vibecheck-explain "the login problem"
/vibecheck-explain all critical
```
Gets a plain-language explanation with fix options and trade-offs.

### Fix issues
```
/vibecheck-fix                  → Fix all open items in priority order
/vibecheck-fix SEC-01           → Fix a specific finding
/vibecheck-fix security         → Fix all security findings
```

## Design principles

- **Non-technical first**: All explanations and questions use plain language.
- **Evidence-based**: Findings cite actual code, config, or confirmed user input.
- **Trackable**: The `vibecheck/` folder is a living document — always up to date.
- **Agent-friendly**: Fix prompts are designed to be copy-pasted into any AI coding agent.
- **Human-aware**: Issues that need manual action are clearly separated from code fixes.

## Requirements

- An AI coding agent that supports slash commands or custom prompts
- A codebase to audit (works with any language/framework)
- The agent needs read access to the full repository

## FAQ

**Q: Can I commit the `vibecheck/` folder?**
A: Yes! It's designed to be committed. It serves as documentation and a tracking tool for the team.

**Q: What if I disagree with a finding?**
A: Mark it with `Status: Won't fix` and add a note explaining why. The status report will skip it.

**Q: Can I re-run the audit after fixing things?**
A: Yes. A re-audit will detect what's been fixed and may find new issues. The existing `vibecheck/` folder will be updated.

**Q: Does this work with monorepos?**
A: Run the audit from the specific package/app directory you want to check. Each gets its own `vibecheck/` folder.
