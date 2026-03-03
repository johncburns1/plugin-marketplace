---
name: retro
description: Development session retrospective. Analyze what went well and what did not across all workflow steps, then create a GitHub issue in the plugin marketplace with specific, actionable improvement suggestions. Used by the retro-agent.
user-invocable: false
---

# Session Retrospective

Analyze the completed development session and surface improvements for the workflow and marketplace skills.

## Core Principle

**Every session is data.** Friction in the workflow means specific skills, agents, or plan formats need improvement. Capture it precisely while it is fresh.

## Artifacts to Gather

Before analyzing, collect:
- The original GitHub issue (approved plan)
- Plan review summary (amendments made and their rationale)
- PR: commits, diff size, divergence from plan
- Validation report: failure count and categories
- Code review report: required vs. optional changes
- Skill invocation trace: which skills and agents were invoked and in what order (captures skill routing errors — e.g., pipeline entered at wrong step, `engineering-standards` never loaded)

## Source Repository Detection

Before analyzing, detect which repository is the subject of this dev session:

```bash
gh repo view --json nameWithOwner -q '.nameWithOwner'
```

- If the output is `johncburns1/plugin-marketplace`, the session was a marketplace-internal change. Skip source repo issue creation.
- If the output is any other value (e.g., `acme-corp/my-service`), that is the **source repository**. Hold this value — you will use it in the Finding Categorization and Issue Creation sections below.

Substitute the detected value directly into the `--repo` flag when constructing the source repo issue command. Do not rely on a shell variable being set in the environment.

## Analysis Framework

Evaluate each step for friction and success signals:

### Step 1 — Planning
- How far was the initial draft from the final approved plan?
- What categories of gaps did the plan reviewer catch?
- Could a better planning skill or template have surfaced these earlier?

### Step 2 — Plan Review
- How many amendments were needed?
- Were any issues missed that only surfaced in validation or code review?
- Was the review appropriately critical, or too lenient/too strict?

### Step 3 — Implementation (TDD cycle)
- How many test/implement iterations were needed before all gates passed?
- Were there failures caused by plan ambiguity rather than implementation error?
- What assumptions did the agent have to make that weren't in the plan?
- Did the tests adequately cover the acceptance criteria and failure modes?

### Step 4 — Code Review
- How many required changes were flagged?
- Were any preventable by better validation or a more precise plan?
- Was scope creep detected (implementation built things not in plan)?

## Improvement Targeting

Match each finding to a specific, named skill or agent in the marketplace:

| Finding | Target |
|---|---|
| Plan was missing contract detail | `plan-review` skill (interface contracts section) |
| Plan reviewer missed something | `plan-reviewer` agent checklist |
| Tests didn't cover an error case | `implementation-agent` system prompt (red-before-green section) |
| Implementation deviated from plan | `implementation-agent` system prompt (contracts are law section) |
| TDD cycle needed many iterations | `implementation-agent` or `plan-review` skill (ambiguous contracts) |
| Code review found quality issues | `code-review` skill or `engineering-standards` |
| Retro was missing context | `retro` skill (artifacts to gather section) |
| `engineering-standards` skill was never invoked | `dev-workflow` skill (add explicit invocation before Step 1) |

Vague feedback ("the plan could be better") is not actionable. Specific feedback ("the plan-review skill should require explicit error type enumeration — three sessions have had ConflictError missing from plans") is.

## Finding Categorization

Before creating issues, classify every finding into one of two buckets:

### Marketplace / Workflow Findings

Route to `johncburns1/plugin-marketplace`. A finding is marketplace-targeted when it relates to:
- A named skill or agent in this marketplace needing a change
- The dev-workflow pipeline steps (planning, review, implementation, retro)
- Gaps in how Claude was prompted or instructed

Use the Improvement Targeting table above to map these to specific skills and agents.

### Source Repository Findings

Route to the detected source repository. A finding is source-repo-targeted when it is a concrete gap in the *project being built*, not in the workflow used to build it:

| Category | Examples |
|---|---|
| Documentation | README gaps, missing API docs, undocumented public interfaces |
| Validation & error handling | No input validation at boundaries, unhandled edge cases visible in tests |
| Test infrastructure | Missing fixtures/utilities, no coverage enforcement, no integration test harness |
| Observability | No structured logging, missing tracing, absent metrics instrumentation |
| CI/CD | Missing lint/type-check gates, no coverage threshold, missing PR validation |
| Code patterns | Repeated workarounds, unclear module boundaries, architectural inconsistencies |

A finding may fit both buckets. If so, create entries in both issues with distinct framings: the marketplace issue gets a workflow improvement suggestion; the source repo issue gets a project improvement suggestion.

**Do not create a source repo issue if:**
- The detected repo equals `johncburns1/plugin-marketplace`
- No findings fall into the source repository categories above

## GitHub Issue Format

### Marketplace Issue (always create)

```bash
gh issue create \
  --repo johncburns1/plugin-marketplace \
  --title "Workflow Retro: [one-line session description]" \
  --label "retro,workflow-improvement" \
  --body "$(cat <<'EOF'
## Session Retro

**Session**: [brief description of what was built]
**Date**: [YYYY-MM-DD]

## What Went Well
- [specific thing that worked smoothly]

## What Didn't Go Well
- [specific friction point and which step it occurred in]

## Root Causes
- [why each friction point occurred]

## Improvement Suggestions

### Skill/Agent Changes
| Target | Suggested Change | Evidence from Session |
|--------|-----------------|----------------------|
| [skill/agent name] | [specific change] | [what happened] |

### Workflow Changes
| Step | Suggested Change | Evidence from Session |
|------|-----------------|----------------------|
| [step N] | [specific change] | [what happened] |

## Session Metrics
- Plan amendments in review: [N]
- Validation cycles needed: [N]
- Required code review changes: [N]
- Optional suggestions: [N]
EOF
)"
```

### Source Repository Issue (conditional)

Create only when: the detected source repo differs from `johncburns1/plugin-marketplace` AND at least one source-repo finding exists. Substitute the actual repo name into `--repo` (e.g., `--repo acme-corp/my-service`). Omit any section below where no findings exist for this session.

```bash
gh issue create \
  --repo [detected-source-repo] \
  --title "Engineering Improvements: [one-line description of the session's feature area]" \
  --label "engineering,tech-debt" \
  --body "$(cat <<'EOF'
## Engineering Improvements

**Context**: Identified during a dev session implementing [brief feature description] on [YYYY-MM-DD].
**Source**: Automated retrospective from the dev-workflow agent pipeline.

These are concrete improvements to consider for this project, surfaced during implementation. They are not blockers.

## Documentation
- [specific gap and where it should be added]

## Validation & Error Handling
- [specific boundary and what validation is missing]

## Test Infrastructure
- [specific missing tooling, fixture, or coverage mechanism]

## Observability
- [specific absent logging, tracing, or metric]

## CI/CD
- [specific missing gate or check]

## Code Patterns
- [friction pattern, which module, why it caused friction]
EOF
)"
```
