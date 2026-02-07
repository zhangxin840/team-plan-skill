---
name: team-plan
description: "Scout-then-plan workflow for Agent Teams. Gathers context with subagents, then enters plan mode to design team strategy before execution."
argument-hint: <task-description>
---

**Task**: $ARGUMENTS

## Step 1: Scout (parallel subagents)

Before planning, launch subagents **in parallel** to gather context. Use as many as needed — multiple Explore agents for different code areas, multiple Researcher agents for different knowledge domains.

- **Explore agents**: Scan codebase for files, patterns, conventions, and dependencies relevant to the task. Each can focus on a different area.
- **Researcher agents**: Search web for official docs, changelog, known issues, best practices. Each researcher should first review existing project documentation to avoid duplicating known findings, then write results to a doc file.

Wait for all scouts to return.

## Step 2: Plan (EnterPlanMode)

Call `EnterPlanMode`. Based on scout findings, produce a plan that includes:

1. **Task Analysis** — what needs to be done, scope boundaries, risks
2. **Team Design** — for each teammate: name, role, agent type, model, owned files, and whether to require plan approval before they make changes (for high-risk teammates, spawn with "Require plan approval before making changes" and specify approval criteria the lead should use to judge)
3. **Task Breakdown** — specific tasks with owners, dependencies (`blockedBy`), and acceptance criteria. Aim for self-contained units (a function, a test file, a review) — not so small that coordination overhead dominates, not so large that teammates work too long without check-ins
4. **Phase Sequence** — which tasks run in parallel, which must wait
5. **File Ownership** — no two teammates write the same file
6. **Spawn Context** — for each teammate, note the key context to include in their spawn prompt (file paths, tech stack, output format, constraints) since teammates do NOT inherit the lead's conversation history

Call `ExitPlanMode` for user approval. After approval, execute the plan — use delegate mode (Shift+Tab) if the lead should coordinate only and not implement.
