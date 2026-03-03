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

## GitHub Issue Format

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
