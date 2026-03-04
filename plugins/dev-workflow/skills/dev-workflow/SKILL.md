---
name: dev-workflow
description: Full development workflow orchestration. Runs the complete plan → review → implement → code-review → retro pipeline. Invoke with /dev-workflow to start a new feature implementation session.
disable-model-invocation: true
argument-hint: "[feature description or GitHub issue URL]"
---

# Dev Workflow

Orchestrates the complete development workflow from requirements to merged PR with two human touchpoints.

## Prerequisites

- `gh` CLI is installed and authenticated
- You are in a git repository with a clean working branch

## Pipeline

Follow these steps in order. Do not skip steps. Pause at human touchpoints marked ←.

> **Routing rule**: Always enter at Step 1, even if the user provides a complete plan, so it is reviewed and approved before implementation begins. If the user provides a plan as chat text rather than a GitHub issue URL, create a GitHub issue from it first.

---

### Step 0 — Engineering Standards

Invoke the `engineering-standards` skill. This loads the foundational principles — simplicity-first, TDD, hexagonal architecture, SRP/DRY/KISS/YAGNI — that guide every decision in this pipeline.

---

### Step 1 — Plan

Produce a detailed implementation plan from `$ARGUMENTS`:

- One-sentence goal
- Explicit scope (what is and is NOT being built)
- Complete interface contracts for all new functions, APIs, and data models
- Specific, testable acceptance criteria
- All failure modes and error types
- Dependencies and risks

Do not proceed to Step 2 until the plan contains explicit interface contracts.

---

### Step 2 — Plan Review

Invoke the `plan-reviewer` skill with the draft plan. It returns an amended plan + review summary + verdict.

---

### Step 3 — Human Approval ← TOUCHPOINT

Present the amended plan and review summary. Ask the user to approve before proceeding.

If the user requests changes, apply them and return to Step 2.

---

### Step 4 — Implementation

Invoke the `implementation-agent` with the approved plan. Pass the full plan text:

```
Use the implementation-agent to implement this plan:

[paste the full approved plan here]

Existing branch (if any): [branch name or NONE]
Existing PR (if any): #[PR number or NONE]
```

Wait for the PR URL before proceeding.

---

### Step 5 — Code Review

Invoke the `code-reviewer` skill with the PR number and issue number.

---

### Step 6 — Human Decision ← TOUCHPOINT

Present the review findings and verdict. Ask the user whether to merge or address required changes.

If changes are needed, address them and return to Step 4.

---

### Step 7 — Retro

Invoke the `retro-agent` skill. This runs without human input and files improvement issues in the marketplace.
