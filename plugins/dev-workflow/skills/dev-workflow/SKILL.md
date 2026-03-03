---
name: dev-workflow
description: Full development workflow orchestration. Runs the complete plan → review → implement → code-review → retro pipeline. Invoke with /dev-workflow to start a new feature implementation session.
disable-model-invocation: true
argument-hint: "[feature description or GitHub issue URL]"
---

# Dev Workflow

Orchestrates the complete development workflow from requirements to merged PR with two human touchpoints.

## Prerequisites

Before starting:
- `gh` CLI is installed and authenticated
- You are in a git repository with a clean working branch
- You have a clear feature requirement to implement
- The `engineering-standards` skill is available — it provides the foundational principles (simplicity-first, TDD, hexagonal architecture, SRP/DRY/KISS/YAGNI) that every agent in this pipeline applies

## Pipeline

Follow these steps in order. Do not skip steps. Pause at each human touchpoint marked with ←.

---

### Step 1 — Plan

Produce a detailed implementation plan from `$ARGUMENTS` directly:

The plan must produce:
- A one-sentence goal
- Explicit scope (what is and is NOT being built)
- Complete interface contracts for all new functions, APIs, and data models
- Acceptance criteria (specific and testable, not vague)
- All failure modes and error types
- Dependencies and risks

Do not proceed to step 2 until the plan contains explicit interface contracts. Vague plans produce bad implementations.

---

### Step 2 — Plan Review

Invoke the `plan-reviewer` agent with the draft plan:

```
Use the plan-reviewer agent to critically review this plan and amend it as needed.
```

The agent will check completeness, contract precision, and simplicity, then return an amended plan + review summary + verdict.

---

### Step 3 — Human Approval ← TOUCHPOINT

Present the amended plan to the user:

```
## Plan Ready for Approval

[amended plan]

---
## Review Summary
[what the plan-reviewer changed and why]

## Verdict: [APPROVED / NEEDS REVISION]

Do you approve this plan? Once approved, I will create a GitHub issue and begin implementation.
```

- If the user requests changes, apply them and return to step 2
- If approved, create a GitHub issue:

```bash
gh issue create \
  --title "[Feature]: [brief description]" \
  --label "dev-workflow,implementing" \
  --body "[full amended plan]"
```

Note the issue number — the implementation agent references it.

---

### Step 4 — Implementation

Invoke the `implementation-agent` with the GitHub issue number:

```
Use the implementation-agent to implement GitHub issue #[N].
```

The agent runs the full TDD cycle in an isolated worktree: reads the issue, writes failing tests, implements to pass, iterates until all gates pass (tests, lint, coverage), then opens a PR. Wait for the PR URL before proceeding.

---

### Step 5 — Code Review

Invoke the `code-reviewer` agent:

```
Use the code-reviewer agent to review the PR against the plan in GitHub issue #[N].
```

The agent returns a structured review: plan alignment, code quality, test quality, security, and verdict.

---

### Step 6 — Human Decision ← TOUCHPOINT

Present the code review to the user:

```
## Code Review Complete

[full review findings]

## Verdict: [APPROVE / REQUEST CHANGES]

Do you want to merge this PR, or should I address the required changes first?
```

- If the user approves: merge the PR
- If the user requests changes: address required changes, re-run implementation (step 4), then re-run code review (step 5)

---

### Step 7 — Retro

After the merge decision, invoke the retro agent autonomously:

```
Use the retro-agent to conduct a session retrospective and create an improvement
issue in johncburns1/plugin-marketplace.
```

This step runs without human input. The retro agent analyzes the session and files a GitHub issue with improvement suggestions for the marketplace.
