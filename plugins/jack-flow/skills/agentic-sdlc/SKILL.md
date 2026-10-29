---
name: agentic-sdlc
description: Attention-optimized agentic software delivery workflow - decompose work into small independently deployable PRs, run many concurrent agent workstreams against main, and prioritize the human review queue. Use when planning or orchestrating feature work, splitting tickets into PRs, running parallel agents/worktrees, managing a backlog of open PRs, or when the user mentions shipping velocity, PR throughput, review bottlenecks, or agentic SDLC.
---

# Agentic SDLC

Code generation is no longer the bottleneck — **human attention is**. Reviewing, giving feedback, and verifying are the scarce resources. Treat the developer like an I/O-bound resource in a high-throughput system: minimize cost per visit, never let them idle, and serve the highest-value work first.

Every decision below should answer: *does this reduce the attention required to ship, or keep attention busy on the most valuable thing?*

## 1. Small, independently deployable PRs

Each PR should demand one or two things of the reviewer to verify.

- **Scope**: one behavioral change per PR. Refactors, renames, dependency bumps, and feature logic go in separate PRs.
- **Independently deployable**: each PR is safe to merge and deploy alone. Use feature flags, additive schema changes (expand → migrate → contract), and dark-launched code paths so partial work can ship.
- **Target `main`, not each other.** Avoid stacked PRs — dependencies serialize the queue and kill concurrency. If B truly needs A, ship A first, or restructure so both are additive against main.
- **Size heuristic**: if the PR description needs more than ~3 bullets to explain what to verify, split it.
- **State the verification ask explicitly** in the PR description:

```markdown
## What changed
<one sentence>

## Verify
- [ ] <the one or two things a human must check>

## Deploy safety
<flag name / why it's safe to deploy alone / rollback plan>
```

When decomposing a ticket, output a list of PRs first, each with its single verify item and why it's independently deployable, before writing code.

## 2. Concurrent workstreams

Run many workstreams (target 10–20) so there is almost always one ready for human attention.

- **One agent per PR**, each with its own isolated context (separate worktree / branch / cloud session). Don't mix PRs in one context.
- **Orchestrator pattern**: a lead agent owns the ticket list (e.g. Linear/GitHub issues), delegates each PR to a subagent, and tracks status — it does not write the code itself.
- **Cycle, don't block**: when one workstream waits on human feedback or CI, move to the next. Never sit idle waiting on a single PR.
- **Status at a glance**: maintain a short board of workstreams with state: `drafting → needs-review → addressing-feedback → ready-to-merge → deployed → verified`.
- **Agent-readable signals**: route CI results and deploy/monitoring alerts somewhere agents can consume (a channel, a log, `gh` checks) so agents verify their own deploys without consuming human attention.

## 3. Priority queue for attention

Human attention flows to the highest-impact work first.

- Label every workstream with a priority (emoji or label, e.g. `🔥 P0`, `⭐ P1`, `· P2`). Keep the scheme tiny and consistent.
- When presenting items needing review, **sort by priority, then by smallest verification cost** — quick wins clear the queue.
- Low-priority work proceeds autonomously as far as possible and only surfaces when blocked or ready to merge.
- Batch trivial approvals; never interleave them ahead of high-priority reviews.

## Operating loop

1. **Intake**: pull tickets; decompose each into small, main-targeted, independently deployable PRs.
2. **Fan out**: assign each PR to its own agent/worktree with a priority label.
3. **Agents work autonomously**: implement, test, self-review, make CI green, write the verify checklist.
4. **Surface to human**: present the ready queue sorted by priority → verification cost, each with its one-line verify ask.
5. **Feedback**: route review comments back to the owning agent; human moves to the next item.
6. **Ship & verify**: merge, deploy, and have the agent confirm via monitoring signals; mark verified.
7. **Re-evaluate the bottleneck**: if the queue is consistently empty, increase concurrency; if reviews pile up, shrink PRs further or tighten automated verification.

## Anti-patterns

- Large "while I'm in here" PRs bundling unrelated changes.
- Stacked PR chains that force serial review.
- One long-lived agent context juggling multiple PRs.
- Asking the human to verify things CI or the agent could check.
- Measuring success by lines of code generated rather than PRs shipped *and verified* without regressions.
