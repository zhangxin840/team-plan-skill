---
name: team-plan
description: "Scout-then-plan workflow for Agent Teams. Gathers context with subagents, then enters plan mode to design team strategy before execution."
argument-hint: <task-description>
---

**Task**: $ARGUMENTS

## Step 1: Scout (parallel subagents)

Launch subagents **in parallel** to gather context:

- **Explore agents**: Scan codebase for relevant files, patterns, conventions, dependencies. Each focuses on a different area.
- **Researcher agents**: Search web for docs, changelog, known issues. Check existing project docs first to avoid duplication, write results to a doc file.

Wait for all scouts to return.

## Step 2: Plan (EnterPlanMode)

Call `EnterPlanMode`. Based on scout findings, produce a plan:

1. **Task Analysis** — scope, boundaries, risks
2. **Team Design** — each teammate: name, role, agent type, model, owned files. High-risk teammates require plan approval before changes. Overlapping files → use `isolation: worktree`
3. **Task Breakdown** — tasks with owners, `blockedBy` dependencies, acceptance criteria. Size: self-contained units, not too small (coordination overhead) or too large (no check-ins)
4. **Phase Sequence** — what runs in parallel, what must wait
5. **Isolation Strategy** — default: no two teammates write the same file. Same-file edits → `isolation: worktree` + branch merge order
6. **Spawn Context** — key context per teammate's prompt (file paths, tech stack, constraints). Teammates do NOT inherit lead's history

Call `ExitPlanMode` for approval. Then execute — use delegate mode (Shift+Tab) if lead should coordinate only.
