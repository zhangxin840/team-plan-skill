# team-plan

One command to go from task description to a coordinated [Agent Team](https://code.claude.com/docs/en/agent-teams).

Agent Teams are powerful but using them well requires thought — team composition, task dependencies, file ownership, spawn context, plan approval. This skill wraps the [official best practices](https://code.claude.com/docs/en/agent-teams#best-practices) into a repeatable workflow so you get a well-structured team every time.

```
/team-plan <task>
```

```
Scout (subagents)  →  Plan (plan mode)  →  You approve  →  Agent Team executes
```

## Why

Without this skill, using Agent Teams typically means:
- Telling Claude to "create a team" and hoping it designs a good one
- Teammates that lack context because they don't inherit the lead's history
- File conflicts when two teammates edit the same file
- No review gate before high-risk changes

This skill fixes that by requiring a structured plan **before** any team is spawned:

- **Scout first** — Subagents explore the codebase and research external knowledge in parallel, so the plan is grounded in facts, not assumptions
- **Plan with team design** — The plan explicitly defines roles, models, file ownership, task dependencies, and spawn context for each teammate
- **Plan approval for high-risk teammates** — Teammates making dangerous changes must get their approach approved before writing code ([official docs](https://code.claude.com/docs/en/agent-teams#require-plan-approval-for-teammates))
- **No file conflicts** — The plan enforces that no two teammates write the same file ([official best practice](https://code.claude.com/docs/en/agent-teams#best-practices))
- **Spawn context** — Each teammate's prompt is planned with the specific context they need, because they don't inherit the lead's conversation history
- **You approve before execution** — Nothing runs until you review the plan

## Install

### Plugin Marketplace (recommended)

```
/plugin marketplace add zhangxin840/team-plan-skill
/plugin install team-plan@team-plan-skills
```

### npx

```bash
npx skills add zhangxin840/team-plan-skill
```

### Manual

```bash
git clone https://github.com/zhangxin840/team-plan-skill.git
cp -r team-plan-skill/plugins/team-plan/skills/team-plan ~/.claude/skills/
```

## Usage

```
/team-plan Refactor src/auth/ from session-based to JWT authentication
```

```
/team-plan Research Claude Code v2.1.35 changes, produce upgrade report and articles
```

```
/team-plan Review and update all API documentation to match current implementation
```

## What the plan covers

| Section | Purpose |
|---------|---------|
| **Task Analysis** | Scope, boundaries, risks — refined from scout findings |
| **Team Design** | Each teammate: name, role, agent type, model, owned files, plan approval requirements |
| **Task Breakdown** | Tasks with owners, `blockedBy` dependencies, acceptance criteria |
| **Phase Sequence** | What runs in parallel, what must wait |
| **File Ownership** | Explicit map — no two teammates write the same file |
| **Spawn Context** | Key context for each teammate's prompt |

## Prerequisites

Agent Teams must be enabled in `~/.claude/settings.json` or `.claude/settings.json`:

```json
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```

## Design

- **Minimal skill, maximum autonomy** — 30 lines of instructions. Only enforces scout → plan → approve. Execution is left entirely to Claude.
- **Works in any project** — No hardcoded paths. Agents discover structure from CLAUDE.md and project conventions.
- **Built on official best practices** — Plan approval, file ownership, spawn context, task sizing — all from the [Agent Teams docs](https://code.claude.com/docs/en/agent-teams#best-practices).

## License

MIT
