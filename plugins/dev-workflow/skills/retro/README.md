# Retro

**Version**: 1.0.0
**Last Updated**: 2026-03-03

## Description

Development session retrospective skill. Analyzes the completed session across all workflow steps and creates improvement issues in the appropriate repositories: a workflow retro issue in the plugin marketplace, and — when the session targeted a different source repository — a focused engineering improvements issue in that source repo.

## When to Use

Used by the `retro-agent` in step 6 of the dev workflow. Runs autonomously after every session — no human input required.

## Source Repository Detection

The skill detects which repository was being worked on by running:

```bash
gh repo view --json nameWithOwner -q '.nameWithOwner'
```

If the result differs from `johncburns1/plugin-marketplace`, that repo receives a focused engineering improvements issue for any source-repo findings. If the session was marketplace-internal, only the marketplace retro issue is created.

## Output

Up to two GitHub issues:

**1. Marketplace retro issue** (always created) in `johncburns1/plugin-marketplace`:
- What went well / what didn't
- Root causes of friction
- Specific improvement suggestions targeted at named skills and agents
- Session metrics (plan amendments, validation cycles, code review findings)

**2. Source repository engineering issue** (created when the session targeted a different repo AND source-repo findings exist):
- Missing or insufficient documentation
- Missing validation layers and error handling patterns
- Test infrastructure gaps
- Observability gaps (logging, tracing, metrics)
- CI/CD improvement suggestions
- Code patterns that caused implementation friction

## Continuous Improvement Loop

Each session's retro feeds back into the marketplace, improving the skills and agents for the next session.
