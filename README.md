# team-plan

A Claude Code skill that enforces a **scout → plan → execute** workflow for [Agent Teams](https://code.claude.com/docs/en/agent-teams).

```
Scout (subagents)  →  Plan (plan mode)  →  User approval  →  Execute (agent team)
```

1. **Scout** — Launches Explore and Researcher subagents in parallel to gather codebase context and external knowledge
2. **Plan** — Enters plan mode and produces a plan with team design, task breakdown, dependencies, and file ownership
3. **Execute** — After user approves the plan, launches the Agent Team

## Install

### Plugin Marketplace (recommended)

```
/plugin marketplace add zhangxin840/team-plan-skill
/plugin install team-plan@team-plan-skills
```

### Manual

```bash
git clone https://github.com/zhangxin840/team-plan-skill.git
cp -r team-plan-skill/plugins/team-plan/skills/team-plan ~/.claude/skills/
```

### npx

```bash
npx add-skill zhangxin840/team-plan-skill --skill team-plan
```

## Usage

```
/team-plan <task-description>
```

### Examples

```
/team-plan Refactor src/auth/ from session-based to JWT authentication
```

```
/team-plan Research Claude Code v2.1.35 changes, produce upgrade report and articles
```

```
/team-plan Review and update all API documentation to match current implementation
```

## What the plan includes

The skill requires the plan to cover:

- **Task Analysis** — scope, boundaries, risks
- **Team Design** — roles, models, file ownership, plan approval for high-risk teammates
- **Task Breakdown** — tasks with owners, dependencies, acceptance criteria
- **Phase Sequence** — parallel vs sequential execution
- **File Ownership** — no two teammates write the same file
- **Spawn Context** — key context for each teammate's prompt (they don't inherit the lead's history)

## Prerequisites

Agent Teams must be enabled:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

Add this to `~/.claude/settings.json` (user level) or `.claude/settings.json` (project level).

## Design choices

- **Minimal** — Only enforces the scout-plan-approve flow. No execution instructions; Claude handles that.
- **No hardcoded paths** — Works in any project. Agents discover structure from CLAUDE.md and conventions.
- **Plan approval** — High-risk teammates can require plan approval before modifying code.
- **Spawn context** — Each teammate's prompt is planned explicitly since they don't inherit the lead's conversation.

## License

MIT
