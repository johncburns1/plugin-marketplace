# Agentic SDLC

An attention-optimized software delivery workflow for running many concurrent agent workstreams. Code generation is no longer the bottleneck — human attention is — so this skill structures work to minimize the attention needed to ship and keep it focused on the most valuable work.

## What it does

- **Small, independently deployable PRs** — one behavioral change per PR, targeting `main` (no stacked PRs), with an explicit "Verify" checklist and deploy-safety notes in every description.
- **Concurrent workstreams** — one agent per PR in an isolated worktree/branch/session, coordinated by an orchestrator that tracks each workstream's state (`drafting → needs-review → addressing-feedback → ready-to-merge → deployed → verified`).
- **Priority queue for attention** — items needing review are surfaced sorted by priority, then by smallest verification cost.

## When it activates

Claude uses this skill when planning or orchestrating feature work, splitting tickets into PRs, running parallel agents or worktrees, managing a backlog of open PRs, or when you mention shipping velocity, PR throughput, review bottlenecks, or agentic SDLC.

## Example prompts

- "Break this ticket into PRs I can ship independently."
- "I have 12 open PRs — what should I review first?"
- "Fan this epic out across parallel agents."
